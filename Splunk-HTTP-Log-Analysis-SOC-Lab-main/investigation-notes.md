# Investigation Notes – HTTP Log Analysis

## Scope

Review the sample Zeek-style HTTP events ingested into Splunk and summarise:
- Source IP addresses (`id.orig_h`)
- HTTP server errors (`status_code` 500–599)
- Selected User-Agent values
- Large response bodies (`resp_body_len` greater than 500,000 bytes)

## Search context

- Source: `http_logs.json`
- Host filter: `splunkvm`
- Sourcetype: `_json`

These filters reproduce the searches visible in the evidence screenshots.

## Analyst observations

### Top source IPs

The top source IP in the displayed aggregation is `10.0.0.28` with 76 events. `10.0.0.31` and `10.0.0.42` each have 73 events. The search returned 3,000 matching events before aggregation.

A high request count should be compared with normal host behaviour, asset role, and time period.

### HTTP server errors

The 5xx filter returned 285 events. These indicate server-side error responses, but the aggregate count does not reveal the root cause. Individual status codes, responder hosts, URIs, and timestamps should be reviewed.

### Selected User-Agent values

The selected values returned 296 events:
- `sqlmap/1.5.1`: 79
- `python-requests/2.25.1`: 78
- `botnet-checker/1.0`: 70
- `curl/7.68.0`: 69

Some of these strings are associated with automation or security testing. User-Agent values are client-supplied and can be changed or spoofed, so they are clues rather than definitive attribution.

### Large responses

The filter `resp_body_len>500000` returned 323 events. The first visible rows show `/index.html` responses with body lengths around 1.9 million bytes.

Large responses may be expected downloads or application content. Additional context is required before treating them as possible data exfiltration.

## Assessment

**Outcome:** Basic HTTP traffic analysis completed.  
**Incident status:** Several patterns identified for contextual review; no confirmed attack or data exfiltration established by these aggregate searches.

## Recommended follow-up

Review individual events and timelines, group 5xx errors by status and responder, inspect the URIs accessed by selected User-Agent values, and validate whether large responses were expected.
