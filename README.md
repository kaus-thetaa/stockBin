# StockBin — Smart Cabinet Inventory System

A live-syncing inventory tracker for a physical storage cabinet, built with Google Sheets as the database, Google Apps Script as the API layer, and a static site on GitHub Pages as the frontend. No traditional backend server — the "database" is a spreadsheet, and it just works.

**Live demo:** _add your GitHub Pages link here_
**Stack:** Google Sheets · Google Apps Script · vanilla JS/HTML/CSS · GitHub Pages

---

## What this does

- A physical cabinet has N numbered compartments, each holding several items.
- Every item is logged in a Google Sheet: name, quantity, status, compartment, remarks, last-modified timestamp.
- A small Apps Script backend exposes that sheet as a JSON API.
- A static website renders the cabinet as an interactive grid — click a compartment, see what's inside. Search for an item, jump straight to its compartment. All of it updates automatically whenever the sheet changes, no redeploy needed.

## Why I built it

I wanted a project that touched a few things I hadn't combined before: using a spreadsheet as a real (if lightweight) database, writing serverless backend logic in Apps Script, and building a frontend that treats a physical object (a cabinet with compartments) as a first-class UI element instead of just a list or table.

## What I learned

- **Spreadsheets can be a legitimate lightweight database.** With data validation, dropdowns, and an `onEdit` trigger, Google Sheets handles auto-incrementing IDs, default values, and timestamps — things I'd normally reach for a real DB + backend for.
- **Apps Script as a serverless API.** `doGet()` in Apps Script, deployed as a Web App, turns a spreadsheet into a JSON endpoint anyone (or any frontend) can fetch from — zero infrastructure, zero hosting cost.
- **Bulk operations need different handling than single-row triggers.** My first version of the auto-ID trigger only worked for one edited row at a time. Pasting in 100+ rows at once silently broke it — taught me to write a separate bulk-backfill pass instead of assuming triggers scale to batch operations.
- **Designing UI around a physical constraint.** Instead of a generic table, the site mirrors the actual cabinet — a grid of compartments you click into. Small thing, but it made the tool feel like it belonged to the object it was tracking.
- **Decoupling data from presentation.** Because the frontend only talks to the Apps Script JSON endpoint, I can reskin the entire UI (I went from a blue theme to black) without touching a single line of backend code.

## Architecture

```
Google Sheet (data)
      │  edited directly, or via CSV import
      ▼
Apps Script Web App  →  doGet() serves JSON  (/exec endpoint)
      │
      ▼
Static site (GitHub Pages)
      │  fetch() polls the endpoint every 30s
      ▼
Interactive cabinet UI — compartment grid, search, live dashboard
```

## Features

- Auto-generated unique Item IDs, no duplicates
- Dropdown-enforced Status (`Working`, `Needs Repair`, `Broken`, `Missing`) and Compartment fields
- Auto-stamped Last Modified on every edit
- Live dashboard: total unique items, total quantity, breakdown by status
- Interactive cabinet grid — click any compartment to see its contents
- Instant search across all items, jumps to and highlights the right compartment
- Color-coded status indicators
- Fully data-driven: edit the sheet, the site updates within 30 seconds, no code changes

## Setup

See `SETUP.md` for full step-by-step instructions (written for someone setting this up for the first time, no prior Apps Script/GitHub experience assumed).

Short version:
1. Run `1_SheetSetup.gs` in a fresh Google Sheet's Apps Script editor to build the Inventory + Dashboard tabs.
2. Add `2_API.gs` in the same project, deploy it as a Web App.
3. Add `3_BulkBackfill.gs` for handling bulk CSV imports.
4. Paste in `inventory_import_dummy.csv` for demo data.
5. Drop your Web App URL into `index.html`, push to a GitHub repo, enable GitHub Pages.

## Possible next steps

- Swap the polling model for a webhook / push-based update
- Add an admin view for editing directly from the website instead of the Sheet
- Barcode/QR scanning to look up items by physical label
- Export low-stock or broken-item reports automatically

---

Built as a personal project to explore serverless architecture patterns using tools I already had on hand (a Google account and a GitHub account) — no cloud provider signup required.
