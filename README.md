# ✈️ Flight Cost Intelligence

> Compare Indian domestic flights by **₹ per kilometre** — not just total price.

![HTML5](https://img.shields.io/badge/Frontend-HTML%2FCSS%2FJS-orange?logo=html5&logoColor=white)
![Python](https://img.shields.io/badge/Backend-Python%203.9+-blue?logo=python&logoColor=white)
![Flask](https://img.shields.io/badge/API-Flask%202.0-black?logo=flask)
![GitHub Pages](https://img.shields.io/badge/Hosted%20on-GitHub%20Pages-222?logo=github)
![Render](https://img.shields.io/badge/API%20on-Render-46E3B7?logo=render&logoColor=white)

| | |
|---|---|
| 🌐 **Live Site** | https://pawankushwahh.github.io/Flight_per_km_cost/ |
| ⚙️ **Live API** | https://flight-cost-intelligence-api.onrender.com |
| 📁 **Frontend repo** | https://github.com/pawankushwahh/Flight_per_km_cost |
| 📁 **Backend repo** | https://github.com/pawankushwahh/Flight_per_km_backend |

---

## 📖 About

Most travellers compare total fares — but a ₹3,000 short-hop and a ₹3,000 long-haul are not equal value. **Flight Cost Intelligence** normalises every Indian domestic route to **₹/km** so you can compare fairly.

```
₹ per km  =  Total Ticket Price (₹)  ÷  Route Distance (km)

Lower ₹/km = better value per kilometre flown.
```

The project is split into two repos:

| Repo | What it does | Hosted on |
|------|-------------|-----------|
| `Flight_per_km_cost` (this) | 8-page HTML/JS frontend | GitHub Pages |
| `Flight_per_km_backend` | Flask REST API + data files | Render |

---

## 🏗️ Architecture

```
Browser (GitHub Pages)
        │
        ▼  fetch JSON over REST (CORS enabled)
Flask API on Render
        │
        ▼  loaded once at worker startup
CSV + JSON data files (~118 routes, 26 airports)
```

- No database — data lives in flat CSV/JSON files loaded into memory at startup
- Frontend auto-detects localhost vs. production and switches the API URL accordingly
- A background ping to `/api/ping` warms the Render server on every page load

---

## 🖥️ Pages

| Page | File | What it does |
|------|------|-------------|
| Home | `index.html` | Quick compare, popular routes, live stats |
| Route Compare | `compare.html` | Multi-route ₹/km table, bar chart, Leaflet map |
| Price Predictor | `predictor.html` | Monthly price trends, best booking month |
| Route Finder | `route-finder.html` | All destinations from one origin, ranked by ₹/km |
| Route Optimizer | `optimizer.html` | Nearby airports, cabin classes, layover options |
| Cost Heatmap | `heatmap.html` | India map coloured by cost intensity |
| Visualizations | `visualizations.html` | Cheapest/priciest routes, city averages |
| FAQ | `faq.html` | Methodology, data sources, technical info |
| 404 | `404.html` | Friendly not-found page |

---

## 📡 API Endpoints

All responses: `{ "success": true, "data": ... }` or `{ "success": false, "error": "..." }`

| Method | Endpoint | Description | Used on |
|--------|----------|-------------|---------|
| `GET` | `/api/ping` | Health check / server warm-up | All pages |
| `GET` | `/api/airports` | All airports with coords | All dropdowns |
| `POST` | `/api/compare` | Compare routes by ₹/km | Compare |
| `POST` | `/api/predict` | Monthly price trend for a route | Predictor |
| `POST` | `/api/route-find` | Best destinations from an origin | Route Finder |
| `GET` | `/api/nearby-airports` | Nearby airports by IATA code | Optimizer |
| `GET` | `/api/class-layover` | Cabin class & layover data | Optimizer |
| `GET` | `/api/heatmap` | Regional cost heatmap data | Heatmap |
| `GET` | `/api/visualizations` | Cheapest/priciest routes + city averages | Visualizations, Home |
| `GET` | `/api/raw-compare-data` | Enriched routes (CSV + JSON merged) | Home popular routes |

> `POST /api/compare` also returns `skipped` and `not_found` arrays for invalid or missing routes.

---

## 🚀 Run Locally

You need **both repos** running simultaneously.

### 1 — Start the backend

```bash
git clone https://github.com/Rakshita-0206/Flight_Project.git
cd Flight_Project/Flight_backend

python3 -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate

pip install -r requirements.txt
python app.py
```

Backend runs at http://127.0.0.1:5000

### 2 — Serve the frontend

```bash
git clone https://github.com/Rakshita-0206/Flight_Project.git
cd Flight_Project/Flight_Fronted
python3 -m http.server 5500
```

Open **http://localhost:5500** in your browser.

`config.js` auto-detects localhost and points the frontend at `http://127.0.0.1:5000`.

---

## 📁 Project Structure

### Frontend (`Flight_per_km_cost`)

```
├── index.html
├── compare.html
├── predictor.html
├── route-finder.html
├── optimizer.html
├── heatmap.html
├── visualizations.html
├── faq.html
├── 404.html
└── assets/
    ├── css/
    │   └── main.css           # Design system (dark theme, responsive)
    ├── images/                # Hero + sub-hero photos
    └── js/
        ├── config.js          # API base URL + endpoint map
        ├── common.js          # apiCall, fetchAirports, formatters, share URLs
        └── images.js          # Route thumbnail paths
```

### Backend (`Flight_per_km_backend`)

```
├── app.py                     # Flask app + all API routes
├── gunicorn_config.py         # 4 workers, port 10000
├── Procfile
├── render.yaml                # Render deploy config
├── requirements.txt
├── scripts/
│   └── generate_data.py       # Regenerates JSON from CSV
├── data/
│   ├── compare_data_new.csv   # Canonical route prices (UPDATE FIRST)
│   ├── merged_flight_data.csv # Airport coords + names (UPDATE FIRST)
│   ├── compare_data.json      # auto-generated
│   ├── trend_data.json        # auto-generated
│   ├── class_layover_data.json# auto-generated
│   ├── heatmap_data.json      # auto-generated
│   └── nearby_airports.json   # auto-generated
└── tests/
    └── test_api.py            # 12 smoke tests covering all endpoints
```

---

## ☁️ Deployment

### Frontend → GitHub Pages

Push to `main` — GitHub Pages auto-deploys in 1–3 minutes.

```bash
git add .
git commit -m "Your message"
git push origin main
```

Live at: https://pawankushwahh.github.io/Flight_per_km_cost/

### Backend → Render

Push to the backend repo's `main` branch — Render auto-deploys.

```bash
git add .
git commit -m "Your message"
git push origin main
```

Render runs `pip install -r requirements.txt` then `gunicorn -c gunicorn_config.py app:app`.

| Setting | Value |
|---------|-------|
| Health check | `/api/ping` |
| Port | `10000` |
| Python version | `3.9.0` |

### Coordinating deploys

| You changed… | Push frontend? | Push backend? |
|---|:---:|:---:|
| HTML, CSS, JS, images only | ✅ Yes | ❌ No |
| `config.js` API URL | ✅ Yes | ❌ No |
| `app.py` or data files | ❌ No | ✅ Yes |
| Both UI and API | ✅ Yes | ✅ Yes (backend **first**) |

---

## 🗄️ Data Management

### Regenerate derived JSON (after CSV updates)

```bash
cd Flight_per_km_backend
python scripts/generate_data.py          # write all JSON files
python scripts/generate_data.py --check  # coverage report only
```

### Replace dummy data with real scraped data

1. Update `compare_data_new.csv` and `merged_flight_data.csv`
2. Run `python scripts/generate_data.py`
3. Push to GitHub → Render auto-redeploys (~2–5 min)
4. Run `pytest tests/ -v` to verify

---

## 🧪 Tests

```bash
cd Flight_per_km_backend
pip install -r requirements.txt
pytest tests/ -v
```

12 smoke tests cover all API endpoints.

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| Frontend | HTML5, CSS3, Vanilla ES6+ |
| Icons | Font Awesome 6.4 |
| Charts | Chart.js 4.4.1 |
| Maps | Leaflet 1.9.4 |
| Backend | Python 3.9+, Flask 2.0, flask-cors |
| Production server | Gunicorn + gevent (4 workers) |
| Storage | No database — in-memory cache at startup |
| Frontend hosting | GitHub Pages |
| API hosting | Render (free tier) |

---

## 🔧 Troubleshooting

| Problem | Fix |
|---------|-----|
| Slow first load (30–60 s) | Render free-tier cold start — wait and refresh |
| "Could not load route data" | Check backend is running; verify `config.js` API URL |
| Dropdowns empty | `/api/airports` call failed — check CORS and API URL |
| Maps or charts broken | Open browser console; check CDN scripts loaded |
| CORS errors locally | Ensure `flask-cors` is installed in the backend |

> **Tip:** Point an uptime monitor (e.g. UptimeRobot, free) at `/api/ping` every 10 minutes to prevent Render cold starts.

---

## 📝 Notes

- All prices in INR (₹); `cost_per_km` in ₹/km
- IATA codes must be 3 uppercase letters — invalid codes are returned in `skipped`
- Predictor uses historical trend averages from `trend_data.json` — not a live ML model
- Country is hardcoded to `"India"` in `/api/airports`
- Home page falls back to `/api/visualizations` if `/api/raw-compare-data` lacks `cost_per_km`
- Distances use the Haversine formula (great-circle, accurate to ~0.3%)

---

## 👥 Team

| Name | GitHub |
|------|--------|
| Rakshita | [@Rakshita-0206](https://github.com/Rakshita-0206) |
| Pawan Kushwah | [@pawankushwahh](https://github.com/pawankushwahh) |
| Shalini | — |

📍 Lucknow, India
