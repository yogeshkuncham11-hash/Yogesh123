# 📚 Library Management System

Full-stack app: **HTML/CSS/JS** frontend, **Node.js + Express** backend, **SQLite (SQL)** for core data, **MongoDB (NoSQL)** for activity logs.

## Features
- **Registration**: students can self-register (name, username, password) and are logged in automatically
- **Student**: search catalog, borrow books (max 3) and **choose a return date** (up to 30 days, default 14), return books, view rewards & fines
  - Return on/before due date → **+10 reward points**
  - Late return → **tiered fine**: late days 1–7 = ₹2/day, days 8–20 = ₹5/day, after day 20 = ₹10/day (edit `FINE_TIERS` in `server.js`)
- **Payment options**: after a late return (or any time from *My Books*) the student picks **UPI / QR code**, **PhonePe** (opens the PhonePe app or scan the QR) or **Cash at counter**. For UPI/PhonePe the student submits the 12-digit UTR (sends a request). The librarian confirms/rejects in the **Payments** tab, which clears the fine. Set `UPI_ID` / `UPI_NAME` in `.env`.
- **Dashboard & Sections**: librarian sees Books / Available / Students / On loan / Overdue / Fines collected cards on every tab, plus a **Sections** tab (stock per section).
- **Bulk CSV import**: add many students (`name,username,password`) or books (`title,author,section,copies`) at once from the Students / Books tabs. Sample files are in `public/templates/`.
- **Demo time machine**: with `DEMO_MODE=true`, the librarian can *Skip 7 days*, *Skip 20 days* or *Reset* the app's date to test fines vs rewards. Turn it off in production.
- **Refunds (admin)**: in the Payments tab the librarian can **Refund** a confirmed payment and choose whether the fine becomes due again.
- **Fine system**: late fines are tiered and logged automatically; the librarian's **Fines** tab adds manual fines (damaged/lost book) with a reason, and shows the full fine ledger. Students see their own **Fine records** under Payment History.
- **Return history**: students and the librarian each get a **Return History** tab (returned books, dates, on-time/late, reward points, fines).
- **Payment history**: students get a **Payment History** tab (date, method, amount, reference, receipt no., status); the librarian's Payments tab shows all payments with total collected. The librarian can also **record cash** received at the desk from the Students tab.
- **Librarian**: add/remove students, add/remove books, section-wise stock, all borrowals/returns, overdue count, clear fines, mark books returned for a student, verify UPI payments, activity log

## Run
```bash
npm install
cp .env.example .env     # set JWT_SECRET; MONGO_URI optional
npm start
```
Open http://localhost:3000 — Librarian login: `admin` / `admin123` (change it!). Create students from the Students tab.

> Upgrading from an older version? Delete `library.db` once (or keep it – the app adds the new payment columns automatically, but UPI-only rows stay as they were).

## Structure
```
server.js   REST API + auth (JWT, bcrypt)
db.js       SQL schema (users, books, borrowals)
logger.js   MongoDB activity logs (memory fallback)
public/     index.html, style.css, app.js
```
Rules (loan days, fine tiers, reward) are constants at the top of `server.js`.

## Push to GitHub
```bash
git init && git add . && git commit -m "Library Management System"
git branch -M main
git remote add origin <your-repo-url> && git push -u origin main
```
