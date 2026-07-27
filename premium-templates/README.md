# Premium Templates catalog

This folder is what the desktop app's Premium Templates gallery fetches at runtime, once a
user has unlocked it with a `premium_templates`-type key (see `db/schema.sql` and
`admin/admin.html` in the `screens` source repo). Nothing here is sensitive — it's fetched over
plain HTTPS, same as `version.json`.

## Shipping a new template

1. In the desktop app, use **Build a template…** to draw the photo/text placeholders and get the
   font/size/color exactly right, then **Save as template**.
2. Open **Settings → Export template package…**, pick a folder. This copies the template's
   manifest (`<id>.json`) and background image, unmodified, into that folder.
3. Copy both files into this folder.
4. Add an entry to `index.json`:

   ```json
   {
     "id": "<id>",
     "name": "Human-readable name shown in the gallery",
     "category": "A short category label",
     "manifestFile": "<id>.json",
     "imageFile": "<id>.png",
     "version": 1
   }
   ```

5. Commit and push. The app re-fetches `index.json` every time a user opens the Premium
   Templates gallery — no rebuild or release needed.

## Updating a template you already shipped

Repeat steps 1–3 for the revised version, overwriting the same `<id>.json`/`<id>.png` in this
folder, then **bump `version`** on that entry in `index.json` (2, 3, …) and push. Every user's
app re-checks each template's version the moment they open the gallery and silently re-downloads
anything the admin has bumped — there's no separate "check for updates" step for them to find.
Forgetting to bump `version` means the new files sit here unused; every existing install keeps
using its already-cached copy.

Every field in a template loaded from this folder has its font/size locked regardless of what
`userEditableColor`/`userEditableSize` say in the manifest — see `TemplateService.LoadFrom` in
the source repo. Users can only change the *value* of each field, never its styling.
