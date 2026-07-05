# toggl-2-prototype

Toggl 2.0 prototype — Project Variance Alerts

A self-contained, static prototype exploring estimate-overrun alerts for Toggl 2.0 (timer, weekly calendar, projects list, project dashboard, log-time modal, and toast notifications).

No backend, no build step — it's a single HTML page that runs React from a CDN.

## Run locally

Serve the folder over HTTP (the runtime does a same-origin `fetch` on load, so `file://` won't work):

```bash
python3 -m http.server 8000
```

Then open http://localhost:8000/.

## Availability

Live prototype: https://toggl-2-prototype.vercel.app/
