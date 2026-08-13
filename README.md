# Invoice Generator + Ledger

A lightweight, no-install invoicing system for freelancers and small businesses, in two parts:

- **`invoice-generator.html`** — a single-file, browser-based invoice generator. Create clean PDF invoices, in any of € / £ / $, and duplicate previous ones. Nothing to install; runs entirely in your browser.
- **`ledger-template.xlsx`** — a spreadsheet that tracks your invoices and projects: billed vs. unbilled per project, quarter totals, and a per-quarter filter. Opens in Excel or LibreOffice.

It's especially suited to freelancers who:

- **issue a limited number of invoices** — say, fewer than 50 a year — and don't need heavy automation;
- **charge a fixed fee per project** rather than billing by the hour;
- **often split a project's billing across several invoices** — the ledger updates each project's billed and unbilled amounts as those invoices go out;
- **bill in different currencies** — invoice in €, £ or $ as each client needs.

Everything runs locally: the generator uses your browser's local storage; the ledger is just an Excel file.

---

## Invoice generator

**Two ways to use it — both are the complete tool, and neither needs installing:**

1. **Use it online (easiest):** open the live link — <!-- add your GitHub Pages URL here, e.g. https://your-username.github.io/your-repo/invoice-generator.html --> — and it runs straight in your browser. Best if you want your saved details and invoice library to persist reliably.
2. **Download and use offline:** grab `invoice-generator.html` from the repo (green **Code → Download ZIP**, or open the file and download it) and double-click it to open in any browser. Fully offline, and it's your own permanent copy.

One thing to know: the generator remembers your business details and past invoices in **your browser**, and each way of opening it keeps its *own* separate store — so pick one way and stick with it, and your invoices stay put.

- **Set up once:** fill in your business name, address and tax ID at the top, and your payment details at the bottom, then click **Save my details** — they're remembered on that browser for next time. You can also add a logo.
- **Create an invoice:** every field is editable — click and type. Add line items with **+ Add line**; the amount and total calculate themselves. Pick the currency (€ / £ / $) from the toolbar; the symbol shows next to each amount.
- **Duplicate:** the **Duplicate from previous…** dropdown starts a new invoice from an old one (same client, line items, payment block), with the next invoice number and today's date filled in. Due date defaults to 30 days out.
- **Export:** click **Export PDF** → choose *Save as PDF* in the print dialog. The file is named `Invoice_<number>`.
- **Save an invoice** to your library with **Save this invoice** so you can duplicate it later.

Payment details can auto-fill by the client's country (a UK vs. US example is built in) — edit those blocks with your own accounts.

## Ledger

Open `ledger-template.xlsx` in Excel or LibreOffice. The golden rule: **blue cells are editable, black cells contain formulas and should not be typed over.** Here's exactly what's manual and what's automatic on each tab.

**Projects** — one row per project, with its invoices listed underneath. Your first step is to set your projects up here.

*You enter (blue):*

- Project name
- Client
- **Total value** — the contract value
- optional **Original value** — e.g. you bill in euros but quoted a US client in USD and want a reminder of the original USD figure

*Calculated (black):*

- **Billed** — the sum of every invoice tagged to that project
- **Unbilled** (Total − Billed), the indented list of the project's invoices, and the TOTAL row

These updates are triggered the moment you assign a project to a specific invoice on the Invoices tab; you never type billed/unbilled by hand. (Invoice tagging is explained in the Invoices section below.)

**To add a project:** just fill in the next empty project row (name + total value). The template ships with **10 ready-to-use project rows**, each already wired up — so a new project shows up in the **Project** dropdown on the Invoices tab automatically, with nothing else to set up. Renaming a project updates the dropdown too. (Only if you need more than 10 projects does the sheet need extending — see *Making changes* below.)

**Invoices** — your master list, one row per invoice.

*You enter (blue):*

