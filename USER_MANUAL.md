# 📖 IPH Real Time Tracker - User Manual

### Amazon KYC · VRMO SJO

## Table of Contents

- \[Getting Started\](#1-getting-started)
- \[Understanding the Interface\](#2-understanding-the-interface)
- \[Timers\](#3-timers)
- \[Seller Wallet & SnowBall\](#4-seller-wallet--snowball)
- \[Queue, Skills & Tier\](#5-queue-skills--tier)
- \[Sub-Task Timers\](#6-sub-task-timers)
- \[The Color System\](#7-the-color-system)
- \[The IPH Life Bar\](#8-the-iph-life-bar)
- \[Tasks Timer Registry\](#9-tasks-timer-registry)
- \[Statistics Modal\](#10-statistics-modal)
- \[IPH Metrics Modal\](#11-iph-metrics-modal)
- \[IPH Reference Guide\](#12-iph-reference-guide)
- \[Smart Suggestions\](#13-smart-suggestions)
- \[Guided Tour\](#14-guided-tour)
- \[Backup & Restore\](#15-backup--restore)
- \[Export to Excel\](#16-export-to-excel)
- \[Dark / Light Theme\](#17-dark--light-theme)
- \[Resetting All Data\](#18-resetting-all-data)
- \[Tips & Best Practices\](#19-tips--best-practices)
- \[Troubleshooting\](#20-troubleshooting)

## 1\. Getting Started

- Open index.html in your browser (Chrome, Edge, Firefox, or Brave recommended)
- No login, installation, or internet connection needed
- Your data is saved automatically - it will still be there after closing or refreshing
- If it's your first time, a guided hint will appear pointing to the **?** button in the top-right corner

_\*\*Tip:\*\* Click the \*\*?\*\* button to take the full 14-step guided tour before starting._

## 2\. Understanding the Interface

| **Section**                      | **What it does**                                            |
| -------------------------------- | ----------------------------------------------------------- |
| \*\*Header\*\*                   | Amazon KYC logo, app title, dark/light toggle, guide button |
| \*\*Timers\*\*                   | Four main timer cards (Acct Sum, CTC, Inv-Crt, Int-Esc)     |
| \*\*Seller Wallet / SnowBall\*\* | Mode toggles                                                |
| \*\*Selections Panel\*\*         | Queue, Skills (with Tier), and Sub-Task timer buttons       |
| \*\*Smart Suggestions\*\*        | Contextual tips (appear dynamically)                        |
| \*\*Action Bar\*\*               | IPH bar, management buttons, modal buttons                  |
| \*\*Tasks Timer Registry\*\*     | Full history of all recorded entries                        |

## 3\. Timers

Four independent timers, each for a specific task type:

| **Timer**        | **Used for**                     |
| ---------------- | -------------------------------- |
| \*\*Acct Sum\*\* | Account summarization tasks      |
| \*\*CTC\*\*      | Customer contact (call) tasks    |
| \*\*Inv-Crt\*\*  | Investigation and creation tasks |
| \*\*Int-Esc\*\*  | Internal escalation tasks        |

### How to use a timer

- **Type your Case ID** in the input field below the timer (optional but recommended)
- **Click the timer button** to START - it turns bright green and begins counting
- Work on your task
- **Click the button again** to STOP and record the entry

_Running timers are saved automatically. If you refresh the browser, the timer resumes exactly where it left off._

### What happens when you stop a timer

- The entry is recorded in the **Tasks Timer Registry**
- The **IPH Life Bar** updates immediately
- A color-coded entry appears (green, yellow, red, or black)

## 4\. Seller Wallet & SnowBall

| **Toggle**                 | **When to use**                        |
| -------------------------- | -------------------------------------- |
| \*\*Seller Wallet (SW)\*\* | When working Seller Wallet queue tasks |
| \*\*SnowBall (SB)\*\*      | When working SnowBall queue tasks      |

Activate before stopping the timer. The prefix **(SW)** or **(SB)** appears in every session block header, and IPH thresholds adjust automatically.

## 5\. Queue, Skills & Tier

Select your context before stopping any timer:

### Queue (select one)

| **Button**    | **Queue**       |
| ------------- | --------------- |
| 🇧🇷 \*\*BR\*\* | Brazil domestic |
| 🇨🇦 \*\*CA\*\* | Canada          |
| 🇨🇳 \*\*CN\*\* | China           |

### Skills (select applicable)

| **Button**     | **Meaning**                     |
| -------------- | ------------------------------- |
| \*\*OOC CN\*\* | Out-of-Country - China origin   |
| \*\*OOC EN\*\* | Out-of-Country - English origin |
| \*\*SU\*\*     | Special Unit                    |

### Tier (required)

| **Button**  | **Meaning**                     |
| ----------- | ------------------------------- |
| \*\*T1\*\*  | Tier 1                          |
| \*\*T2\*\*  | Tier 2                          |
| \*\*ESC\*\* | Escalation (combinable with T2) |

_T1 and T2 are mutually exclusive. ESC requires at least one Queue selected._

## 6\. Sub-Task Timers

Sub-task buttons (**IDV**, **TIV**, **BIV**, **BOV**, **LOA**, **LRV**, **ADV**, **FO**, **BAV**) each have their **own independent timer** displayed inside the button.

### Requirements

- **Acct Sum must be running** before any sub-task timer can be started
- Queue and Tier must be selected before stopping (recording) a sub-task timer

### How to use

**Step 1 - Start the sub-task timer:**

Click a sub-task button (e.g. **IDV**). The button lights up with a cyan glow border and a live MM:SS counter appears inside it:

\[ IDV 00:00 \] → \[ IDV 01:23 \] → \[ IDV 04:30 \]

**Step 2 - Work on that step** while the timer counts up.

**Step 3 - Stop and record:**

Click the button again. The timer stops, the entry is recorded in the registry with the sub-task's exact handle time, and the button resets to its normal state.

### Multiple sub-tasks simultaneously

You can run multiple sub-task timers at the same time. For example, IDV and TIV can both be counting independently while Acct Sum is running.

### Live color feedback

While a sub-task timer is running, the button changes color based on **its own** elapsed time vs the IPH limit for your current context:

| **Color** | **Meaning**                 |
| --------- | --------------------------- |
| 🟢 Green  | Within the time limit       |
| 🟡 Yellow | Up to +3 min over limit     |
| 🔴 Red    | +3 to +7 min over limit     |
| ⚫ Black  | More than +7 min over limit |

### What if Queue or Tier is not selected when stopping?

A warning popup appears, but the timer **keeps running** - you don't lose your time. Simply select the required Queue and/or Tier, then click the button again to stop and record.

### Sub-task timers and page refresh

Sub-task timers **persist across page refreshes**, exactly like the main timers (Acct Sum, CTC, etc.). If you refresh the browser while IDV is running at 01:23, it will resume at 01:24 when the page reloads.

## 7\. The Color System

Every recorded entry is color-coded based on handle time vs IPH target:

| **Color** | **Meaning**                           |
| --------- | ------------------------------------- |
| 🟢 Green  | Within time limit - great performance |
| 🟡 Yellow | Up to +3 min over limit               |
| 🔴 Red    | +3 to +7 min over limit               |
| ⚫ Black  | More than +7 min over limit           |

Limits are context-specific - see the **IPH Reference Guide** for exact values.

## 8\. The IPH Life Bar

IPH \[████████████████████░░░░░░░░\] 8.50

- **Green bar** = Your IPH ≥ Blended Target (Variance ≥ 100%)
- **Red bar** = Your IPH < Blended Target
- Updates automatically after every entry

## 9\. Tasks Timer Registry

Every recorded entry appears here grouped into session blocks.

### Session block anatomy

\[ ACCT SUM \] 🇧🇷 \[ T1 \] Case-12345 \[ ✏️ Edit \] \[ ✓ CLOSED \]  
<br/>1\. 5/13/2026, 10:30 AM · IDV · Duration: 04:15:00 Great job! Keep it up! 💪  
2\. 5/13/2026, 10:34 AM · TIV · Duration: 03:52:00 Great job! Keep it up! 💪  
3\. 5/13/2026, 10:38 AM · ACCT SUM CLOSED · Total: 8:07 Great Time! 😁

### Editing the Case ID on a closed block

- Click **✏️ Edit** on the block
- The Case ID becomes editable
- Click **📌 Fix** to save

### Deleting an entry

Click the 🗑️ icon on any entry. The IPH bar updates automatically.

## 10\. Statistics Modal

Click **📊 Statistics** for a full session breakdown:

- Total tasks, per Queue, per Timer, per Skill, per Sub-Task
- Seller Wallet and SnowBall totals
- Visual progress bars for each category

## 11\. IPH Metrics Modal

Click **📈 IPH Metrics** for your real-time performance dashboard:

| **Field**              | **Description**                            |
| ---------------------- | ------------------------------------------ |
| \*\*Total Resolves\*\* | Tasks counted toward IPH                   |
| \*\*Your IPH\*\*       | Actual items per hour                      |
| \*\*Blended Target\*\* | Weighted target for your task mix          |
| \*\*Variance %\*\*     | Your IPH ÷ Target × 100. ≥100% = on target |
| \*\*Status\*\*         | ON TARGET ✅ or BELOW TARGET ❌            |

### Formula

Variance% = Σ(Expected min) ÷ Σ(Actual min) × 100

## 12\. IPH Reference Guide

Click **📋 IPH Info** to browse time limits for every scenario.

- Use the **tabs** to switch between contexts (BR T1, BR T2, BR OOC EN, CA, CN, Seller Wallet, SnowBall, General Timer Limits)
- **Minutes Per Task** column (highlighted in amber) = the time limit for each sub-task or timer
- **N/A** = no limit defined for that scenario
- **SPECIAL RULES** boxes show overrides (SU mode, ESC mode, CA queue exceptions)

## 13\. Smart Suggestions

A contextual suggestion strip appears below the Selections Panel:

| **Type**          | **Color** | **Example**                                            |
| ----------------- | --------- | ------------------------------------------------------ |
| Sub-task priority | 🟢        | "Fastest: TIV (3.73 min), BIV (3.83 min)..."           |
| Timer tip         | 🟢        | "Acct Sum running - limit is 11.74 min for BR (T1)..." |
| On-target pace    | 🟢        | "Variance 115% - you're ahead!"                        |
| Near-target pace  | 🟡        | "Variance 97% - almost there!"                         |
| Below-target pace | 🔴        | "Variance 88% - focus on faster tasks!"                |
| SW tips           | 🟡        | "SW: Acct Sum target is 7.20 min (T2)..."              |

Dismiss with **✕** - suggestions resume after 90 seconds.

## 14\. Guided Tour

Click **?** (top-right) to open the 14-step interactive tour:

| **#** | **Step**                          |
| ----- | --------------------------------- |
| 1     | Timers                            |
| 2     | Seller Wallet & SnowBall          |
| 3     | Queue, Skills & Sub-Task Timers   |
| 4     | IPH Life Bar (live animation)     |
| 5     | Tasks Timer Registry (demo block) |
| 6     | Statistics Modal                  |
| 7     | IPH Metrics                       |
| 8     | IPH Reference Guide               |
| 9     | Data Management Buttons           |
| 10    | Resolves Dashboard                |
| 11    | Theme Toggle & Guide Button       |
| 12    | Smart Suggestions (live demo)     |
| 13    | Backup & Restore                  |
| 14    | Footer / Copyright                |

- Navigate with **Next →** / **← Prev** or click any dot to jump
- **✕** exits anytime - **✓ Finish** on the last step
- Tour is blocked if any main timer is running

## 15\. Backup & Restore

### Creating a backup

- Click **💾 Backup** (left of "Tasks Timer Registry")
- A .json file downloads: IPH Results, Week #, Day, Month, Date, Year.json
- Keep this file safe - it's your full data snapshot

### Restoring from backup

- Click **📂 Restore** (right of "Tasks Timer Registry")
- Select your saved .json file
- Page reloads with all data recovered

_Download a backup at the end of each shift._

## 16\. Export to Excel

Click **Export to Excel** for a .xlsx spreadsheet with two sheets:

**Sheet 1 - Timer Records:** all entries grouped by session block (Date, Duration, Queue, Skill, Sub-Task, Feedback, Timer ID)

**Sheet 2 - IPH Summary:** Total Resolves, Your IPH, Blended Target, Variance%, Status, Sigma minutes

Filename: IPH Results, Week {WW}, {Day}, {Mon}, {DD}, {YYYY}.xlsx

## 17\. Dark / Light Theme

Click 🌙/☀️ (top-right) to switch themes. Preference is saved and restored automatically.

## 18\. Resetting All Data

Click **Reset All Data** → confirm → all entries, blocks, and IPH data are cleared.

_⚠️ This cannot be undone. Download a \*\*💾 Backup\*\* first._

## 19\. Tips & Best Practices

**For accurate IPH:**

- Select Queue and Tier before stopping any timer
- Enter the Case ID before stopping for easy reference later
- Start sub-task timers immediately when you begin each step

**For better performance:**

- Check the Smart Suggestions strip - it tells you which sub-tasks to prioritize
- If the bar is red, focus on the fastest sub-tasks for your context
- Monitor IPH Metrics regularly to track your variance

**For data safety:**

- Download a backup at the end of each shift
- Never use "Clear all site data" without a backup
- Sub-task timers persist across refresh - just like main timers, they resume where they left off

**For the best experience:**

- Use full-screen or maximized browser window
- Keep browser zoom at 90-110%
- Chrome or Edge recommended

## 20\. Troubleshooting

| **Problem**                            | **Solution**                                                                 |
| -------------------------------------- | ---------------------------------------------------------------------------- |
| Sub-task timer won't start             | Make sure Acct Sum is running first                                          |
| Warning popup when stopping sub-task   | Select the required Queue and/or Tier - the timer keeps running until you do |
| Main timer not resuming after refresh  | Refresh again; if it persists, check that no other tab has the app open      |
| Registry entries not showing           | Refresh the page - entries are in localStorage and will reappear             |
| IPH bar empty after guide              | Record some real entries - guide demo data is cleared on exit                |
| Guide button not working               | Stop all running main timers first                                           |
| Data disappeared after browser clear   | Use \*\*📂 Restore\*\* with your saved \`.json\` backup                      |
| Excel export not downloading           | Stop all running main timers before exporting                                |
| Date shows as "2026-05-12" in registry | Restore from a fresh backup; this was fixed in the current version           |

_Document version: May 2026 · vssalama@ · Amazon KYC · VRMO SJO_