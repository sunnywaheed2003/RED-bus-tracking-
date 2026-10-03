# 🚌 Karachi Red Bus Live Tracking & Route Finder System

A complete, modern, responsive real-time public transit tracking and route finding web application for **Karachi Red Bus (Sindh Peoples Bus Service)**.

Built with **HTML5, CSS3, modular JavaScript, Node.js, Express.js, Socket.IO, and Google Maps with an OpenStreetMap fallback**.

---

## 🌟 Key Features

1. **Live Interactive Map (Leaflet.js + OpenStreetMap)**:
   - Real-time animated Red Bus markers showing bus numbers, live speed (km/h), and direction of travel with orientation arrows.
   - 51 authentic Karachi bus stops mapped with interactive popups and approaching bus count.
   - Color-coded transit route polylines covering Karachi's flagship corridors.

2. **Accurate Karachi Red Bus Route Network**:
   - **Route 1 (R1)**: Model Colony / Malir ⇄ Tower (via Sharea Faisal, Karsaz, Nursery, FTC, Metropole, I.I. Chundrigar).
   - **Route 2 (R2)**: North Karachi (4K Chowrangi) ⇄ Tower (via Nagan, Sohrab Goth, Water Pump, Teen Hatti, Guru Mandir, Numaish, Jama Cloth).
   - **Route 3 (R3)**: Nagan Chowrangi ⇄ Singer Chowrangi / Korangi (via Nazimabad, Lasbela, Garden, Tibet Centre, Saddar Empress Market, Korangi Crossing).
   - **Route 9 (R9)**: Gulshan-e-Hadeed ⇄ Tower (via Port Qasim, FAST University, Quaidabad, Malir 15, Sharea Faisal).
   - **Route 10 (R10)**: Numaish ⇄ Dockyard / Kemari (via Saddar Regal, Arts Council, Shaheen Complex, Tower, Custom House).

3. **User Geolocation & Nearest Stop Radar**:
   - Locates user with animated radar pulse beacon (`"You are here"`).
   - Automatically detects nearest Red Bus stop and calculates walking distance in meters and walking time in minutes.
   - Shows live count of approaching buses for that stop.

4. **Stop Details & Arriving Buses**:
   - Displays all incoming buses with distance, travel time, and destination.
   - Lists all reachable destinations from that stop.
   - Instant "Plan Journey From Here" button.

5. **Bus Details Drawer**:
   - Live telemetry: Speed gauge, Direction & Heading in degrees, Current stop, Next stop, Distance to next stop, Assigned driver.
   - Last updated indicator (`"Updated 4s ago"`).

6. **"Which Bus Should I Take?" Route Finder**:
   - Origin and Destination stop selection with auto-complete and direction swap.
   - **Direct Routes**: Recommended buses, ETA to origin stop, total travel time, and stop count.
   - **Multi-Hop Transfer Routes**: Automatic calculation of 1-transfer itineraries with intermediate interchange stations and step-by-step visual timeline!

7. **Dual GPS Engine (Real-World Architecture)**:
   - **DEMO / SIMULATION MODE**: Smooth polyline interpolation along authentic Karachi streets every 4 seconds. Prominently badged with an interactive toggle switch.
   - **Driver GPS Cockpit (`/driver.html`)**: Mobile driver interface streaming live device GPS or test telemetry to `/api/location` via JWT authentication.
   - Seamless transition: when real driver or hardware GPS coordinates arrive, simulation is superseded for that vehicle!

8. **Admin Management Console (`/admin.html`)**:
   - Fleet oversight (Total, Live, Delayed, Stopped, Offline).
   - CRUD management for Buses, Routes, and Bus Stops.
   - Real-time GPS Telemetry Inspector with raw coordinates, speed, and signal health.

9. **Mobile-First Responsive Design**:
   - Optimized for desktop, laptops, tablets, and smartphones.
   - Dedicated mobile bottom navigation bar (`Home`, `Nearby`, `Find Bus`, `Routes`, `Driver`).

---

## 🔑 Default Credentials

| Role | Email | Password |
|------|-------|----------|
| **Admin** | `admin@redbus.pk` | `admin123` |
| **Driver 1** | `driver@redbus.pk` | `driver123` |
| **Driver 2** | `rehman@redbus.pk` | `driver123` |
| **Passenger** | `ali@gmail.com` | `user123` |

---

## 🚀 Running the Project

```bash
cd "C:\Users\F I ENTERPRISES\.gemini\antigravity\scratch\karachi-redbus-tracker"
npm install
node backend/server.js
```

Then visit:
- **Passenger Live Map**: [http://localhost:5000](http://localhost:5000)
- **Driver GPS Portal**: [http://localhost:5000/driver.html](http://localhost:5000/driver.html)
- **Admin Fleet Console**: [http://localhost:5000/admin.html](http://localhost:5000/admin.html)

### Google Maps setup (optional)

The passenger map uses OpenStreetMap until a Google Maps key is configured. Create a Maps JavaScript API key, restrict it to your local website origins (for example, `http://localhost:5000/*` and `http://localhost:5001/*`), and then put it in `public/js/map-config.js`:

```js
window.GOOGLE_MAPS_API_KEY = 'YOUR_GOOGLE_MAPS_KEY';
```

The local config file is ignored by Git. Restart or reload the app after adding the key. If it is blank or rejected, the map falls back to OpenStreetMap. Browser API keys are visible to visitors, so restrict the key by website referrer and API in Google Cloud Console.

## Deploying to Render

1. Push this project to a GitHub repository. Do not add `public/js/map-config.js` or `.env`.
2. In Render, choose **New → Blueprint**, connect the repository, and deploy the included `render.yaml`.
3. When prompted, enter new, strong values for `ADMIN_PASSWORD` and `DRIVER_PASSWORD`. Render generates `JWT_SECRET` for the service.
4. Set `GOOGLE_MAPS_API_KEY` in the Render service environment. Restrict the key to the deployed `https://<your-service>.onrender.com/*` origin and the Maps JavaScript API.
5. Open the deployed `onrender.com` URL after the service becomes live.

The included Render plan is free for a demo: it can spin down when idle, and its filesystem is not durable across deploys. For continuous GPS updates and persistent database changes, use an always-on service with a persistent disk or move the JSON database to a managed database before production use.

---

## 📡 REST API & Socket.IO Endpoints

### REST APIs:
- `GET /api/buses` (filters: `route_id`, `status`)
- `GET /api/buses/:id`
- `GET /api/routes`
- `GET /api/routes/:id`
- `GET /api/routes/:id/buses`
- `GET /api/stops`
- `GET /api/stops/:id`
- `GET /api/stops/:id/buses`
- `GET /api/nearby-stops?lat=...&lng=...`
- `GET /api/route-finder?from=...&to=...`
- `GET /api/eta?bus_id=...&stop_id=...`
- `GET /api/stats`
- `POST /api/auth/login`
- `POST /api/auth/register`
- `POST /api/location` (driver telematics ingestion)
- `POST /api/buses`, `PUT /api/buses/:id`, `DELETE /api/buses/:id` (Admin)
- `POST /api/routes`, `PUT /api/routes/:id`, `DELETE /api/routes/:id` (Admin)
- `POST /api/stops`, `PUT /api/stops/:id`, `DELETE /api/stops/:id` (Admin)

### Socket.IO Events:
- `busLocationUpdate` - live GPS updates
- `busStatusChanged` - status changes
- `busAdded` / `busRemoved` - fleet mutations
- `routeUpdated` - route geometry updates
- `simulationStatusChanged` - simulator start/pause
