# SPL Queries – DNS Log Analysis

> These searches use the exact source, host, and sourcetype filters shown in the submitted Splunk screenshots. If your Splunk setup uses another index or sourcetype, change the base search accordingly.

## 1. Count DNS query types

```spl
source="dns_logs.json" host="splunkvm" sourcetype="_json"
| stats count by qtype
```

**What it does:** Groups events by the `qtype` field and counts events in each group.

## 2. Find the most active originating IPs

```spl
source="dns_logs.json" host="splunkvm" sourcetype="_json"
| stats count by "id.orig_h"
| sort -count
```

**What it does:** Counts events for each originating IP and sorts the highest counts first.

## 3. Find the most frequently queried domains

```spl
source="dns_logs.json" host="splunkvm" sourcetype="_json"
| stats count by query
| sort -count
```

**What it does:** Counts each value in the `query` field and sorts the most common names to the top.

## SPL terms used

- `source`: filters events by the input source name.
- `host`: filters events by the host value assigned by Splunk.
- `sourcetype`: identifies how Splunk parsed the event data.
- `stats`: calculates a summary from matching events.
- `count`: counts events.
- `by`: groups the results by a field.
- `sort -count`: sorts the count field in descending order.
- `id.orig_h`: Zeek field representing the originating host address in the connection/event context.
- `qtype`: DNS query type.
- `query`: requested DNS name.
