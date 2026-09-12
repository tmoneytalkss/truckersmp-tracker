# TruckersMP Tracker

Live server status, player lookup, events and in-game time for [TruckersMP](https://truckersmp.com).

**Live site:** https://tmoneytalkss.github.io/truckersmp-tracker/

## How it works

The TruckersMP API blocks browser requests (no CORS headers).  
This site therefore:

1. Stores live data as JSON files in `data/`
2. A **GitHub Action** updates those files every 5 minutes
3. The page loads `./data/*.json` from the same origin → **no CORS problems**

Player lookup still tries the live API (may fail due to CORS).

## Enable automatic updates

1. Open the repo → **Actions** tab
2. Enable workflows if prompted
3. Run **Update TruckersMP data** manually once (or wait for the schedule)

## Features

- Live server list with player counts & capacity bars
- Events (now / today / featured / upcoming)
- Approximate in-game time
- Player lookup (best-effort)
- Dark futuristic UI

## License

MIT — not affiliated with TruckersMP.
