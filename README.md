# 🐝 IPH Real Time Tracker

### Amazon KYC - VRMO SJO

_A Real-Time, Browser-Based Productivity Tracker for Amazon KYC Specialists._

_Tracks handle times, calculates live IPH, and provides intelligent suggestions - all with zero server dependencies._

## 📋 Table of Contents

- \[Overview\](#overview)
- \[Features at a Glance\](#features-at-a-glance)
- \[File Structure\](#file-structure)
- \[How to Run\](#how-to-run)
- \[Browser Compatibility\](#browser-compatibility)
- \[Data Persistence\](#data-persistence)
- \[Backup & Restore\](#backup--restore)
- \[Obfuscation\](#obfuscation)
- \[Author & Permissions\](#author--permissions)

## Overview

The **IPH Real Time Tracker** is a fully client-side web application for Amazon KYC agents at VRMO SJO. It enables specialists to measure and record task handle times in real time, calculate live IPH against context-specific blended targets, and receive smart sub-task prioritization suggestions - all without a server, login, or internet connection after the initial load.

## Features at a Glance

| **Category**            | **Features**                                                                                                                                                                 |
| ----------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| \*\*Timers\*\*          | Acct Sum, CTC, Inv-Crt, Int-Esc - independent, with Case ID inputs                                                                                                           |
| \*\*Sub-Task Timers\*\* | IDV, TIV, BIV, BOV, LOA, LRV, ADV, FO, BAV - each with its \*\*own independent timer\*\* shown inside the button (requires Acct Sum running; persists across page refreshes) |
| \*\*Color System\*\*    | Green → Yellow → Red → Black based on each timer's own elapsed time vs IPH threshold                                                                                         |
| \*\*Modes\*\*           | Seller Wallet and SnowBall toggles with dedicated thresholds                                                                                                                 |
| \*\*Queue & Skills\*\*  | BR / CA / CN + OOC CN / OOC EN / SU + T1 / T2 / ESC                                                                                                                          |
| \*\*IPH Engine\*\*      | Live bar, variance%, Your IPH vs Blended Target                                                                                                                              |
| \*\*Suggestions\*\*     | Smart rotating tips for sub-task priority, pace, and context                                                                                                                 |
| \*\*Guided Tour\*\*     | 14-step interactive guide with spotlight and demo animations                                                                                                                 |
| \*\*Registry\*\*        | Grouped block history with tier badges, flags, edit, delete                                                                                                                  |
| \*\*Export\*\*          | Excel (two sheets) and JSON backup                                                                                                                                           |
| \*\*Theme\*\*           | Dark / Light mode, persisted across sessions                                                                                                                                 |

## File Structure

IPH/  
├── index.html → Main application (minified HTML)  
├── javascript.js → Full readable source code  
├── javascript_obfuscated.js → Production-ready obfuscated version  
├── styles.css → Minified stylesheet  
├── README.md → Project overview (this file)  
├── TECHNICAL_MANUAL.md → Architecture & developer reference  
└── USER_MANUAL.md → End-user guide

_\*\*For production:\*\* change \`&lt;script src="javascript.js"&gt;\` to \`&lt;script src="javascript_obfuscated.js"&gt;\` in \`index.html\`._

_\*\*For development:\*\* always work on \`javascript.js\`, then regenerate the obfuscated file._

## How to Run

- Place all files in the same folder
- Open index.html in any modern browser
- No installation, no server, no internet connection required

Flag images load from flagcdn.com - the only external dependency, used for visual flags only.

## Browser Compatibility

| **Browser**       | **Status**       |
| ----------------- | ---------------- |
| Chrome 90+        | ✅ Full support  |
| Edge 90+          | ✅ Full support  |
| Firefox 88+       | ✅ Full support  |
| Brave             | ✅ Full support  |
| Safari 15+        | ✅ Full support  |
| Internet Explorer | ❌ Not supported |

Requires: localStorage, IndexedDB, TextDecoder, requestAnimationFrame, Uint8Array.

## Data Persistence

| **Layer**          | **Holds**                     | **Survives**                |
| ------------------ | ----------------------------- | --------------------------- |
| \`localStorage\`   | All entries, blocks, settings | Refresh and tab close       |
| \`IndexedDB\`      | Mirror of critical keys       | Basic "Clear cookies"       |
| JSON file (manual) | Full snapshot                 | Everything - stored on disk |

_⚠️ Choosing \*\*"Clear all site data"\*\* in the browser erases both localStorage and IndexedDB. Use the JSON Backup button regularly._

## Backup & Restore

**Backup:** click **💾 Backup** next to "Tasks Timer Registry" → downloads a .json file: IPH Results, Week #, Day, Month, Date, Year.json

**Restore:** click **📂 Restore** → select your .json → page reloads with full data recovered.

## Obfuscation

javascript_obfuscated.js is generated from javascript.js via:

- Comment stripping + whitespace minification
- UTF-8 → Base64 encoding (8,000-char chunks)
- Self-decoding wrapper: atob() + TextDecoder('utf-8') + (0,eval)

It is functionally identical to the source and runs in all supported browsers.

## Author & Permissions

© 2026 - vssalama@ · Amazon KYC · VRMO SJO

Contact **vssalama@** for feedback, bug reports, or permissions to use or adapt this code.
