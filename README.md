# FlowPros Mobile API — PostPilot workspace

Every HTTP endpoint the **FlowPros mobile app** calls, as a ready-to-run
[PostPilot](https://github.com/ManzurulIslamBista/postpilot) workspace:
**23 requests** in 6 folders, with **Testing** and **Production** environments.

The requests were read from the app's own source
(`FlowPros-MobileApp`, branch `features/offline-support`, commit `8130f6a`);
each request's description names the file and method it came from.

| Folder | Requests |
|---|---|
| Auth & Session | Login, Refresh access token, Get app settings, Get user profile |
| Jobs (tasks) | List (paginated), Incremental sync, Get one, Date range, Create, Update, Save draft, Delete, Bulk update ×2, Upload image |
| Projects & Stages | Project details, Task stage details, Stage-wise job count |
| Map | Job map list |
| Field configuration & master data | Field group list, Relational master data (bulk), Model data (targeted) |
| External (Google Routes) | Compute route |

## Open it in PostPilot

`workspace.json` is a normal PostPilot workspace file, so any of these works:

1. **Desktop (folder):** clone this repo, then *Workplace switcher → Add Workplace…*
   and choose the cloned folder. A folder that already holds a `workspace.json`
   is opened as it is, never overwritten.
2. **Desktop or web (Git):** *Add Workplace…* → tick **Connect Git Repository** →
   paste this repository's URL, branch `main` and a GitHub **classic** token with
   the `repo` scope. PostPilot pulls `workspace.json`; *Sync with Git* pushes your edits back.
3. **Any platform (paste):** *Import…* in the collections sidebar → paste the
   whole contents of `workspace.json`.

## Use it

1. Pick **Testing** or **Production** in the top bar.
2. Fill `username` and `password` in that environment (leave them out of Git).
3. Run **Auth & Session → Login**. Its extractors save `accessToken` and
   `refreshToken` into the active environment.
4. Everything else inherits `access-token: {{accessToken}}` from the collection's auth.

Conventions worth knowing: authentication is the custom header `access-token`
(not `Authorization: Bearer`); every response is `{ "status": true|false, … }`;
`last_sync_time`, `date_from` and `date_to` are epoch **seconds**; optional query
parameters are listed but disabled until you tick them.

> **Running in a browser?** The FlowPros servers send no CORS headers, so a browser
> blocks the calls. Use PostPilot on desktop or Android, or a browser started without
> web security for testing.

## Secrets

Nothing sensitive is stored here: `password`, `accessToken`, `refreshToken` and
`googleMapsApiKey` are empty secret variables, and the Git token is never written to
`workspace.json`. Put real values in your local environment only.
If you commit changes made in PostPilot, check the diff for secrets first.

## Notes

- *Create job* and *Bulk update — select all* use sample bodies: the mobile app defines
  those calls but builds the payload elsewhere, so adjust the field names to your server.
- *Upload image* needs a file part (`image_file`); PostPilot's form-data editor is text-only,
  so the request documents the equivalent `curl`.
