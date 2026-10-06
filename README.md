# liftnote-monitor-snapshot
Sanitized public snapshots for the LiftNote autonomous monitor.

## What this repository is

- Created **automatically** on 2026-09-30 by the LiftNote local orchestrator in
  [`homebrw/unicorn-cf-prog`](https://github.com/homebrw/unicorn-cf-prog), not by hand and not by an agent session.
  The code is `scripts/liftnote-monitor-snapshot.mjs`, function `ensureSnapshotRepo()`. It calls `POST /user/repos`
  on the first publish if this repo does not exist.
- It ran on the owner's PC with the owner's Git credential, which is why the repo and every
  `snapshot: <ISO date>` commit are under the owner's account.
- `latest.json` is the sanitized orchestrator state: current lot, history, gate, PR numbers, SHAs.
  It contains no tokens, chat URLs or local paths.
- The `/liftnote/monitor` page of the app reads it (`src/components/liftnote/liftnote-remote-monitor.tsx`).

## Status — 2026-10-06: private (decided by the owner)

The repository was set to **private** at the owner's request. With the current code:

- the publisher refuses to write to a private repo, so snapshots stop. The orchestrator logs
  `Remote monitor publish skipped` and keeps running lots;
- it will **not** recreate a public copy while this private repo exists;
- `/liftnote/monitor` can no longer read the snapshot, because it reads without authentication.

## If you want to change this later

- **Re-enable the remote monitor:** set the repo back to public, or change the app to read it through an
  authenticated server route.
- **Delete it:** also stop or disable the publisher, otherwise the next orchestrator pass recreates it as public.

Full decision log: `docs/liftnote/LOCAL_ORCHESTRATOR.md` → "Snapshot repository — status and decision log"
in `homebrw/unicorn-cf-prog`.
