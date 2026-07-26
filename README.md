# Screens — distribution

This repo holds only build artifacts for **Screens**, a Windows desktop
app for filling in professional image templates. Source code lives in
[`RafiqulIslamNiL100/screens`](https://github.com/RafiqulIslamNiL100/screens).

## Download

- **Installer:** [`Screens-Setup-1.0.0.exe`](./Screens-Setup-1.0.0.exe) —
  per-user install, no admin rights required, no UAC prompt.
- **Admin dashboard:** [`admin.html`](./admin.html) — download and
  double-click, works in any browser, no build step.

## Verify the installer

```
sha256sum Screens-Setup-1.0.0.exe
```

Compare against the `sha256` field in [`version.json`](./version.json).

## Updating

The app checks `version.json` in this repo on startup to offer in-app
updates. See `docs/RELEASE.md` in the source repo for how new versions are
published here.
