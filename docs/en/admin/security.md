# Security

## IP Stats

Per-window (5m / 1h / 24h / all) request & traffic stats, with search, sorting and pagination.

## Anomaly Detection

IPs exceeding thresholds are flagged with reasons and can be banned with one click:

- requests per minute / hour
- MB per minute / hour

## Ban & Whitelist

- **Ban**: blocks all access from that IP (persists across restarts)
- **Whitelist**: bypasses PoW / rate limits / danmaku limits and is excluded from anomaly detection

## Login Records & Protection

- Every login attempt is recorded (success / fail / locked) with IP, account, region and reason
- **Fail lockout**: configure max failures, window and lock minutes; locked IPs are denied login
- One-click ban / whitelist from the records

## Geo Info

- Built-in ip2region v4 / v6 databases (xdb), updatable with one click
- ECharts heat maps (world / China) for IP distribution (requires CDN access)

## Firewall

PoW, global rate limiting and danmaku frequency limits are configured under Server Config.
