# find-my-room

Indoor campus navigation: search for a room and get walked to it on a floor plan.
A team project (Elaine Chen, Karen Agustino, Jasper He, Simarjot Singh) built with
Next.js, Supabase, and Mapbox.

> 📸 **Screenshot needed**: the navigate view, a floor plan with rooms drawn on it and the search bar open. Save to `docs/navigate.png` and replace this line with `![Navigate view](docs/navigate.png)`.

> 📸 **Screenshot needed**: the admin map editor with a room polygon being drawn on the Mapbox canvas. Save to `docs/admin.png` and replace this line with `![Admin editor](docs/admin.png)`.

## Quickstart

The app lives in the `room-finder/` subdirectory.

```bash
git clone https://github.com/simar-s2/find-my-room.git
cd find-my-room/room-finder

npm install
# create room-finder/.env.local with the values in the table below
npm run dev
```

Open http://localhost:3000.

### Environment

| Variable | Used for |
|---|---|
| `NEXT_PUBLIC_SUPABASE_URL` | Supabase project URL |
| `NEXT_PUBLIC_SUPABASE_PUBLISHABLE_KEY` | Supabase anon/publishable key |
| `NEXT_PUBLIC_MAPBOX_TOKEN` | Mapbox GL token (map + draw tools) |
| `GOOGLE_API_KEY` | Google GenAI (optional helper features) |

## How it works

**Data lives in Supabase**, three tables in a `buildings → floors → rooms`
hierarchy. Each row keeps a freeform `json` (jsonb) column holding geometry and
properties, so a room's shape and metadata travel together:

```
buildings (id, name, address, floors, json)
    └─ floors (id, building_id, name, rooms, json)
        └─ rooms (id, floor_id, name, json)   <- polygon / centroid / props
```

**Two sides to the app:**

- **Navigate** (`app/navigate/...`): pick a building, pick a floor, search rooms
  (`ilike` against the room name), and see them on a pannable, zoomable floor
  plan. Queries run straight from the browser via the Supabase client.
- **Admin** (`app/admin`): a Mapbox GL canvas with `mapbox-gl-draw`. You draw a
  room as a polygon, name it, and `saveRoomToSupabase()` writes the geometry into
  the `rooms` table. This is how the floor data gets created in the first place.

Built on the Next.js App Router (Turbopack), with shadcn/ui and Radix components
and Framer Motion for the navigate interactions.

## What it does

- Browse buildings and floors
- Search for a room by name and locate it on the floor plan
- Pan / zoom / switch floors
- Admin tool to digitise floor plans by drawing rooms on a map

## Built with

Next.js 15 · TypeScript · Supabase · Mapbox GL + mapbox-gl-draw · SVG.js · Tailwind · shadcn/ui · Framer Motion

## License

See [LICENSE.md](LICENSE.md).
