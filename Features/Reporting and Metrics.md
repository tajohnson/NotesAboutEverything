# Reporting and Metrics

## TODOs
- [ ] Store metrics in leveldb via statsd (per domain, per user)
- [ ] Set up rollup/retention: 1m:7d, 1h:90d, 1d:5y
- [ ] Store metrics for customer servers deferring mail
- [ ] Set up alerting on deferrals
- [ ] Interface for reporting in control panel

---

## Implementation Plan

### 1. Store metrics in leveldb via statsd
- Use translate feature in telegraf to turn a statsd keyname into something useful with tags
- Series:
  - 1 per domain
  - 1 per user
  - Do we need to do it for customer/reseller? Maybe yes for customer, no for reseller

### 2. Set up rollup/retention
```
1m:7d,1h:90d,1d:5y
```
(by the minute for a week, by the hour for 90 days, then daily for 5 years)

Over 40 metrics, about 6.4 per user. So 500K users is only 3.07 TB.

### 3. Store metrics for customer servers deferring mail
- When we have mail in the queues for retry
- Store per domain? Or per customer?
- Then set up a way to alert on these

### 4. Interface for reporting in control panel

---

## Metrics Hierarchy Ideas

```
postfix.relaycust.deferred.domain
```

If use statsd and use gauge, then in datadog, you can "aggregate with the sum of reported values". Can you do this with influxdb?

---

## Metrics List

### Per-domain AND per-user

**Inbound:**
- Inbound bandwidth
- Average Size
- Total Msg Count
- Spam, Virus, BadHeader, etc.

**Outbound:**
- (TBD)

**Other:**
- Top viruses
- Quarantine Releases

### Per-inbound Server
- Mail delivery
- Deferred count, instantaneous
