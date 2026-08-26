# daily-log

A scheduled heartbeat. Once an hour a workflow bumps a counter in
[`activity-log.json`](./activity-log.json), commits it, and pushes.

That is the whole repository. It is here so that it is *only* here.

## Why it exists as its own repo

The same job used to run inside four real repositories — the profile README,
`STC`, `lunora-studio` and `portfolio`. Every run added a commit that changed
nothing in that project, so `git log` and the blame view of actual work were
interleaved with noise, and `git bisect` had to step over commits that could not
possibly be the cause of anything.

Moving it here costs those repositories nothing and gives them their history
back. Nothing in this repo is presented as project work.

## How it works

| | |
|---|---|
| Schedule | `0 * * * *` — the top of every hour, UTC |
| Manual run | the **Actions** tab → Heartbeat → *Run workflow* |
| Permissions | `contents: write`, and nothing else |
| Concurrency | one run at a time, so a delayed run cannot race the next |
| On push conflict | rebase and retry, up to three times |

GitHub's cron is best-effort. Under load a scheduled run can be delayed or
dropped, so the counter is approximate — it is a heartbeat, not a clock.

## Turning it off

Disable **Heartbeat** in the Actions tab, or delete
`.github/workflows/heartbeat.yml`. Nothing else depends on it.
