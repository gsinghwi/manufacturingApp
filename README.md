# ND Enterprises Desk (prototype)

A single-page prototype for a manufacturing unit's daily desk work: stock, vendors, customers, sales and multi-currency invoicing.

## Run it

Open `index.html` in a browser, or serve the folder:

```bash
python3 -m http.server 8080   # then open http://localhost:8080
```

The camera QR scanner needs `https://` or `localhost`. Data is kept in the browser's `localStorage`. To reset to the demo data, clear site data.

## Sign-in

| Username | Password | Role | Screens |
|---|---|---|---|
| `admin` | `nd@admin26` | Administrator | All, including Users & access and Currency & rates |
| `sales` | `nd@sales26` | Sales | Overview, New sale, Sales register, Customers, Stock master |
| `stores` | `nd@stores26` | Stores | Overview, Stock master, Inward, Reconciliation, Ledger, Vendors |

- Passwords are stored as salted SHA-256 hashes; administrators can add, disable and remove users, and anyone can change their own password.
- Sessions end when the tab closes, or after 12 hours with "Keep me signed in". Idle sessions sign out after 30 minutes.
- Five wrong passwords lock sign-in for one minute. Invoices record who prepared them.
- **This is client-side authentication for a prototype.** Anyone with access to the browser can read or change its storage. Before real use, move users and data to a server with a proper identity provider.

Change the demo passwords after first sign-in.

## Modules

| Module | What it does |
|---|---|
| Plant overview | Stock value, reorder watch, monthly billing, today's INR rates |
| New sale | Scan QR (camera, photo upload, typed code), quantity per scan, bill in any enabled currency, GST or export (LUT) tax, invoice |
| Sales register | All invoices with the rate used on the day |
| Customers | Add customers with a preferred billing currency |
| Stock master | Item master (SKU, HSN, price, cost, reorder, bin) with QR labels per unit and per pack |
| Stock inward | Goods receipt against a vendor; updates on-hand |
| Reconciliation | Physical count vs system, variance value, posts adjustments |
| Stock ledger | Every receipt, sale and adjustment |
| Vendors | Supplier master |
| Users & access | Add, disable or remove users; role access matrix; change own password |
| Currency & rates | Base currency (default INR, switchable with re-pricing), enabled currencies, live rates, manual overrides, GST rate |

## QR label format

- `SKU` adds 1 unit, e.g. `FLG-2040`
- `SKU|QTY` adds a pack, e.g. `FLG-2040|10`
- JSON is also accepted: `{"sku":"FLG-2040","qty":10}`

## Exchange rates

Rates are fetched from `https://open.er-api.com/v6/latest/INR` (quoted against the Indian rupee) on load and on **Refresh**. If the service can't be reached, saved or sample rates are used. Any rate can be overridden (₹ per unit) to lock a bank or forward-contract rate. Each invoice stores the rate it was raised at.

## Libraries

- [jsQR 1.4.0](https://github.com/cozmo/jsQR) for decoding
- [qrcode-generator 1.4.4](https://github.com/kazuhikoarase/qrcode-generator) for labels

## Prototype limits

No server or login; data lives in one browser. All companies, GSTINs and invoices are demo data.
