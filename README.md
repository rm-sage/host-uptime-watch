# host-uptime-watch

External (off-host) uptime check for the self-hosted Stremio addon host. Every
on-host monitor (Beszel, `~/container_health_sweep.sh`, the updater pre-check)
dies with the host, so this runs on GitHub's runners.

- `.github/workflows/uptime.yml`: every 15 min, probes AIOStreams
  `/api/v1/status`, AIOMetadata `/health/ready`, Aura `/healthz` and the Beszel
  hub `/api/health` (3 tries, 20 s apart). Pushes to ntfy (repo secret
  `NTFY_URL`) when the failing set changes, every 2 h while down, and on recovery.
  The run also fails while anything is down, so GitHub's failure email is a
  second channel.
- Manual run: Actions -> Host uptime watch -> Run workflow (tick "test_alert" to
  also send a test push).
