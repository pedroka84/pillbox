# Pillbox — PWA setup

This folder contains a installable, offline-capable version of Pillbox:

```
pillbox-pwa/
├── index.html
├── manifest.json
├── service-worker.js
└── icons/
    ├── icon-192.png
    ├── icon-512.png
    └── icon-maskable-512.png
```

## Why you need to host it (can't just double-click index.html)

Browsers only allow service workers (what makes offline support and "Install"
work) on pages served over **https://**, or on **http://localhost**. Opening
the file directly from disk (`file://...`) will not register the service
worker or show an install prompt — the app will still work as a normal
webpage, just without the "installable" part.

## Option 1: Try it locally first

From inside the `pillbox-pwa` folder, run one of these (whichever you have
installed), then open the printed address in your browser:

```
python3 -m http.server 8000
```
or
```
npx serve .
```

Then visit `http://localhost:8000`. You should see an "Install app" button
appear in the header (Chrome/Edge), or you can use your browser's
"Add to Home Screen" / "Install" option manually (Safari on iOS, Firefox).

## Option 2: Put it on the web for real (free options)

Any static hosting works, since there's no server-side code. Two easy, free
choices:

- **GitHub Pages** — push this folder to a GitHub repo, enable Pages in the
  repo settings, done.
- **Netlify / Vercel** — drag-and-drop the `pillbox-pwa` folder onto their
  web dashboard and it deploys in seconds.

Once it's live on an https:// address, visit it on your phone or desktop
and use the browser's install option (or the in-app "Install app" button).

## Notes

- All medication data still lives only in the browser's `localStorage` on
  each device — hosting the files publicly does not mean anyone else can
  see your data. Only the app's own code (HTML/CSS/JS/icons) is public,
  the same as any website's source code.
- Because data is per-device and per-browser, installing the app on your
  phone and on your laptop will keep two separate histories. There's no
  sync between them without adding a backend, which is outside what this
  local-only version does.
- If you ever update `index.html`, bump `CACHE_NAME` in
  `service-worker.js` (e.g. `pillbox-v2`) so returning visitors get the
  new version instead of a stale cached copy.
