# solesociety-uptime

External uptime probe for the Sole Society API, running on an hourly
GitHub Actions schedule (minute 7, dodging top-of-hour cron congestion). Public repo on purpose: scheduled workflows in
public repos consume no billed Actions minutes.

- Probes the API Worker's `/health` endpoint (workers.dev origin).
- Telegram alert on the up → down transition, hourly re-nag while down,
  and a recovery message — silent otherwise.
- `state` / `state_at` track the last known status; the daily heartbeat
  commit keeps GitHub from auto-disabling the schedule after 60 days of
  repo inactivity.

This covers the one failure the Worker's own internal pulse cannot
report: the Worker being dead. Everything else (payment reconciliation,
moderation and dispute thresholds) alerts from inside the Worker — see
`docs/ops/monitoring.md` in the main repo.
