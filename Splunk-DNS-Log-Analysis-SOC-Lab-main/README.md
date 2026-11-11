# Splunk DNS Log Analysis | SOC Lab

**Category:** Security Operations / SIEM / Network Log Analysis  
**Tool:** Splunk Enterprise 9.3.0  
**Data:** JSON-formatted Zeek DNS logs  
**Events analysed:** 1,200  
**Author:** Tushar Tyagi

## Project Overview

This project demonstrates a basic SOC investigation workflow using DNS logs in Splunk. I ingested a JSON-formatted DNS dataset, searched the events with Splunk Search Processing Language (SPL), and summarised DNS activity by query type, source IP address, and requested domain.

The goal was to practise extracting useful network activity from logs and turning raw events into simple investigation findings.

## Objectives

- Ingest and search DNS logs in Splunk.
- Understand common DNS query types: A, AAAA, CNAME, and PTR.
- Identify source IP addresses generating the most DNS queries.
- Identify the most frequently requested domains.
- Practise basic SPL commands such as `stats` and `sort`.

## Lab Environment

| Component | Details |
|---|---|
| SIEM | Splunk Enterprise 9.3.0 |
| Log format | JSON |
| Dataset | Zeek DNS logs |
| Events returned | 1,200 |
| Search host filter | `splunkvm` |
| Source filter | `dns_logs.json` |
| Sourcetype used in screenshots | `_json` |

The original lab instructions suggest using an index such as `dns_lab` and a JSON sourcetype. In my screenshots, I searched the uploaded data using `source="dns_logs.json"`, `host="splunkvm"`, and `sourcetype="_json"`. These are different ways of identifying the same lab data; use the filters that match your own Splunk ingestion settings.

## Investigation Workflow

1. Uploaded the JSON DNS log file through **Splunk Web → Settings → Add Data → Upload**.
2. Selected JSON event parsing and completed ingestion.
3. Confirmed that Splunk returned 1,200 events.
4. Ran SPL searches to group DNS events by query type, source IP, and queried domain.
5. Reviewed the statistics and saved screenshots as evidence.

## SPL Queries

The following queries match the searches visible in the screenshots.

### 1. DNS query type breakdown

```spl
source="dns_logs.json" host="splunkvm" sourcetype="_json"
| stats count by qtype
```

**Observed results:**

| Query type | Events |
|---|---:|
| A | 313 |
| AAAA | 321 |
| CNAME | 257 |
| PTR | 309 |
| **Total** | **1,200** |

### 2. Most active source IP addresses

```spl
source="dns_logs.json" host="splunkvm" sourcetype="_json"
| stats count by "id.orig_h"
| sort -count
```

**Top results visible in the screenshot:**

| Source IP | DNS events |
|---|---:|
| 192.168.1.10 | 113 |
| 192.168.1.18 | 113 |
| 192.168.1.21 | 112 |
| 192.168.1.20 | 105 |
| 192.168.1.12 | 102 |
| 192.168.1.13 | 99 |
| 192.168.1.16 | 97 |
| 192.168.1.17 | 96 |
| 192.168.1.19 | 95 |
| 192.168.1.11 | 92 |

### 3. Most frequently queried domains

```spl
source="dns_logs.json" host="splunkvm" sourcetype="_json"
| stats count by query
| sort -count
```

**Top results visible in the screenshot:**

| Queried domain | DNS events |
|---|---:|
| google.com | 139 |
| printer.local | 139 |
| example.com | 137 |
| yahoo.com | 125 |
| fileserver.local | 120 |
| router.local | 116 |
| internal.lan | 114 |
| backup.local | 105 |
| ipv6test.local | 103 |
| microsoft.com | 102 |

## Findings and SOC Interpretation

- The dataset contains 1,200 DNS events distributed across four query types. AAAA queries are the largest group (321), followed by A (313), PTR (309), and CNAME (257).
- The two highest source-IP counts shown are 113 events each, from `192.168.1.10` and `192.168.1.18`.
- `google.com` and `printer.local` are tied as the most frequently queried names, with 139 events each.
- The results provide a baseline view of DNS activity in this sample. A high query count alone does not establish malicious behaviour.

## What I Would Investigate Next as a SOC Analyst

If this were a live environment, I would:
- Review the time distribution of DNS requests from the busiest hosts.
- Check for unusually frequent queries or possible DNS tunnelling indicators.
- Investigate rare, newly observed, or suspicious domains.
- Correlate DNS activity with endpoint, firewall, proxy, and authentication logs.
- Validate suspicious domains using approved threat-intelligence sources.

These are recommended next steps, not detections completed in this lab.

## Evidence

Screenshots are stored in the [`screenshots/`](screenshots/) directory.

1. [DNS query type breakdown](screenshots/01-dns-query-type-breakdown.png)
2. [Top source IP addresses](screenshots/02-top-source-ip-addresses.png)
3. [Most queried domains](screenshots/03-most-queried-domains.png)

## Dataset Attribution

The lab instructions reference this sample dataset:

[DNS logs JSON – 30 Days SOC Challenge Beginner](https://raw.githubusercontent.com/0xrajneesh/30-Days-SOC-Challenge-Beginner/refs/heads/main/dns_logs.json)

Dataset source is credited here. This repository contains my own lab notes, SPL queries, interpretation, and screenshots.

## Skills Demonstrated

- Splunk Enterprise fundamentals
- Log ingestion and event searching
- JSON and Zeek DNS log familiarity
- SPL: `stats`, `count`, `by`, and `sort`
- DNS query analysis
- Basic SOC investigation and documentation

## Disclaimer

This is an educational lab using a sample dataset. The results are not evidence of a real security incident or confirmed malicious activity. Internal IP addresses and domains shown are part of the lab data.