- Invoice data: number, Client, Value (in your ledger currency), Issue Date, Status (from the dropdown), and Due Date / Payment Date if you want them
- **Project** (from the dropdown — this is what links the invoice to a project)
- optionally an **Original value** (the native-currency amount, if you're billing a client in a different currency, for your reference only)

*Calculated (black):*

- **Days to Due** (from the due date)
- **Period** — tags each invoice *This quarter / Last quarter / Older* from its issue date. Use the filter on the **Period** column to show just the quarter you're working on.
- **PDF Link** — an optional clickable link to that invoice's saved PDF on your local machine, so you can open the actual invoice document straight from the ledger. You can add the links manually (Excel's *Insert → Link*, or a `=HYPERLINK("C:\Invoices\Invoice_012026.pdf","Open")` formula), or have an AI assistant create them for you.

**Summary (read only)** — nothing to enter here at all; every figure is automatic. Project totals (total / billed / unbilled), invoice totals (outstanding, paid, overdue), and an **issued-by-period** view (this/last quarter, this/last year) that re-rolls with today's date.

**Archived projects** — a place to retire finished projects so the Projects tab doesn't clutter over time. **Don't just delete a finished project's row** — invoices link to a project by its *name*, so deleting it would strand those tags and lose the billed/unbilled numbers. Instead: copy the finished project's row onto the **Archived projects** tab (paste as *values*), then clear its row on the Projects tab to free the slot. The invoices keep their tag and stay on the Invoices tab, so their history and the invoice/quarter totals are completely unaffected — you're only moving the project's final snapshot out of the active list. One rule: don't reuse an archived project's name for a new project, or old invoices would re-link to it.

To start: delete the sample project and sample invoice, then add your own. The one action that ties everything together is **assigning a Project to an invoice** (via the dropdown) — that single step feeds the Projects and Summary views automatically.

---

## Importing existing invoices (optional)

If you already track invoices somewhere else — Harvest, Wave, FreshBooks, another spreadsheet — you can seed the ledger from an export instead of retyping everything.

Export your invoices to **CSV**. Most tools include the fields the ledger needs. A Harvest invoice-report export, for example, has *Issue Date, Last Payment Date, ID (invoice number), Client, Invoice Amount, Balance, Currency* and the client address. Those map straight onto the Invoices tab: ID → **Invoice #**, Client → **Client**, Invoice Amount → **Value** (convert to your ledger currency if needed), Issue Date → **Issue Date**, Last Payment Date → **Payment Date**, and **Status** = *Paid* when the balance is zero, otherwise *Pending payment*.

The quickest way is to hand the CSV to an AI assistant (see *Making changes*) and ask it to fill the Invoices tab from it — that's exactly how this ledger's history was first loaded. A couple of things exports usually **don't** carry, so you add them after: the **Project** tag (assign it from the dropdown) and the **PDF Link** (point it at your saved PDF).

---

## Setting up the live link (GitHub Pages)

*For whoever publishes or forks the repo — regular users can skip this.* This is the one-time step that creates the "use it online" link in option 1 above (free on a public repo):

1. Create a new GitHub repository and upload these files.
2. In the repo, go to **Settings → Pages**.
3. Under *Build and deployment*, set **Source: Deploy from a branch**, branch **main**, folder **/ (root)**, and Save.
4. After a minute, the generator is live at:
   `https://<your-username>.github.io/<your-repo>/invoice-generator.html`

The ledger template stays a download from the repo.

---

## Making changes

There's no code to compile and no build step — the whole system is just one HTML file and one spreadsheet. Change either yourself, or hand the files to an AI assistant (such as Claude) and describe what you want in plain language: add a column, change the statuses, add more project rows, tweak the layout, adjust the wording. It can edit the files for you — which is exactly how this was built.

## Privacy

Nothing is uploaded anywhere. The generator stores your business details and invoice library in your browser's local storage (per browser, on your device). The ledger is a local spreadsheet file. Clearing your browser data resets the generator.

## License

Public domain (The Unlicense) — see `LICENSE`. No copyright, no attribution required. Free to use, modify, sell and share, for any purpose.
