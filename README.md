# THREADLY clothing inventory

QR-based clothing inventory and sales tracker. The dashboard opens directly; Express serves the frontend and handles inventory, QR codes, and sales using the existing Supabase tables.

## Run locally on Windows

Use Node.js 22 or newer. From PowerShell:

```powershell
cd "C:\Users\matta\OneDrive\Documents\ChatGPT\OS&CN\qr-clothing-inventory"
node --version
Copy-Item .env.example .env
notepad .env
npm install
npm start
```

In `.env`, replace the example values for `SUPABASE_URL` and `SUPABASE_SECRET_KEY`. Keep the Supabase secret key in this server-side `.env` file only; do not put it in frontend code or commit `.env`. Manager username, password, and session settings are no longer used.

`PUBLIC_APP_URL` is already set for local use. If you scan a generated QR code with a phone on the same Wi-Fi, change it to `http://<your-computer-LAN-IP>:3000` (find the IPv4 address with `ipconfig`) and restart the server. For a deployed site, set it to that site's public base URL.

Once the server prints that it is listening, open <http://localhost:3000/>. Leave the terminal running while using the site. Stop it with `Ctrl+C`.

**Access note:** Login has been removed. Anyone who can reach the app can view and change inventory, record sales, and view sales data. Keep it on a trusted private network; do not expose it publicly unless access controls are added.

For development with automatic restart, use `npm run dev` instead of `npm start`.

## Existing Supabase data

The app expects the existing `products` and `sales` tables. Product sizes are stored as separate rows in `products`, so the multi-size stock form sends one existing inventory request for each size. The billing basket sends one existing sale request per product-size line. No schema or Supabase configuration changes are needed for these interface features.

Expected product fields: `id`, `name`, `category`, `size`, `price`, `stock`.

Expected sale fields: `id`, `product_id`, `product_name`, `quantity`, `total`, `created_at`.

The existing sales endpoint records and updates each line individually. If one line fails after others complete, those earlier sales remain saved; the interface reports this and keeps the remaining lines in the basket.
