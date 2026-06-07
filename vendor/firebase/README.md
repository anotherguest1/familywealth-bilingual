# Self-hosted Firebase SDK (for blocked / unreliable networks)

If `www.gstatic.com` / jsDelivr are blocked or unreliable (e.g. mainland China,
strict firewalls, offline), Cloud Sync can fail with **"Firebase SDK failed to
load"**. The app's loader (in `index.html`) tries these locations in order:

1. `./firebase-*-compat.js`            (same folder as index.html)
2. `./vendor/firebase/firebase-*-compat.js`  (this folder)
3. www.gstatic.com  →  www.gstatic.cn  →  cdn.jsdelivr.net

So if you place the three SDK files **here** (or next to `index.html`), the app
loads them from your own origin and never depends on Google's CDN.

## Files needed (Firebase compat SDK v10.12.2)

- `firebase-app-compat.js`
- `firebase-auth-compat.js`
- `firebase-firestore-compat.js`

## How to download them (run on a machine that CAN reach Google, e.g. via VPN)

```sh
cd vendor/firebase
BASE=https://www.gstatic.com/firebasejs/10.12.2
curl -fL -O $BASE/firebase-app-compat.js
curl -fL -O $BASE/firebase-auth-compat.js
curl -fL -O $BASE/firebase-firestore-compat.js
```

(or download them in a browser and copy them into this folder)

Then commit the three `.js` files and redeploy. The version in the filenames /
URL **must stay 10.12.2** to match `SDK_VER` in `index.html`.

> Note: loading the SDK locally only solves the *download*. Firebase's backend
> (`*.googleapis.com` for Firestore/Auth) must still be reachable from the
> device for sync to actually work — in a fully blocked network you need a VPN
> or a non-Google backend.
