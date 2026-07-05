# toggl-2-prototype

Toggl 2.0 prototype — Project Variance Alerts

A feature to prevent project scope creep for individual contributors, give contextual warnings about hitting project estimations for all users and turning time data into actionable insights that help people and teams plan better.

A self-contained, static prototype exploring estimate-overrun alerts for Toggl 2.0:
- Logged-entry crossing toast notifications
- Planned-entry crossing toast notifications
- Logged-entry crossing sidebar banner
- Link to project dashboard
- Variance column tooltip

No backend, no build step — it's a single HTML page that runs React from a CDN.

## Run locally

Serve the folder over HTTP (the runtime does a same-origin `fetch` on load, so `file://` won't work):

```bash
python3 -m http.server 8000
```

Then open http://localhost:8000/.


## Artifacts

- Live prototype: https://toggl-2-prototype.vercel.app/
- Video walkthrough: https://www.loom.com/share/896730e86e83474d8e9a0f92c9e5da64
- Documents:
  - Prototype Brief
  - Requirements Specification