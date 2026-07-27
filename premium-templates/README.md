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
     "imageFile": "<id>.png"
   }
   ```

5. Commit and push. The app re-fetches `index.json` every time a user opens the Premium
   Templates gallery — no rebuild or release needed.

Every field in a template loaded from this folder has its font/size locked regardless of what
`userEditableColor`/`userEditableSize` say in the manifest — see `TemplateService.LoadFrom` in
the source repo. Users can only change the *value* of each field, never its styling.
