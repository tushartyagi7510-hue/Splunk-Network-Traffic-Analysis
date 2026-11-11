# Splunk HTTP Log Analysis | SOC Lab

**Category:** Security Operations / SIEM / Web Traffic Analysis  
**Tool:** Splunk Enterprise 9.3.0  
**Data:** JSON-formatted Zeek-style HTTP logs  
**Author:** Tushar Tyagi

## Project Overview

This project demonstrates a basic SOC investigation workflow using HTTP logs in Splunk. I searched a JSON-formatted HTTP dataset with Splunk Search Processing Language (SPL) to review the busiest source IPs, HTTP server errors, selected User-Agent strings, and large HTTP responses.

The goal was to practise web-traffic triage and understand how an analyst can use HTTP metadata to identify activity that may need further investigation.

## Objectives

- Ingest and search HTTP logs in Splunk.
- Identify source IP addresses generating the most web traffic.
- Count HTTP server responses in the 5xx range.
- Review selected User-Agent strings associated with common command-line or scripted tools.
- Identify HTTP responses larger than 500 KB.
- Document observations without treating an indicator alone as proof of an attack.

## Lab Environment

| Component | Details |
|---|---|
| SIEM | Splunk Enterprise 9.3.0 |
| Log format | JSON |
| Dataset | Zeek-style HTTP logs |
| Source filter in screenshots | `http_logs.json` |
| Host filter in screenshots | `splunkvm` |
| Sourcetype in screenshots | `_json` |

The lab handout suggests an index such as `http_lab` and sourcetype `json`. My screenshots show searches using `source="http_logs.json"`, `host="splunkvm"`, and `sourcetype="_json"`. These filters reflect my actual Splunk ingestion configuration.

## Investigation Workflow

1. Uploaded the JSON HTTP log file through **Splunk Web → Settings → Add Data → Upload**.
2. Confirmed that Splunk returned events for the selected source.
3. Grouped events by originating IP address to identify the busiest clients.
4. Counted responses with status codes from 500 through 599.
5. Counted events containing four selected User-Agent values.
6. Reviewed responses larger than 500,000 bytes.
7. Saved screenshots of the SPL searches and results as evidence.

## SPL Queries and Results

The following searches reproduce the filters and queries visible in my screenshots.

### 1. Top 10 source IPs generating HTTP traffic

```spl
source="http_logs.json" host="splunkvm" sourcetype="_json"
| stats count by "id.orig_h"
| sort -count
| head 10
```

**Top results visible in the screenshot:**

| Source IP | Event count |
|---|---:|
| 10.0.0.28 | 76 |
| 10.0.0.31 | 73 |
| 10.0.0.42 | 73 |
| 10.0.0.27 | 72 |
| 10.0.0.40 | 70 |
| 10.0.0.45 | 70 |
| 10.0.0.14 | 69 |
| 10.0.0.50 | 67 |
| 10.0.0.13 | 65 |
| 10.0.0.25 | 65 |

The search returned 3,000 matching events before aggregation.

### 2. Count HTTP server errors (5xx)

```spl
source="http_logs.json" host="splunkvm" sourcetype="_json" status_code>=500 status_code<600
| stats count as server_errors
```

**Observed result:** 285 server-error events.

HTTP 5xx status codes indicate server-side error responses. The count alone does not explain the cause; individual status codes, endpoints, timestamps, and server-side logs would be needed for diagnosis.

### 3. Review selected User-Agent strings

```spl
source="http_logs.json" host="splunkvm" sourcetype="_json" user_agent IN ("sqlmap/1.5.1", "curl/7.68.0", "python-requests/2.25.1", "botnet-checker/1.0")
| stats count by user_agent
```

**Observed results:**

| User-Agent | Count |
|---|---:|
| sqlmap/1.5.1 | 79 |
| python-requests/2.25.1 | 78 |
| botnet-checker/1.0 | 70 |
| curl/7.68.0 | 69 |
| **Total matching events** | **296** |

These User-Agent strings may be associated with automation, scripting, or security testing tools. A User-Agent can be spoofed and is not proof that a specific tool or attack was actually used.

### 4. Find large HTTP responses (greater than 500 KB)

```spl
source="http_logs.json" host="splunkvm" sourcetype="_json" resp_body_len>500000
| table ts "id.orig_h" "id.resp_h" uri resp_body_len
| sort -resp_body_len
```

**Observed result:** 323 matching events.

The screenshot shows large responses for `/index.html`, with response body lengths near 1.9 million bytes in the first visible rows. The table includes timestamp, source IP, responder IP, URI, and response size.

Large responses can be normal (for example, large pages or downloads) or worth investigating if unexpected. The response size alone does not establish data exfiltration.

## Findings and SOC Interpretation

- The top 10 source IPs account for the highest event counts in the displayed HTTP traffic aggregation.
- The search identified 285 HTTP 5xx server-error events.
- The selected User-Agent filter returned 296 events across four values.
- The large-response search returned 323 events with `resp_body_len` greater than 500,000 bytes.
- These results provide investigation leads, not confirmed attack findings.

## Further Investigation as a SOC Analyst

In a real environment, I would:

- Review individual 5xx status codes (such as 500, 502, 503, and 504) and group them by URI and responder.
- Build a timeline for the selected User-Agent values and check which endpoints they accessed.
- Correlate high-volume source IPs with authentication, firewall, proxy, and endpoint logs.
- Review large responses by destination, URI, content type, and user/session context.
- Check whether large transfers were expected business activity before considering data-exfiltration hypotheses.
- Validate suspicious indicators using approved internal and external intelligence sources.

These are recommended follow-up steps, not detections completed in this lab.

## Evidence

Screenshots are stored in the [`screenshots/`](screenshots/) directory.

1. [Large HTTP responses over 500 KB](screenshots/01-large-http-responses-over-500kb.png)
2. [Selected User-Agent analysis](screenshots/02-suspicious-user-agent-analysis.png)
3. [HTTP 5xx server errors](screenshots/03-http-server-errors-5xx.png)
4. [Top source IPs generating HTTP traffic](screenshots/04-top-source-ips-web-traffic.png)

## Dataset Attribution

The lab instructions reference this sample dataset:

[HTTP logs JSON – 30 Days SOC Challenge Beginner](https://raw.githubusercontent.com/0xrajneesh/30-Days-SOC-Challenge-Beginner/refs/heads/main/http_logs.json)

The dataset source is credited here. This repository contains my own lab notes, SPL searches, interpretation, and screenshots.

## Skills Demonstrated

- Splunk Enterprise fundamentals
- JSON log ingestion and searching
- HTTP status-code analysis
- Web traffic source-IP analysis
- User-Agent investigation
- Large response-size analysis
- SPL: `stats`, `count`, `sort`, `head`, `table`, and field filtering
- Basic SOC triage and evidence documentation

## Disclaimer

This is an educational lab using a sample dataset. The results are not evidence of a confirmed real-world attack, data exfiltration, or compromised system. The IP addresses and HTTP records shown are part of the supplied lab data.
