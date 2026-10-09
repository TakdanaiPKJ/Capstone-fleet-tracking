# Concrete Fleet Dispatch — Project Context & AI Agent Guide

> **Document Purpose**: This file serves as the single source of truth for the project. It is specifically designed to provide full context to any AI agent (Claude, Cursor, Copilot, ChatGPT, Windsurf, etc.) or human developer working on this codebase in future iterations.

> [!NOTE]
> **Data Status (ข้อมูล Mockup ทั้งหมด)**:
> ทั้งหมดของพิกัดแพลนท์ (Batching Plants), โครงสร้างไซต์งานก่อสร้าง (Job Sites), ชื่อจุดหมายปลายทาง, ข้อมูลรถ และคนขับในโปรเจกต์นี้ **เป็นข้อมูลจำลอง (Mockup / Synthetic Data)** ทั้งสิ้น สำหรับใช้ในการจำลองระบบ (Simulation), ทดสอบอัลกอริทึม และพัฒนา Prototype ของ Capstone ไม่ใช่พิกัดหรือข้อมูลความลับของบริษัทจริง

---

## 1. Project Overview & Background
- **Project Name**: Concrete Fleet Dispatch (Capstone Project)
- **Domain**: Ready-Mix Concrete (RMC) Logistics & Fleet Management
- **Target City**: Bangkok & Metropolitan Region (กรุงเทพฯ และปริมณฑล), Thailand
- **Data Nature**: 100% Mockup / Simulated Data (Plants, Coordinates, Job Sites, Trucks)
- **Repository**: `TakdanaiPKJ/Capstone-fleet-tracking`
- **Hosting / Deployment**: Vercel (Auto-deployed from `main` branch on GitHub)
- **Tech Stack**: Vanilla HTML5, CSS3, JavaScript (ES6+), Leaflet.js (v1.9.4), OpenStreetMap, OSRM (Open Source Routing Machine API)
- **Primary Entrypoint**: `index.html` (single-file architecture, no build tool or bundler needed)

---

## 2. Key Architecture & File Structure

```text
Capstone/
├── index.html           # Main production application (HTML, CSS, and JS all-in-one)
├── .gitignore           # Standard git ignore (node_modules, .vercel, scratch files)
├── PROJECT_CONTEXT.md   # This documentation and AI Agent handoff context file
└── README.md            # Repository overview
```

### Why Single-File (`index.html`)?
The application is currently designed as a zero-dependency, standalone single-page application (SPA). It uses CDNs for Leaflet CSS & JS and connects directly to public APIs (OpenStreetMap tile server & OSRM routing). Any static server or Vercel can serve it immediately without a build step (`npm run build`).

---

## 3. Core Features & Capabilities

### 3.1. Interactive Real-World Map (Leaflet.js + OSM)
- **Map Provider**: OpenStreetMap (Free, open-source, no API key required).
- **Map Styles (Dropdown Selector)**:
  1. `OSM Default`: Standard full-color OpenStreetMap tiles.
  2. `OSM Clean / Minimal`: Custom CSS-filtered OSM tiles (`grayscale(1) opacity(0.5) contrast(1.1)`), providing a subtle, muted background so that colorful trucks, plants, and route polylines pop out cleanly without visual clutter.
- **Auto-Fit Bounds on Click**:
  - Clicking a **Truck**: Automatically adjusts map view (`fitBounds`) to enclose the **Home Plant**, **Current Truck Position**, and **Delivery Site**, with max zoom capped at 15 to maintain route visibility.
  - Clicking a **Plant**: Automatically fits map view to cover the plant and all its active delivery site destinations.
  - Reset Target Button: Restores default view centered over Greater Bangkok (`[13.75, 100.5]`, zoom level 11).

### 3.2. Simulated Geographic Layout (Mockup Plants in Bangkok & Vicinity)
The project simulates **6 mockup concrete batching plants** distributed around Bangkok's metropolitan perimeter:
1. `P01 - Bang Sue Plant` (`lat: 13.82, lng: 100.53`) — North / Chatuchak Zone (Mockup)
2. `P02 - Bang Na Plant` (`lat: 13.66, lng: 100.61`) — East / Bang Na - Samut Prakan Zone (Mockup)
3. `P03 - Rama 2 Plant` (`lat: 13.66, lng: 100.43`) — South-West / Rama 2 - Samut Sakhon Corridor (Mockup)
4. `P04 - Min Buri Plant` (`lat: 13.81, lng: 100.72`) — Eastern Logistics Hub (Mockup)
5. `P05 - Pathum Thani Plant` (`lat: 13.98, lng: 100.52`) — Northern Industrial Belt / Rangsit (Mockup)
6. `P06 - Bang Yai Plant` (`lat: 13.87, lng: 100.41`) — Western Metropolitan Zone / Nonthaburi (Mockup)

