# 🚚 Concrete Fleet Dispatch (Capstone Project)

A real-time fleet dispatch and simulation web application for ready-mix concrete (RMC) batching plants in the Bangkok Metropolitan Region.

Built with **HTML5, CSS3, JavaScript (ES6+), Leaflet.js, OpenStreetMap, and OSRM (Open Source Routing Machine)**.

🌐 **Live Demo**: Automatically deployed on [Vercel](https://vercel.com/) via GitHub push.

---

## 📖 For AI Agents & Collaborators

If you are an AI assistant (Claude, Cursor, Copilot, ChatGPT, Windsurf, etc.) or a new developer taking over this repository, please read **[`PROJECT_CONTEXT.md`](./PROJECT_CONTEXT.md)** first.

`PROJECT_CONTEXT.md` contains the full technical context, architectural decisions, data models, routing API details, coordinate conventions, and development guidelines.

---

## ✨ Features

- **Interactive Bangkok Map**: Powered by Leaflet.js and OpenStreetMap without requiring paid API keys.
- **Map Themes**:
  - `OSM Default`: Standard full-color OpenStreetMap.
  - `OSM Clean / Minimal`: Custom CSS-filtered muted map designed for operational dashboards.
- **6 Realistic Batching Plants**: Located across Bangkok perimeter (Bang Sue, Bang Na, Rama 2, Min Buri, Pathum Thani, Bang Yai).
- **Realistic Delivery Radius (5–15 km)**: Trucks deliver to construction sites within a realistic operational radius around each specific plant.
- **Real-Road OSRM Routing**: Trucks simulate movement along actual roads using the Open Source Routing Machine driving API.
- **Smart Auto-Fit Zoom**:
  - Clicking a truck zooms and fits bounds perfectly between the home plant, current truck position, and the job site.
  - Clicking a plant fits all associated delivery sites.
- **Fleet Simulation Engine**: Realistic delivery progress, return trips, at-plant loading states, and live ETA calculation.
- **Fleet Assignment Management**: Reassign trucks to different default plants with local browser storage persistence.

---

## 🚀 Running Locally

No build tools, bundlers, or `npm install` needed! Simply open `index.html` in your web browser:

```bash
# Optional: run with any local static server
npx serve .
# or simply double-click index.html
```

---

## 🛠️ Tech Stack & Services

- **Frontend**: Vanilla JavaScript, CSS3, HTML5
- **Mapping**: [Leaflet.js](https://leafletjs.com/) v1.9.4
- **Map Tiles**: [OpenStreetMap](https://www.openstreetmap.org/)
- **Routing Engine**: [OSRM (Open Source Routing Machine)](https://project-osrm.org/) public API
- **Deployment**: [Vercel](https://vercel.com/) (Connected to `main` branch)
