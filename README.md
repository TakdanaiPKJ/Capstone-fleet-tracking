# 🚚 Concrete Fleet Dispatch

A real-time fleet tracking and simulation web application for ready-mix concrete (RMC) dispatch operations in the Bangkok Metropolitan Region.

Built as an interactive, lightweight single-page application using **HTML5, CSS3, JavaScript, Leaflet.js, OpenStreetMap, and OSRM**.

🌐 **Live Demo**: [Deployed on Vercel](https://vercel.com/) (Auto-deployed from `main` branch)

---

## ✨ Features

### 🗺️ Interactive Fleet Tracking Map
- **Bangkok Metropolitan Map**: Real geographic visualization using Leaflet.js and OpenStreetMap without requiring any paid API keys.
- **Multiple Map Themes**:
  - **OSM Default**: Standard OpenStreetMap view showing full terrain and road labels.
  - **OSM Clean / Minimal**: Muted, grayscale theme designed for operational control rooms so that fleet markers and route lines stand out clearly.
- **Intelligent Auto-Zoom (`fitBounds`)**:
  - Selecting a truck automatically zooms and frames the view to encompass the **Home Plant**, **Current Truck Position**, and **Delivery Site**.
  - Selecting a plant frames the plant and all its associated delivery sites.
  - Reset button restores full metropolitan overview.

### 🛣️ Real-Road Routing Simulation (OSRM)
- **Actual Road Trajectories**: Rather than straight lines, routes follow real street networks retrieved dynamically from the Open Source Routing Machine (OSRM) driving API.
- **Realistic Logistics Radius**: Trucks deliver to construction sites within an authentic 5–15 km operational radius around their home plant.
- **Smooth Road Navigation**: Concrete mixer trucks interpolate their motion directly along curvy road coordinates.

### ⏱️ Real-Time Fleet Simulation Engine
- **Live Fleet Activity**: Simulates trucks transitioning through different operational states:
  - 🟢 **Delivering**: In-transit to the customer site with loaded concrete.
  - 🔵 **Returning**: Heading back to the batching plant after discharge.
  - 🟡 **At plant**: Parked and loading at the batching plant.
- **Dynamic Metrics**:
  - Real-time Estimated Time of Arrival (ETA) calculation based on progress.
  - Speed and load volume indicators.
  - Simulation controls to pause or resume playback anytime.

### 🏭 Plant & Truck Assignment Management
- **Plant Overview**: Inspect individual batching plants, active mixer fleet size, and localized destination sites.
- **Fleet Reassignment**: Reassign default home plants for any truck through a dedicated settings interface.
- **Persistence**: Saved plant assignments are retained locally in the browser (`localStorage`).

### 📊 Operations Dashboard
- High-level overview cards tracking active plants, total vehicles, trucks on the road, and trucks currently loading at base.
- Filterable search to find specific plant locations or truck IDs and drivers quickly.

---

## 🚀 Running Locally

No installation or build step required. Simply open `index.html` in any modern web browser:

```bash
# Option 1: Open directly in your browser
open index.html # On macOS
start index.html # On Windows

# Option 2: Run via any simple static server
npx serve .
```

---

## 🛠️ Tech Stack

- **Frontend**: Vanilla JavaScript (ES6+), CSS3, HTML5
- **Map & GIS**: [Leaflet.js](https://leafletjs.com/) (v1.9.4) & [OpenStreetMap](https://www.openstreetmap.org/)
- **Routing Engine**: [OSRM API](https://project-osrm.org/)
- **Deployment**: [Vercel](https://vercel.com/)