### 3.3. Simulated Logistics Radius (Mockup Job Sites, 5 – 15 km per Plant)
In ready-mix concrete operations, wet concrete must be poured within 60–90 minutes before setting. Therefore, trucks do not cross the entire metropolis arbitrarily.
- Each plant is configured with **4 mock construction site destinations** located within a **5 to 15 km radius** (well within a realistic 20–30 km maximum range).
- All site names (e.g., *Chatuchak Smart Hub*, *Motorway M82 Extension*, *Pink Line Depot Complex*, *Rangsit Tech Innovation Lab*) are **mockup project titles** chosen to represent typical construction scenarios in those districts.
- In production, these mockup arrays can be swapped out with real database tables or dispatch ERP APIs.

### 3.4. Dynamic Road Routing via OSRM API
- Rather than straight-line interpolation, the app queries the public **OSRM Driving API**:
  `https://router.project-osrm.org/route/v1/driving/{lng1},{lat1};{lng2},{lat2}?overview=full&geometries=geojson`
- Route geometry is cached in `routeCache` to prevent rate-limiting.
- Dashed route polylines are rendered dynamically along real roads, and trucks interpolate their positions along these road coordinate arrays.

### 3.5. Simulation Engine
- Runs on a 3-second tick (`setInterval` in `index.html`).
- Trucks cycle through statuses:
  - `Delivering` (Green `#298466`): Progress advances along OSRM route from 0.0 towards 1.0.
  - `Returning` (Blue `#618eac`): Progress advances back towards home plant.
  - `At plant` (Amber `#bd9341`): Parked neatly at the batching plant with slight offset coordinates.
- Simulation can be paused and resumed using the top-right control button.
- Estimated arrival time (ETA) updates dynamically based on route progress.

### 3.6. Plant Settings & Truck Assignment
- A dedicated **Plant Settings** view allows reassigning default home plants for each truck (`CT-101` through `CT-124`).
- Changes are tracked in draft state and persisted to `localStorage` (`concrete-demo-assignments-v1`).

---

## 4. Key Data Models (in `index.html`)

### 4.1. `plants` Array
```javascript
const plants = [
  { id: 'P01', name: 'Bang Sue Plant', area: 'North industrial district', x: 100.53, y: 13.82 },
  { id: 'P02', name: 'Bang Na Plant', area: 'East industrial park', x: 100.61, y: 13.66 },
  // Note: in coordinates, x represents Longitude, y represents Latitude
  ...
];
```

### 4.2. `trucks` Array
```javascript
// 24 trucks (4 trucks initially per plant)
// Generated from driver names array
{
  id: 'CT-101',
  driver: 'Alex Morgan',
  status: 'Delivering' | 'Returning' | 'At plant',
  plant: 'P01',
  progress: 0.20, // 0.0 to 1.0 along the route
  capacity: 8     // m³ mixer drum capacity
}
```

### 4.3. `plantDestinations` Mapping
```javascript
const plantDestinations = {
  P01: [
    { name: 'Chatuchak Smart Hub', lat: 13.815, lng: 100.560 },
    ...
  ],
  ...
};
```

---

## 5. Guidelines for Future AI Agents & Developers

When modifying or extending this codebase, please adhere to these guidelines:

1. **Keep it Free & Keyless**:
   - Do NOT introduce dependencies requiring paid map API keys (Google Maps, Mapbox, Carto paid tokens) unless explicitly asked by the user.
   - Keep OpenStreetMap tile layer and public OSRM / open-source alternatives.

2. **Coordinate Conventions**:
   - Leaflet uses `[latitude, longitude]` (e.g. `[13.75, 100.50]`).
   - In legacy plant definitions, `p.y` is latitude and `p.x` is longitude.
   - OSRM request query requires `lng,lat` (`${p.x},${p.y};${dest.lng},${dest.lat}`).
   - OSRM geojson coordinates return `[lng, lat]`, which are flipped to `[lat, lng]` for Leaflet bounds and polylines.

3. **Deploy Workflow (GitHub -> Vercel)**:
   - Branch: `main`
   - Remote: `origin` (`git@github.com:TakdanaiPKJ/Capstone-fleet-tracking.git`)
   - Every `git push origin main` automatically deploys to Vercel production.
   - Ensure `index.html` is kept in sync and clean.

4. **Potential Next Features / Roadmap**:
   - [ ] Real-time WebSocket or Server-Sent Events (SSE) stream for live GPS tracking.
   - [ ] Order Dispatching Module (scheduling cubic meters, customer job orders, slump test logs).
   - [ ] Driver Mobile Web View (simple ticket check-in / arrival confirmation button).
   - [ ] Telemetry & Sensor Mockup (drum rotation speed, water addition, discharge status).
   - [ ] Carbon emission / fuel consumption dashboard analytics.

---

*Last Updated*: October 2026
