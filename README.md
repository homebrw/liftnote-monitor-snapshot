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

The repository was set to **private** at the owner's request. Since `homebrw/unicorn-cf-prog#1009`, publishing
is also **opt-in**: without `LIFTNOTE_MONITOR_SNAPSHOT_ENABLED=1`, the orchestrator makes no git/GitHub call to
this repo at all and can never recreate it. So:

- no new snapshots are written here;
- if the opt-in is set while the repo is private, the publisher refuses to write and logs
  `Remote monitor publish skipped`, without blocking lots;
- `/liftnote/monitor` can no longer read the snapshot, because it reads without authentication.

## If you want to change this later

- **Re-enable the remote monitor:** set `LIFTNOTE_MONITOR_SNAPSHOT_ENABLED=1` on the PC running the orchestrator
  **and** set the repo back to public, or change the app to read it through an authenticated server route.
- **Delete it:** safe while the opt-in is unset. If the opt-in is set later and the repo is missing, the first
  publish recreates it as **public**.

Full decision log: `docs/liftnote/LOCAL_ORCHESTRATOR.md` → "Snapshot repository — status and decision log"
in `homebrw/unicorn-cf-prog`.
