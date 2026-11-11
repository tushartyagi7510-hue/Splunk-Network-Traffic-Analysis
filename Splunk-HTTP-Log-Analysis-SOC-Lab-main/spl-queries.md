# SPL Queries – HTTP Log Analysis

> These searches use the source, host, and sourcetype filters visible in the submitted screenshots. Adjust the base search if your Splunk ingestion settings differ.

## 1. Top 10 source IPs generating HTTP traffic

```spl
source="http_logs.json" host="splunkvm" sourcetype="_json"
| stats count by "id.orig_h"
| sort -count
| head 10
```

- `stats count by "id.orig_h"` counts events for each originating IP.
- `sort -count` sorts from highest count to lowest.
- `head 10` returns the first 10 rows.

## 2. Count HTTP server errors (5xx)

```spl
source="http_logs.json" host="splunkvm" sourcetype="_json" status_code>=500 status_code<600
| stats count as server_errors
```

This filters status codes from 500 through 599 and counts matching events.

## 3. Count selected User-Agent values

```spl
source="http_logs.json" host="splunkvm" sourcetype="_json" user_agent IN ("sqlmap/1.5.1", "curl/7.68.0", "python-requests/2.25.1", "botnet-checker/1.0")
| stats count by user_agent
```

This filters for the four listed User-Agent values and counts events for each value.

## 4. Find large HTTP responses

```spl
source="http_logs.json" host="splunkvm" sourcetype="_json" resp_body_len>500000
| table ts "id.orig_h" "id.resp_h" uri resp_body_len
| sort -resp_body_len
```

This selects responses larger than 500,000 bytes, displays selected fields, and sorts the largest response bodies first.

## SPL and HTTP terms

- `source`: filters by the input source name.
- `host`: filters by the host value assigned by Splunk.
- `sourcetype`: identifies how Splunk parsed the event.
- `stats`: calculates a summary from matching events.
- `count`: counts events.
- `by`: groups results by a field.
- `sort -field`: sorts in descending order.
- `head 10`: returns the first 10 rows.
- `table`: displays only the specified fields.
- `id.orig_h`: Zeek-style originating host IP field.
- `id.resp_h`: Zeek-style responding host IP field.
- `status_code`: HTTP response status code.
- `user_agent`: client-provided User-Agent string.
- `resp_body_len`: response body length in bytes.
- `ts`: event timestamp.
- `uri`: requested URI path.
