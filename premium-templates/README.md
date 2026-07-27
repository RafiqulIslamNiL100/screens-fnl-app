# Premium Templates catalog (deprecated location)

**As of Screens v1.11.0, this folder is no longer read by the app.** The Premium Templates
catalog moved to the same Supabase project everything else already uses: a
`premium_templates_catalog` table plus a `premium-templates` Storage bucket (see
`db/schema.sql` in the `screens` source repo). `index.json` below is now historical only — it
is not fetched by any shipped version of the app anymore.

## Publishing a template now

An admin account (one with a row in `public.admins` — see `docs/SETUP.md` in the `screens`
repo for how to grant that) builds or scans a template in the desktop app as normal, then clicks
**Settings → Publish as Premium Template**. This uploads the manifest+image straight to Supabase
Storage and upserts the catalog row — live for every user with a `premium_templates` key the
moment they next open the gallery. No file in this repo needs to be touched, and no commit/push
is needed for a new or updated template anymore.

## Why this moved

The previous flow (export a package here, hand-edit `index.json`, commit, push) required a
GitHub commit for every single template, which only the people with push access to this repo
could do. Publishing directly from inside the app needed a backend the app could write to under
its own authenticated session with server-side rules enforcing "only admins can write" — which
meant Supabase (already the project's auth/license backend) rather than this static-file repo,
since embedding a GitHub write-credential in a publicly distributed desktop app would let anyone
who decompiled it push to this repo too.

This folder is kept only so old installs pointed at these exact URLs (versions before v1.11.0)
keep working until everyone has updated; it will not receive new templates.
