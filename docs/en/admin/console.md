# Console

The Console tab (admin home) shows traffic & performance stats, auto-refreshing every 5 seconds.

## Stat Cards

| Card | Description |
| --- | --- |
| Total requests | Cumulative API requests (persisted) |
| Total traffic | Cumulative response bytes |
| Unique IPs | Distinct visitor IPs |
| Total danmaku | Stored danmaku count |
| Videos | Video mapping count |
| Subtitles / Banned words | Library sizes |
| Uptime | Service running time |
| Today requests | Requests since local midnight |
| Active IPs (24h) | Visitors active in the last 24 hours |
| Today danmaku | Danmaku sent today |
| New videos today | Mappings added today |

## Performance

- Memory (RSS), heap, CPU (derived from successive samples)
- Requests in the last minute (second-bucket accumulation)
- Node.js version, PID, system / disk usage
- Line chart: memory & CPU trend (10s sampling, ~20 minutes)

## Request Trend

Live request curve for the last 30 minutes (second precision), with total requests and uptime.
