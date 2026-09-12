# TruckersMP Tracker

Live server status, player lookup, events and in-game time for [TruckersMP](https://truckersmp.com).

**Live site:** https://tmoneytalkss.github.io/truckersmp-tracker/

## Why data sometimes fails to load

The official TruckersMP API does **not** send CORS headers. Browsers therefore block direct requests from GitHub Pages (and any other website). Free public CORS proxies are often rate-limited or offline, so the page may show a CORS error.

### Permanent free fix (recommended, ~2 minutes)

1. Go to [Cloudflare Workers](https://workers.cloudflare.com/) and create a free account if needed.
2. Create a new Worker and paste this code:

```js
export default {
  async fetch(request) {
    const url = new URL(request.url);
    const target = url.searchParams.get('url');
    if (!target || !target.startsWith('https://api.truckersmp.com/')) {
      return new Response('Missing or invalid url param', { status: 400 });
    }
    const res = await fetch(target, {
      headers: { 'User-Agent': 'TruckersMP-Tracker-Proxy' }
    });
    const body = await res.arrayBuffer();
    return new Response(body, {
      status: res.status,
      headers: {
        'Content-Type': res.headers.get('Content-Type') || 'application/json',
        'Access-Control-Allow-Origin': '*',
        'Cache-Control': 'public, max-age=30'
      }
    });
  }
};
```

3. Deploy the Worker and copy its URL (e.g. `https://tmp-proxy.yourname.workers.dev`).
4. In `index.html`, change the `PROXIES` array so your Worker is first:

```js
const PROXIES = [
  (url) => `https://tmp-proxy.yourname.workers.dev/?url=${encodeURIComponent(url)}`,
];
```

5. Commit & push — the tracker will work reliably.

## Features

- Live server list with player counts, queues and capacity bars
- Player lookup (TMP ID or SteamID64)
- Events (now / today / featured / upcoming)
- Approximate in-game time
- Auto-refresh every 60s for servers
- Dark futuristic UI, mobile-friendly

## License

MIT — not affiliated with TruckersMP.
