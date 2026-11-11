# Investigation Notes – DNS Log Analysis

## Scope

Review the sample Zeek DNS events ingested into Splunk and summarise:
- DNS query types (`qtype`)
- Originating IP addresses (`id.orig_h`)
- Requested domain names (`query`)

## Data and search context

- Source: `dns_logs.json`
- Host filter: `splunkvm`
- Sourcetype: `_json`
- Events returned: 1,200

The exact search filters depend on how the data was uploaded. The searches in this project reproduce the filters visible in the evidence screenshots.

## Analyst observations

### DNS query types
- AAAA: 321
- A: 313
- PTR: 309
- CNAME: 257

The counts add up to 1,200. AAAA is the most common query type in this sample, but the difference between categories is not, by itself, suspicious.

### Source IP activity
The highest counts shown are 113 requests from `192.168.1.10` and 113 from `192.168.1.18`. These are private IPv4 addresses. Their activity should be compared with the expected role of each device before deciding whether it is unusual.

### Requested domains
`google.com` and `printer.local` are tied at 139 requests each. Internal-looking names such as `printer.local`, `fileserver.local`, and `router.local` may be expected in a lab network. Domain names should be assessed in context.

## Assessment

**Outcome:** Basic DNS activity analysis completed.  
**Incident status:** No confirmed malicious activity established by these three aggregations alone.

A SOC analyst would need additional context—timestamps, full DNS records, endpoint ownership, query frequency over time, and threat-intelligence results—before escalating a suspicious domain or host.
