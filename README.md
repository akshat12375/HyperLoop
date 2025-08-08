# 🌐 HyperLoop — Walmart Surplus Redistribution Platform

**HyperLoop** is an AI-powered, store-to-store surplus inventory redistribution system built to reduce waste and optimize inventory flow across Walmart stores. It predicts surplus using a trained ML pipeline, recommends transfers to nearby stores in need, manages transfer requests via a lightweight dashboard, and rewards participating stores with Eco Credits.

**Live demo:** https://walmart-bly2.onrender.com

---

## 🔥 One-Line Summary
**HyperLoop:** AI-powered surplus redistribution system optimizing Walmart's inventory flow.

---

## 🚀 Key Features
- **Inventory Intake**: Add inventory with metadata (expiry, price, temperature, rainfall, local event).
- **ML-Powered Prediction**: XGBoost-based pipeline predicts demand / surplus per item.
- **Marketplace**: Browse surplus items from other stores and request transfers.
- **Transfer Requests**: Accept / Reject workflow with audit timestamps.
- **Eco Credit System**: Award credits for confirmed transfers to incentivize sustainability.
- **Dashboards & Charts**: Visualize surplus distribution and Eco Credits per store.
- **Lightweight, extendable Flask app** with MongoDB persistence.

---

## 🧩 Tech Stack
- **Backend**: Flask (Python)
- **Database**: MongoDB (PyMongo)
- **ML**: XGBoost, scikit-learn, pandas
- **Visualization**: matplotlib (for saved charts)
- **Deployment**: Gunicorn / Render (production), local Flask for dev
- **Other**: python-dotenv for environment configuration

---

## ⚙️ How it works (end-to-end - concise)
1. **Login** — store manager authenticates (store id, email, numeric password).
2. **Add Inventory** — manager submits product details + environment (temp, rainfall) and local event flag.
3. **Prediction** — `pipe.pkl` (preprocessing + XGBoost) predicts expected daily sale or required quantity.
4. **Labeling** — system compares predicted vs actual stock → marks item as `surplus` or `less` and computes `surplus_kg` / `less_kg`.
5. **Marketplace** — surplus items are listed (except from the same store). Other stores can request quantities.
6. **Requests** — recipient sends a `transfer_request` (Pending). Sender sees incoming requests on `active-requests`.
7. **Accept / Confirm** — when confirmed, inventory is updated for both stores and **Eco Credits** are awarded to the sender; transfers are logged.
8. **Dashboard** — store sees inventory, generated charts, and Eco Credits balance.


🔌 Important Endpoints (overview)
GET /login — login page
POST /login — authenticate and set session (store_id, region)
GET /dashboard — store dashboard (inventory, eco credits, charts)
GET /marketplace — list available surplus items from other stores
POST /request-transfer — request an item transfer from a store
GET /active-requests — view incoming requests (for recipient or sender depending impl)
POST /update-request — Accept/Reject (on Accept: update inventories + award eco credits)
POST /add-inventory — add new inventory (triggers ML prediction & status setting)
GET /logout — clear session

🧠 ML Model & Pipeline Details
Inputs/features used (example):
region (categorical)
item_name (categorical)
day_of_week (categorical)
quantity_in_stock (numeric)
price_per_kg (numeric)
expiry_days (numeric; days until expiry)
local_event (binary)
temperature, rainfall (numeric)
