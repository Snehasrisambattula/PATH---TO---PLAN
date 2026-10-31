# Path to Plan – Smart Travel Planning & Nearby Places Assistant

## Project objective
Path to Plan is a full-stack travel planning assistant. It searches starting locations and destinations, calculates routes, discovers nearby facilities, and saves a travel plan in SQLite.

## Features
- Starting-location and destination search
- Browser "Use My Current Location" with permission handling
- Driving, walking and cycling route modes
- Distance and estimated travel time
- Interactive Leaflet + OpenStreetMap map
- Nearby hospitals, bus stations, railway stations, airports, seaports, hotels and restaurants
- 1 km, 2 km, 5 km and 10 km radius
- Nearest-first sorting
- Category filtering
- Directions to nearby places
- SQLite trip storage
- Multiple destination database structure for future itinerary expansion
- Friendly error handling and no fabricated place information

## Technology
- Frontend: HTML5, CSS3, JavaScript
- Backend: Python + Flask
- Database: SQLite
- Maps: Leaflet + OpenStreetMap
- Geocoding: Nominatim
- Routing: OSRM
- Nearby places: OpenStreetMap Overpass API

## Workflow
Starting Location → Destination → Route → Nearby Facilities → Complete Travel Plan

## Project structure
```text
Path-to-Plan/
├── app.py
├── requirements.txt
├── README.md
├── database/
│   ├── __init__.py
│   ├── db.py
│   └── travel.db
├── templates/
│   ├── base.html
│   ├── index.html
│   ├── plan_trip.html
│   ├── route.html
│   ├── nearby_places.html
│   ├── my_trip.html
│   ├── about.html
│   └── 404.html
├── static/
│   ├── css/style.css
│   └── js/
│       ├── script.js
│       ├── plan.js
│       └── nearby.js
└── services/
    ├── __init__.py
    ├── geocoding.py
    ├── routing.py
    └── nearby_places.py
```

## Database design
### trips
id, start_location, start_latitude, start_longitude, destination, destination_latitude, destination_longitude, distance, estimated_time, created_at

### trip_destinations
id, trip_id, destination_name, latitude, longitude, visit_order

Nearby-place records are retrieved dynamically and are not permanently stored.

## Installation
1. Install Python 3.10+.
2. Open this folder in VS Code.
3. Create a virtual environment:
   ```bash
   python -m venv venv
   ```
4. Activate it on Windows:
   ```bash
   venv\Scripts\activate
   ```
5. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```
6. Start the application:
   ```bash
   python app.py
   ```
7. Open:
   `http://127.0.0.1:5000`

## External services
The application uses public OpenStreetMap ecosystem services. No API keys are hard-coded.
- Nominatim: location search/geocoding
- OSRM: route calculation
- Overpass API: nearby OpenStreetMap features
- Leaflet: interactive map rendering

Public services have usage policies and availability limits. For production-scale deployment, use an appropriate hosted provider or self-host infrastructure and follow each service's policy.

## Data accuracy
The application does not fabricate hospital names, hotels, restaurants, stations, ports, addresses, phone numbers, ratings, prices or availability. Missing fields are shown as "Information not available."

## Limitations
- Public map APIs can be rate-limited or temporarily unavailable.
- OpenStreetMap coverage varies by area.
- Railway/flight schedules are intentionally not provided.
- "Get Directions" opens a map directions page; route calculation inside Path to Plan is used for the main journey.
- Multi-destination storage is included; a full optimized multi-stop route can be added later.

## Screenshots
Add screenshots here after running the project locally.

## Future enhancements
- Full multi-stop itinerary route optimization
- User accounts
- Offline caching
- Public transport schedules through licensed/reliable APIs
- Weather integration
- Trip sharing/export to PDF
- Saved nearby-place favorites
- Production-grade API caching and rate-limit protectio

### Module explanation
- `app.py`: Flask routes and API orchestration.
- `geocoding.py`: Nominatim location search.
- `routing.py`: OSRM route calculation.
- `nearby_places.py`: Overpass category queries and distance sorting.
- `db.py`: SQLite schema and trip persistence.
- Templates/JS/CSS: responsive UI and map interaction.

### Frontend-backend communication
JavaScript uses `fetch()` to call Flask JSON endpoints. Flask validates input, calls a service module, and returns JSON. The browser renders results and markers.

### Database interaction
Flask calls `db.py`, which opens SQLite connections, inserts trips and retrieves/deletes saved trips. Nearby data remains dynamic.

### Geocoding
A text location is sent to Nominatim, which returns coordinates and a display name.

### Route calculation
Coordinates are sent to OSRM. OSRM returns route geometry, distance and duration.

### Nearby-place search
The backend builds category-based Overpass queries around the selected latitude/longitude and radius, then sorts returned places by calculated distance.

### Why OpenStreetMap/Leaflet?
They are open-source/open-data friendly and avoid hard-coded API keys for this project. Leaflet is lightweight and works well with OpenStreetMap tiles.



## Important disclaimer
Nearby place information is provided for reference. Please verify availability and contact details before relying on it.


## Map troubleshooting
If the map appears blank or partially loaded:
1. Make sure the computer has internet access.
2. Open the application through Flask (`http://127.0.0.1:5000`), not by double-clicking an HTML file.
3. Refresh the page after starting Flask.
4. OpenStreetMap tiles are loaded from the public OpenStreetMap tile service, so temporary service/network restrictions can affect map loading.


## Nearby places behavior
- Selecting a start or destination location loads all supported nearby categories within the chosen radius.
- Results are grouped into hospitals, bus stations, railway stations, airports, seaports, hotels, and restaurants.
- Clicking a place name focuses the Leaflet map on that exact OpenStreetMap coordinate and opens its marker popup.
- Nearby data is retrieved dynamically from OpenStreetMap/Overpass; names are not hard-coded.
