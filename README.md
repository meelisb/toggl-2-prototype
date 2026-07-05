# toggl-2-prototype

Toggl 2.0 prototype — Toggl Estimate Alerts

A self-contained, static prototype exploring estimate-overrun alerts for Toggl Track (timer, weekly calendar, projects list, project dashboard, log-time modal, and toast notifications).

No backend, no build step — it's a single HTML page that runs React from a CDN.

## Run locally

Serve the folder over HTTP (the runtime does a same-origin `fetch` on load, so `file://` won't work):

```bash
python3 -m http.server 8000
```

Then open http://localhost:8000/.

## Deploy to GitHub Pages

1. Push this repo to GitHub.
2. In repo Settings → Pages, set source to the `main` branch, root folder.
3. The site will be published at `https://<user>.github.io/<repo>/`.
