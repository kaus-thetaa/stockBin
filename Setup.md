# Setup guide

## 1. Google Sheet

1. Go to sheets.google.com, create a blank spreadsheet, name it `StockBin Inventory`.
2. `Extensions > Apps Script`.
3. Delete everything in the default `Code.gs`, paste in `1_SheetSetup.gs`.
4. Add a new script file named `API`, paste in `2_API.gs`.
5. Scroll to the bottom of `Code.gs`, paste in the contents of `3_BulkBackfill.gs`.
6. Run `setupInventorySystem` (function dropdown at top, click ▶ Run). Approve permissions (Advanced > Go to project (unsafe) > Allow — this warning is normal for your own scripts).
7. Check the spreadsheet — you'll see `Inventory` and `Dashboard` tabs.

## 2. Load demo data

1. Open `inventory_import_dummy.csv`.
2. Easiest way in: make a temporary blank Google Sheet, `File > Import > Upload`, select the CSV, "Insert new sheet." Select all the data there and copy.
3. In your real sheet's `Inventory` tab, click cell **B2** (not A2 — column A is for auto IDs) and paste.
4. Back in Apps Script, run `backfillInventory` to fill in IDs, statuses, and timestamps.

## 3. Deploy the API

1. In Apps Script: `Deploy > New deployment > Web app`.
2. Execute as **Me**, access **Anyone**.
3. Deploy, authorize again if asked, copy the Web App URL (ends in `/exec`).

## 4. Wire up the site

1. Open `index.html`, find `const API_URL = "PASTE_YOUR_WEB_APP_URL_HERE";`, replace with your URL.
2. Save.

## 5. Push to GitHub

1. Create a new repo on GitHub, e.g. `stockbin`.
2. Add all files (`index.html`, the `.gs` files for reference, `README.md`).
3. `Settings > Pages > Deploy from a branch > main / root > Save`.
4. Wait ~1 minute, your live link appears at the top of that Pages settings screen.

## 6. Test

- Open the live link, click through compartments, try the search bar.
- Edit something in the Sheet, refresh the site within 30s, confirm it updates.

---

## Commit message style

Since this is going on your portfolio, commit messages matter. Keep them:
- all lowercase
- no punctuation at the end
- short — a few words, one line, no body text unless truly necessary

Examples:
```
add compartment grid rendering
fix search not matching lowercase queries
wire up api endpoint to frontend
add dummy dataset for demo
switch theme from blue to black
handle bulk csv paste in backfill script
initial commit
```

Avoid:
```
Fixed a bug where the search bar wasn't working properly.
Added new feature: dashboard stats with live updates!
```
