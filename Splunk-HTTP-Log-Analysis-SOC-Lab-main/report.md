# SOC Lab Report – Splunk HTTP Log Analysis

## Executive Summary

A JSON-formatted Zeek-style HTTP dataset was searched in Splunk Enterprise 9.3.0. Four SPL searches were used to identify the busiest source IPs, count HTTP 5xx responses, review selected User-Agent values, and locate HTTP responses larger than 500,000 bytes.

The lab demonstrated basic web-traffic triage and SPL aggregation. The results identify areas for further investigation but do not independently confirm malicious activity or data exfiltration.

## Key Results

| Analysis | Observed result |
|---|---|
| HTTP events in source-IP aggregation | 3,000 |
| Top source IP | `10.0.0.28` – 76 events |
| HTTP 5xx responses | 285 |
| Selected User-Agent events | 296 |
| Large response events (>500,000 bytes) | 323 |

## Analysis

The HTTP dataset contains multiple useful fields for SOC triage, including source and responder IPs, URI, status code, User-Agent, timestamp, and response body length.

The selected User-Agent strings may indicate scripted or automated clients, but the header can be spoofed. A 5xx response indicates a server-side error response, not necessarily an attack. Large response bodies may represent ordinary application content or downloads.

## Limitations

- The evidence is based on aggregate searches and selected table rows.
- No full request/response content or complete incident timeline was analysed.
- No confirmed exploitation, compromise, or data exfiltration was established.
- The User-Agent values are not reliable proof of tool identity.

## Recommended Follow-up

Inspect individual events and time sequences, group 5xx responses by exact status code and responder, review the endpoints accessed by selected User-Agent values, and validate whether large responses are expected. Correlate with endpoint, proxy, firewall, and authentication telemetry.

## Conclusion

This lab strengthened practical familiarity with Splunk search, HTTP metadata, status-code filtering, User-Agent analysis, response-size investigation, and SOC-style reporting.
