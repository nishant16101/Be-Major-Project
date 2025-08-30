# Rough Workflow
---

### 1. **Frontend (Maplibre + Drawing Tools)** --> Research

- **Map Setup**

  - Use **OpenFreeMap tiles** + **Maplibre GL JS**.
  - Load drawing plugin like **[maplibre-gl-draw](https://github.com/maplibre/maplibre-gl-draw)** (a fork of Mapbox Draw).

- **Drawing Interaction**

  - Toolbar gives options:

    - **Point** → drop a marker (like pings).
    - **Polygon** → draw unsafe zone.
    - ~~**Line** → optional (e.g., unsafe road).~~

- **Form Data** --> Find better ways of storing

  - Once drawn, capture geometry + metadata (type, description).
  - Example payload:

    ```json
    {
      "userId": "12345",
      "geometry": {
        "type": "Polygon",
        "coordinates": [
          [
            [77.59, 12.97],
            [77.6, 12.97],
            [77.6, 12.98],
            [77.59, 12.98],
            [77.59, 12.97]
          ]
        ]
      },
      "properties": {
        "type": "accident-prone",
        "description": "Crossroad with many crashes"
      }
    }
    ```

- **Rendering**

  - Fetch stored geometries as **GeoJSON** (`GET /features`).
  - Add them as a source in Maplibre, then apply **style layers**:

    - Points → icons (color based on type).
    - Polygons → semi-transparent fill (e.g., red for unsafe, yellow for noisy).
    - Lines → dashed red/yellow paths.

---

### 2. **Backend (API)**

- Endpoints:

  - `POST /features` → Save a drawn feature (point/line/polygon).
  - `GET /features` → Return all features (filter by bounding box for performance).
  - `DELETE /features/:id` → Remove feature (creator or admin).

- Validation:

  - Geometry should be valid GeoJSON.
  - Properties like `type` should be from a predefined set (unsafe, accident, loud, etc.).

---

### 3. **Database (Postgres + PostGIS)**

- Table: `features`

  | id  | userId | geometry (GEOMETRY) | type | description | createdAt |
  | --- | ------ | ------------------- | ---- | ----------- | --------- |

- Store geometry as **GeoJSON** or directly as **PostGIS geometry**.

- Spatial indexing allows fast queries:

  ```sql
  SELECT * FROM features
  WHERE ST_Intersects(
    geometry,
    ST_MakeEnvelope(minLng, minLat, maxLng, maxLat, 4326)
  );
  ```

  → This fetches only features inside the current map view.

---

### 4. **Optional Enhancements**

- **Clustering points** (already possible in GeoJSON sources).
- **Heatmap layers** for density visualization.
- **Realtime updates** → WebSockets (so new drawings appear instantly).
- **Reputation system** → upvote/downvote zones, hide spam.
- **Temporal features** → expire zones after a time (e.g., loud concerts).

---

## 📊 Visual Workflow (Points + Areas)

1. User logs in → map loads with toolbar (point, polygon, line).
2. User draws on map → fills metadata form → submit.
3. Frontend sends `POST /features` with geometry + metadata.
4. Backend saves in PostGIS.
5. Map fetches latest features (`GET /features`) → renders dynamically.
6. Other users instantly see both pings and zones.

---

## WORK ON ALGORITHM TO FILTER OUT PINGS ON MAP AS SPAM OR ACCORDING TO RELEVANCE
