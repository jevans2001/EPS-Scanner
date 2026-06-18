# EPS Engine Tracker

Internal tool for Archer Aviation's Electric Propulsion Systems (EPS) team to track engine inventory, location, test stand assignments, and scan history.

---

## What's in this repo

| File | Purpose |
|------|---------|
| `index.html` | Scan station web app — open in any browser |
| `EngineTracker.gs` | Google Apps Script — powers the Google Sheets menu + scan API |
| `EngineTracker.bas` | Excel VBA module — for the Excel version of the tracker |
| `EPS_Engine_Tracker.xlsx` | Excel workbook with Database + Entry Form sheets |

---

## System Overview

```
Barcode Scanner
      │
      ▼
Scan Station (index.html)
      │  POST scan data
      ▼
Google Apps Script (Web App endpoint)
      │  writes to
      ▼
Google Sheets
  ├── Database      ← live engine records
  ├── Entry Form    ← manual add/update form
  └── Scan Log      ← full history of every scan
```

---

## Setup Guide

### 1. Google Sheets — Apps Script

1. Open the shared Google Sheet
2. **Extensions → Apps Script**
3. Delete any existing code
4. Paste the entire contents of `EngineTracker.gs`
5. Click **Save** (floppy disk icon)
6. Reload the Google Sheet — you should see **⚙ Engine Tracker** in the menu bar

### 2. Deploy the Web App (connects scanner to Sheets)

1. In the Apps Script editor click **Deploy → New Deployment**
2. Set the following:
   - Type: **Web App**
   - Execute as: **Me**
   - Who has access: **Anyone**
3. Click **Deploy** → authorize when prompted → click **Allow**
4. Copy the Web App URL (starts with `https://script.google.com/macros/s/...`)

### 3. Connect the Scan Station

1. Open `index.html` in a browser (or go to the GitHub Pages URL)
2. Paste the Web App URL into the yellow setup box at the top
3. Click **Save**
4. The footer should show **API: connected ✓**

### 4. GitHub Pages (team access)

The scan station is hosted at:
```
https://[your-org].github.io/eps-scanner
```

To update: replace `index.html` in the repo and push. Changes go live in ~60 seconds.

---

## Using the Scan Station

### Barcode scan flow
1. Scan engine barcode → engine is found, current location shown
2. Scan test stand barcode → check-in fires automatically
3. Google Sheets updates instantly:
   - **Database** — Location, Test Stand, Status → Active, Last Updated
   - **Scan Log** — new row with timestamp, engine, stand, CHECK IN/OUT

### Manual entry (keyboard testing without scanner)
- Type the engine SN in the first box → press **Enter**
- Type or click a stand name → fires automatically

### Auto check-out
If an engine is already checked into a stand and gets scanned into a different stand, it automatically logs a CHECK OUT from the previous stand before logging the new CHECK IN.

### Dormant flag
Engines that haven't been scanned in **14 days** are flagged as **DORMANT** in the Current Locations table.

---

## Using the Google Sheets Menu

The **⚙ Engine Tracker** menu appears at the top of the Google Sheet for everyone who opens it.

| Menu Item | What it does |
|-----------|-------------|
| ➕ Add Engine | Reads the Entry Form and appends a new row to Database |
| ✏️ Update Engine | Finds engine by SN, updates only the fields you filled in |
| 🗑 Clear Form | Clears all Entry Form input cells |
| ℹ️ Help | Shows in-app instructions |

**Update Engine rules:**
- Only fields you fill in get overwritten — blank fields are left exactly as they are
- Last Updated timestamp always refreshes automatically
- SN is required to find the record

---

## Test Stand Names

| Barcode Value | Stand |
|--------------|-------|
| `Jester` | Jester |
| `Maverick` | Maverick |
| `Merlin` | Merlin |
| `Viper` | Viper |
| `EAT` | EAT |
| `PIT LAB` | PIT LAB |
| `Offsite` | Offsite |

---

## Engine Database

51 engines tracked across B2 and B3 batches:
- **B2 Tilters:** B2-204, B2-212, BEE12, BEE58, BEE60, BEE102, BEE104
- **B2 Lifters:** B2-407, B2-409, B2-508, B2-508r1–r3, BEE16, BEE17, BEE20, BEE34, BEE56
- **B3 Tilters:** DEV-B3-203, DEV-B3-206, SOF-B3-204/205/210–212/214/215/219/220/264–268/270/351
- **B3 Lifters:** DEV-B3-403–405, SOF-B3-409–411/415/419–421/446–448/450/537

---

## Status Color Coding (Database sheet)

| Status | Color |
|--------|-------|
| Active | Green |
| Rebuild | Amber |
| Retired / Scrap | Red |
| Other | White / alternating sand |

---

## Field Reference

| Column | Field | Notes |
|--------|-------|-------|
| A | Serial Number | Unique engine ID — used as key for updates |
| B | Batch | B2, B3, B4, Other |
| C | Engine Type | Tilter, Lifter |
| D | Version / Config | e.g. B1.2, r1, r2 |
| E | Vintage | |
| F | Allotment / Campaign | Test campaign name |
| G | Campaign Notes | Additional context |
| H | Test Stand | Jester, Maverick, Merlin, Viper, EAT, PIT LAB |
| I | Procedure | EOL, ATP, HTP, QTP, DO-160 |
| J | Current Status | Active, Rebuild, Storage, Retired, Scrap, TBD |
| K | Current Location | Physical location |
| L | EOL Jira Ticket | |
| M | ATP Jira Ticket | |
| N | HTP Jira Ticket | |
| O | Last Updated | Auto-filled on every change |
| P | Updated By | Name of person who made the change |

---

## Roadmap

- [ ] Barcode label PDF — printable labels for all 51 engines + 6 stands
- [ ] Mobile PWA — install scan station as an app on phone
- [ ] Jira integration — auto-pull ticket status into Database
- [ ] Dashboard view — live status board for the lab wall
- [ ] Time-at-stand analytics — how long each engine spends at each stand

---

## Contacts

EPS Verification & Certification Team — Archer Aviation  
77 Rio Robles, San Jose, CA
