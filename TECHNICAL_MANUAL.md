# 🔧 IPH Real Time Tracker - Technical Manual

### Amazon KYC · VRMO SJO · vssalama@

## Table of Contents

- \[Architecture Overview\](#1-architecture-overview)
- \[File Responsibilities\](#2-file-responsibilities)
- \[Boot Sequence\](#3-boot-sequence)
- \[Storage Layer\](#4-storage-layer)
- \[Main Timer System\](#5-main-timer-system)
- \[Sub-Task Independent Timer System\](#6-sub-task-independent-timer-system)
- \[IPH Engine\](#7-iph-engine)
- \[Color System\](#8-color-system)
- \[Context & Queue Logic\](#9-context--queue-logic)
- \[Registry System\](#10-registry-system)
- \[Suggestion Engine\](#11-suggestion-engine)
- \[Guided Tour System\](#12-guided-tour-system)
- \[Backup System\](#13-backup-system)
- \[Obfuscation Pipeline\](#14-obfuscation-pipeline)
- \[Data Schemas\](#15-data-schemas)
- \[Key Constants Reference\](#16-key-constants-reference)
- \[Known Constraints\](#17-known-constraints)

## 1\. Architecture Overview

The application is a **single-page, zero-dependency, client-side web app**. It uses:

- **Vanilla JavaScript (ES6+)** - no frameworks, no bundler
- **localStorage** - primary data store (synchronous)
- **IndexedDB** - secondary mirror (async, survives partial browser clears)
- **CSS Custom Properties** - theming (dark/light)
- **requestAnimationFrame** - guide animations
- **setInterval** - timer display updates (both main and sub-task timers)

The entire JS is wrapped in a single DOMContentLoaded listener via \_initApp(), called asynchronously from \_bootApp() which first awaits an IndexedDB restore check.

## 2\. File Responsibilities

| **File**                     | **Role**                                                                                    |
| ---------------------------- | ------------------------------------------------------------------------------------------- |
| \`index.html\`               | Static structure - timers, panels, modals, overlays, registry title bar                     |
| \`javascript.js\`            | All logic - main timers, sub-task timers, IPH, registry, guide, suggestions, backup, export |
| \`javascript_obfuscated.js\` | Production build - identical behavior, source unreadable                                    |
| \`styles.css\`               | All visual styling - dark/light theme via \`\[data-theme\]\` attribute                      |

## 3\. Boot Sequence

Script loads (end of &lt;body&gt;)  
│  
├─ Storage Layer initialised (localStorage.setItem override, IDB open)  
│  
└─ \_bootApp() \[async\]  
│  
├─ await \_idbRestoreIfNeeded()  
│ └─ If localStorage empty but IDB has data → restore silently  
│  
└─ \_initApp()  
├─ Theme toggle init  
├─ softReset IIFE → clear in-memory timer state only  
├─ Notification.requestPermission() \[fire-and-forget\]  
├─ setupToggleGroups()  
├─ setupValidations()  
├─ setupIndependentTimerHandlers()  
├─ updateSellerWalletUI() / updateSnowBallUI()  
├─ updateSubtaskButtonAvailability()  
├─ startSuggestionEngine()  
├─ Checkbox change listeners  
├─ updateCasesCount() / restoreCheckboxState() / refreshIPHBar()  
├─ Restore session blocks + entries from localStorage → DOM  
├─ Restore running main timers (reads storageKey timestamps)  
├─ Restore running sub-task timers (reads st\_{VALUE}\_start keys)  
└─ showGuideHint() \[if no entries\]

## 4\. Storage Layer

### localStorage Keys

| **Key**                       | **Type**       | **Description**              |                     |
| ----------------------------- | -------------- | ---------------------------- | ------------------- |
| \`registryEntries\`           | JSON array     | All recorded entries         |                     |
| \`sessionBlocks\`             | JSON array     | All session block metadata   |                     |
| \`dayBanners\`                | JSON object    | Date keys with banner flags  |                     |
| \`casesCount\`                | number string  | Total resolved cases         |                     |
| \`entryCount\`                | number string  | Total individual entries     |                     |
| \`checkboxState\`             | JSON array     | Queue/skill checkbox state   |                     |
| \`sellerWalletActive\`        | boolean string | SW toggle state              |                     |
| \`snowBallActive\`            | boolean string | SB toggle state              |                     |
| \`theme\`                     | \`'dark'\` \\  | \`'light'\`                  | UI theme preference |
| \`acctSumTimerStart\`         | timestamp      | Running Acct Sum start time  |                     |
| \`ctcTimerStart\`             | timestamp      | Running CTC start time       |                     |
| \`invCrtTimerStart\`          | timestamp      | Running Inv-Crt start time   |                     |
| \`intEscTimerStart\`          | timestamp      | Running Int-Esc start time   |                     |
| \`dayBanner-{dateKey}-label\` | string         | Formatted date label per day |                     |

_Sub-task timer start times are persisted to localStorage under keys \`st_{VALUE}\_start\` (e.g. \`st*IDV_start\`) - identical pattern to main timers. They survive page refreshes and are restored in the boot sequence.*

### IndexedDB Mirror

Database: IPHTrackerBackup, Store: kv

Critical keys (\_BACKUP_KEYS) are mirrored to IDB on every localStorage.setItem via a transparent override:

const \_lsSetOrig = localStorage.setItem.bind(localStorage);  
localStorage.setItem = function(key, value) {  
\_lsSetOrig(key, value);  
if (\_BACKUP_KEYS.includes(key)) \_idbPut(key, value); // fire-and-forget  
};

## 5\. Main Timer System

### TIMER_CONFIGS

const TIMER_CONFIGS = {  
acctSum: {  
checkboxId: 'accsum',  
displayId: 'acctSumTimer',  
inputId: 'acctSumIdInput',  
inputStorageKey: 'acctSumId',  
storageKey: 'acctSumTimerStart',  
entryLabel: 'Acct Sum',  
},  
ctc: { ... },  
invCrt: { ... },  
intEsc: { ... },  
}

### Timer State Object

timerState = {  
acctSum: { startTime: null, intervalId: null, alertShown: false },  
ctc: { ... },  
invCrt: { ... },  
intEsc: { ... },  
}

### Timer Lifecycle

User clicks timer button (checkbox change event)  
│  
├─ If unchecked → stopAndRecordIndependentTimer(key)  
│ ├─ clearInterval(state.intervalId)  
│ ├─ calculateDuration()  
│ ├─ saveEntry() → localStorage + IDB mirror  
│ ├─ createSessionBlock() or updateBlockHeader()  
│ ├─ localStorage.removeItem(config.storageKey)  
│ └─ refreshIPHBar()  
│  
└─ If checked → startIndependentTimer(key)  
├─ state.startTime = new Date()  
├─ localStorage.setItem(config.storageKey, startTime.getTime())  
├─ startIndependentTimerInterval(key) → setInterval(10ms)  
└─ createSessionBlock()

## 6\. Sub-Task Independent Timer System

### Overview

Each sub-task button (IDV, TIV, BIV, BOV, LOA, LRV, ADV, FO, BAV) has its **own independent timer** that measures the time the specialist actually spends on that specific step.

**Prerequisite:** Acct Sum must be running. If not, a warning popup is shown and the sub-task timer is not started.

### Sub-Task State Object

const subTaskState = {};  
// Initialised as:  
SUB_TASK_VALUES.forEach(v => {  
subTaskState\[v\] = { startTime: null, intervalId: null };  
});

### Click Lifecycle (Toggle Pattern)

User clicks sub-task button (e.g. IDV)  
│  
├─ If Acct Sum NOT running → warning popup → return (timer untouched)  
│  
├─ If subTaskState\['IDV'\].startTime === null → START  
│ ├─ subTaskState\['IDV'\].startTime = new Date()  
│ ├─ button gets class 'subtask-running' (cyan border glow)  
│ ├─ &lt;span id="st-IDV"&gt; shows '00:00'  
│ └─ setInterval every 500ms → \_stTick('IDV')  
│ ├─ updates display: '01:23'  
│ └─ updates live color from own elapsed time  
│  
└─ If subTaskState\['IDV'\].startTime !== null → STOP (validate first)  
├─ Validate Queue selected → if not: warning, timer keeps running, return  
├─ Validate Tier selected → if not: warning, timer keeps running, return  
├─ clearInterval(state.intervalId)  
├─ Calculate duration from state.startTime → now  
├─ Reset state.startTime = null  
├─ Remove button classes, clear display span  
└─ \_stRecord() → save entry to localStorage + DOM

**Key safety:** Validation runs **before** the timer is stopped. If Queue or Tier is missing, the warning fires but the timer continues running - identical behavior to the main timers.

### Key Functions

| **Function**                  | **Description**                                     |
| ----------------------------- | --------------------------------------------------- |
| \`recordSubTaskClick(value)\` | Entry point - toggle start/stop                     |
| \`\_stStart(value)\`          | Starts the timer interval, updates button visual    |
| \`\_stStop(value)\`           | Validates, stops interval, records entry            |
| \`\_stTick(value)\`           | Called every 500ms - updates display and live color |
| \`\_stFormat(ms)\`            | Formats milliseconds → \`'MM:SS'\` string           |

### Timer Display Inside Button

Each sub-task button contains a &lt;span class="st-display" id="st-{VALUE}"&gt; element:

&lt;button onclick="recordSubTaskClick('IDV')" class="subtask-btn" data-value="IDV"&gt;  
IDV &lt;span class="st-display" id="st-IDV"&gt;&lt;/span&gt;  
&lt;/button&gt;

- **At rest:** span is empty (invisible)
- **Running:** shows MM:SS in cyan (st-running class adds cyan color + glow)
- **Stopped:** cleared back to empty

### Simultaneous Sub-Tasks

Multiple sub-task timers can run at the same time - each has its own subTaskState\[value\] slot and its own setInterval. There is no mutual exclusion between sub-tasks.

### Live Color Logic

\_stTick(value) computes elapsed minutes from the sub-task's **own** startTime (not Acct Sum's), then calls getSubtaskColorData(elapsedMin, value, queueTypes, skillTypes) to determine the current color class. The button color reflects the sub-task's own performance.

### Entry Recording

When stopped, \_stStop records an entry with:

- duration: formatted as MM:SS:cs (minutes:seconds:centiseconds)
- colorClass: based on own elapsed time vs IPH threshold
- subTaskTypes: the value (e.g. 'IDV')
- blockId: current open Acct Sum block (if any), or null
- Entry goes into the open Acct Sum block body if one exists; otherwise inserted at top of registry

## 7\. IPH Engine

### Formula

Variance% = Σ(Expected minutes) / Σ(Actual minutes) × 100  
<br/>Your IPH = Total Resolves / (Σ Actual minutes / 60)  
<br/>Blended Target = Total Resolves / (Σ Expected minutes / 60)

### Expected Minutes Lookup

const lookupKey = \`\${contextKey}\_\_\${taskType}\`;  
const targetIPH = TARGET_IPH_TABLE\[lookupKey\];  
const expectedMinutes = targetIPH ? (60 / targetIPH) : null;

Entries with null expected minutes are excluded from IPH calculation (N/A scenarios).

### refreshIPHBar()

Called after every entry save, delete, or guide close. Updates the live bar fill, color class (iph-bar-good / iph-bar-bad), and value display.

## 8\. Color System

### Timer / Block / Sub-Task Colors

| **State** | **Condition**           | **CSS Class**    |
| --------- | ----------------------- | ---------------- |
| Green     | Within time limit       | \`timer-green\`  |
| Yellow    | +0 to +3 min over limit | \`timer-yellow\` |
| Red       | +3 to +7 min over limit | \`timer-red\`    |
| Black     | +7 min over limit       | \`timer-black\`  |

### Core Function

function minutesToColorData(elapsedMinutes, threshold) {  
if (threshold === null) return { colorClass: 'timer-green' };  
if (elapsedMinutes <= threshold) return { colorClass: 'timer-green' };  
if (elapsedMinutes <= threshold + 3) return { colorClass: 'timer-yellow' };  
if (elapsedMinutes <= threshold + 7) return { colorClass: 'timer-red' };  
return { colorClass: 'timer-black' };  
}

Sub-task buttons use btn-live-green, btn-live-yellow, btn-live-red, btn-live-black CSS classes, applied live via \_stTick() based on their own elapsed time.

## 9\. Context & Queue Logic

### Context Key

'br_t1' | 'br_t2' | 'br_ooc_en_t1' | 'br_ooc_en_t2'  
'ca_t1' | 'ca_t2'  
'cn_t1' | 'cn_t2'  
'sw_t1' | 'sw_t2' (Seller Wallet active)  
'sb' (SnowBall active)

### Priority Overrides

- **SU selected** → all sub-task limits use the SU row value
- **ESC selected** → sub-tasks use ESC-specific limits
- **SnowBall active** → overrides context to 'sb'
- **Seller Wallet active** → overrides context to 'sw_t1' or 'sw_t2'

### Mutual Exclusions

- T1 and T2 are mutually exclusive
- ESC can be combined with T2 only
- ESC requires at least one primary queue (BR/CA/CN)

## 10\. Registry System

### Session Block Schema

{  
blockId: string, // e.g. 'acctSum-1715557200000'  
key: string, // timer key ('acctSum', 'ctc', etc.)  
idText: string, // Case ID or 'N/A'  
queueTypes: string,  
skillTypes: string,  
isOpen: boolean,  
entryIds: string\[\],  
sellerWallet: boolean,  
snowBall: boolean,  
}

### Registry Entry Schema

{  
id: string,  
html: string,  
colorClass: string,  
isSubTask: boolean,  
duration: string, // 'MM:SS:cs' for sub-tasks, 'HH:MM:SS' for main  
subTaskTypes: string, // value for sub-tasks  
timerKey: string, // for main timers  
queueTypes: string,  
skillTypes: string,  
blockId: string | null,  
sellerWallet: boolean,  
snowBall: boolean,  
timestamp: string,  
}

## 11\. Suggestion Engine

### Category Pools

| **Pool**         | **Trigger**                                    |
| ---------------- | ---------------------------------------------- |
| \`performance\`  | ≥2 entries - pace vs target (green/yellow/red) |
| \`timer\`        | Any main timer currently running               |
| \`sellerWallet\` | SW toggle is ON                                |
| \`snowBall\`     | SB toggle is ON                                |
| \`subtasks\`     | Queue + Tier selected, no SW/SB                |
| \`setup\`        | No queue and no tier selected                  |

### Rotation

\_suggLastCategory prevents repeating the same category twice in a row. Priority order: timer → performance → subtasks → sellerWallet → snowBall → setup.

### Triggers

| **Event**          | **Delay**         |
| ------------------ | ----------------- |
| Page load          | 30 seconds        |
| Periodic           | Every 75 seconds  |
| Queue/skill change | 1 second debounce |
| Timer click        | 350ms             |
| SW/SB toggle       | 400ms             |
| Entry saved        | 800ms             |

## 12\. Guided Tour System

### Steps (14 total, 0-indexed)

| **Index** | **Step**                                              | **Special behavior**                                   |
| --------- | ----------------------------------------------------- | ------------------------------------------------------ |
| 0         | Timers                                                | -                                                      |
| 1         | Seller Wallet & SnowBall                              | -                                                      |
| 2         | Queue / Skills / Sub-Tasks (independent timers)       | -                                                      |
| 3         | IPH Life Bar                                          | \`\_guideStartBarAnim()\` - green↔red rAF loop         |
| 4         | Tasks Timer Registry                                  | \`\_guideEnsureRegistryBlock()\` - demo block if empty |
| 5-8       | Modals (Statistics, IPH Metrics, IPH Info, Data Mgmt) | -                                                      |
| 9         | Resolves Dashboard                                    | -                                                      |
| 10        | Theme Toggle & Guide                                  | -                                                      |
| 11        | Smart Suggestions                                     | \`\_guideStartSuggCycle()\` - rotating demo messages   |
| 12        | Backup & Restore                                      | -                                                      |
| 13        | Footer / Copyright                                    | Full-width spotlight                                   |

### Animation Kill-Switch

\_guideBarRunning = false checked on every rAF frame - stops the animation in ≤1 frame regardless of cancelAnimationFrame race conditions.

### Page Lock

document.body.classList.add('guide-page-locked') - pointer-events: none on everything except the guide overlay. Exit only via ✕ or ✓ Finish.

## 14\. Excel Export

### Function: \`exportToExcel()\`

Uses the **ExcelJS** library (loaded via CDN in index.html). Blocked while any main timer is running.

### Output: two-sheet \`.xlsx\`

**Sheet 1 - Timer Records**

| **Column** | **Value**                                                             |
| ---------- | --------------------------------------------------------------------- |
| #          | Entry sequence number                                                 |
| Date       | \`entry.timestamp\`                                                   |
| Duration   | \`entry.duration\` (MM:SS:cs for sub-tasks, HH:MM:SS for main timers) |
| Queue      | \`entry.queueTypes\`                                                  |
| Skill      | \`entry.skillTypes\`                                                  |
| Sub-Task   | \`entry.subTaskTypes\` (blank for main timer entries)                 |
| Feedback   | Motivational text (Great job, Keep working, etc.)                     |
| Timer ID   | \`entry.timerKey\`                                                    |

Entries are grouped visually by session block - each block header row is styled in dark blue, sub-rows alternate between teal (sub-tasks) and blue (timer closes).

**Sheet 2 - IPH Summary**

Calls calcIPH() and writes: Total Resolves, Your IPH, Blended Target, Variance%, Status, Σ Expected minutes, Σ Actual minutes. Formula is also printed for reference.

### Filename

IPH Results, Week {WW}, {Day}, {Mon}, {DD}, {YYYY}.xlsx

Generated by getFormattedDate() using **ISO 8601 week number** - matches the week number shown in the registry day banners.

## 15\. Backup System

### Layers

- **IndexedDB** - auto-mirrors \_BACKUP_KEYS on every write (fire-and-forget)
- **JSON file** - on-demand download from 💾 Backup button
- **File restore** - 📂 Restore reads .json → writes to localStorage → reloads

### JSON Backup Schema

{  
"registryEntries": "\[...\]",  
"sessionBlocks": "\[...\]",  
"dayBanners": "{...}",  
"casesCount": "12",  
"entryCount": "24",  
"checkboxState": "\[...\]",  
"sellerWalletActive": "false",  
"snowBallActive": "false",  
"\_backupDate": "2026-05-13T18:00:00.000Z",  
"\_version": "1.0"  
}

Filename: IPH Results, Week {WW}, {Day}, {Mon}, {DD}, {YYYY}.json

## 14\. Obfuscation Pipeline

\# 1. Strip block comments (/\* ... \*/) - string-aware  
\# 2. Strip line comments (//) - string-aware  
\# 3. Minify whitespace  
\# 4. UTF-8 encode → base64 (8,000-char chunks)  
\# 5. Wrap:  
wrapper = """/\* IPH Real Time Tracker - vssalama@ \*/  
(function(){  
var \_b="&lt;base64&gt;";  
var \_r=atob(\_b);  
var \_u=new Uint8Array(\_r.length);  
for(var \_i=0;\_i<\_r.length;\_i++)\_u\[\_i\]=\_r.charCodeAt(\_i);  
var \_s=new TextDecoder('utf-8').decode(\_u);  
(0,eval)(\_s);  
})();"""

- TextDecoder('utf-8') - correctly handles multi-byte emoji characters
- (0,eval) (indirect eval) - executes in global scope so function X(){} declarations are accessible via onclick handlers

## 15\. Data Schemas

### IPH_TARGETS.subtask\[contextKey\]\[subTaskValue\]

Returns minutes limit (number) or null (N/A). Used for live button colors in \_stTick().

### TARGET_IPH_TABLE\[\`\${contextKey}\_\_\${taskType}\`\]

Returns IPH target (number) or undefined. Used to compute expectedMinutes = 60 / iph.

## 16\. Key Constants Reference

| **Constant**               | **Value / Description**                                                                             |
| -------------------------- | --------------------------------------------------------------------------------------------------- |
| \`PRIMARY_QUEUES\`         | \`\['BR', 'CA', 'CN'\]\`                                                                            |
| \`T_TIER\`                 | \`\['T1', 'T2'\]\`                                                                                  |
| \`ESC_VALUE\`              | \`'ESC'\`                                                                                           |
| \`SUB_TASK_VALUES\`        | \`\['IDV','TIV','BIV','BOV','LOA','LRV','ADV','FO','BAV'\]\`                                        |
| \`subTaskState\`           | Per-sub-task \`{ startTime, intervalId }\` - persisted via \`st\_{VALUE}\_start\` localStorage keys |
| \`GUIDE_STEP_IPH_BAR\`     | \`3\`                                                                                               |
| \`GUIDE_STEP_REGISTRY\`    | \`4\`                                                                                               |
| \`GUIDE_STEP_SUGGESTIONS\` | \`11\`                                                                                              |
| \`\_IDB_NAME\`             | \`'IPHTrackerBackup'\`                                                                              |
| \`\_BACKUP_KEYS\`          | 8 critical localStorage keys                                                                        |

## 17\. Known Constraints

| **Constraint**                          | **Reason**                                              |
| --------------------------------------- | ------------------------------------------------------- |
| Flag images require internet            | Loaded from \`flagcdn.com\`                             |
| "Clear all site data" loses everything  | Browser API limitation - use JSON backup                |
| No specific download folder             | Browser security prevents forced folder paths           |
| Obfuscation is not encryption           | Determined person can reverse with effort               |
| \`TextDecoder\` required                | IE11 not supported                                      |
| Guide blocked if any main timer running | Prevents data loss during guided steps                  |
| Sub-tasks require Acct Sum running      | By business rule - enforced in \`recordSubTaskClick()\` |