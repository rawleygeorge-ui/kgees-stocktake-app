# KGEES Stock Take – installable app wrapper

Turns the Stock Take Apps Script web app into an installable phone app (PWA), hosted free on GitHub Pages,
the same way as the KGEES FSR app. It opens on the **Unloading** screen; long-press the icon for
**Unloading**, **Van return** or **Van load**.

| File | What it does |
|---|---|
| `index.html` | Splash, then loads Stock Take inside it. **You edit one line here.** |
| `manifest.json` | App name, icon, colours, full-screen, long-press shortcuts. |
| `sw.js` | Makes it installable and shows a "No signal" screen when offline. |
| `icons/` | KGEES Fine Foods logo with "STOCK TAKE" below. |

## Step 1 – Stock Take must allow being opened inside the wrapper

Already done in the code (`Code.js` → `doGet` uses `XFrameOptionsMode.ALLOWALL`).
Make sure that version is live: on the PC run `git pull`, then `clasp push`, then
**Deploy → Manage deployments → ✏️ Edit → Version: New version → Deploy**.
Copy the **Web app URL** (ends in `/exec`).

## Step 2 – Paste the URL into `index.html`

```js
const APP_URL = 'PASTE_YOUR_APPS_SCRIPT_EXEC_URL_HERE';
```

To open on a different screen, change `const DEFAULT_TAB = 'unload';` to `'van'`, `'load'` or `'count'`.

## Step 3 – Create the GitHub repo

1. github.com → **New repository** → name `kgees-stocktake-app` → **Public** → **Create repository**.
   (Public is safe: it only holds this wrapper and the `/exec` link, not the app's code or data.)
2. **uploading an existing file** → drag in `index.html`, `manifest.json`, `sw.js`, `README.md` **and the `icons` folder** → **Commit changes**.

## Step 4 – Turn on GitHub Pages

**Settings → Pages → Deploy from a branch → main → / (root) → Save.** After 1–2 minutes:
`https://rawleygeorge-ui.github.io/kgees-stocktake-app/` – that link is the app.

## Step 5 – Test, then send to the factory team

1. Open the link in **Chrome** on an Android phone. Stock Take appears after the splash, on Unloading.
2. Tap **Install** (or ⋮ → **Install app**). Open it from the home-screen icon: full screen, no browser bar.
3. Count a test item, then check it in Totals.
4. WhatsApp to staff: "Open in Chrome → tap Install → use the Stock Take icon from now on."

Other screens: `…/kgees-stocktake-app/?tab=van` opens Van return, `?tab=load` Van load.

## Troubleshooting

**Blank screen or "script.google.com refused to connect"** – Step 1's new version isn't deployed yet.
If it still fails, change `const MODE = 'frame';` to `'redirect'` in `index.html`.

**Camera / photo button does nothing** – long-press the app icon → App info → Permissions → Camera → Allow.

**Phones show an old version of the wrapper** – in `sw.js` change `kgees-stocktake-v1` to `v2` (then `v3`…) and upload again.
Changes to Stock Take itself show up as soon as you deploy a new version in Apps Script.

**Optional APK** – pwabuilder.com → paste the github.io link → Android → Generate.
