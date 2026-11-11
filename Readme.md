# Splunk Network Traffic Analysis | DNS & HTTP SOC Lab

**Author:** Tushar Tyagi  
**Role:** Aspiring SOC Analyst  
**SIEM Tool:** Splunk Enterprise 9.3.0  
**Log Source:** Zeek-style DNS and HTTP logs  
**Query Language:** Splunk Search Processing Language (SPL)

## Project Overview

This repository combines two practical network traffic analysis labs performed using Splunk Enterprise: DNS Log Analysis and HTTP Log Analysis.

The objective is to develop practical SOC investigation skills by analysing network logs, identifying traffic patterns, reviewing suspicious indicators, and extracting useful information through SPL queries.

Both investigations are organised into separate folders with their own documentation, queries, findings, and screenshots.

## Project Objectives

- Practise log ingestion and searching in Splunk Enterprise.
- Analyse DNS queries and identify frequently requested domains.
- Investigate source IP addresses generating DNS and HTTP traffic.
- Examine HTTP server errors (5xx status codes).
- Review User-Agent strings associated with automated tools.
- Identify unusually large HTTP responses.
- Develop basic network monitoring and SOC investigation skills.

## Lab Environment

| Component | Details |
|---|---|
| SIEM | Splunk Enterprise 9.3.0 |
| Virtual Environment | VMware Workstation |
| Log Format | JSON |
| Network Logs | DNS and HTTP |
| Log Framework | Zeek-style logs |
| Query Language | SPL |

## Project Structure

```text
Splunk-Network-Traffic-Analysis/
│
├── README.md
│
├── DNS-Log-Analysis/
│   ├── README.md
│   ├── investigation-notes.md
│   ├── spl-queries.md
│   ├── report.md
│   └── screenshots/
│
└── HTTP-Log-Analysis/
    ├── README.md
    ├── investigation-notes.md
    ├── spl-queries.md
    ├── report.md
    └── screenshots/
```

## Investigation 1: DNS Log Analysis

**Objective:** Analyse DNS activity to understand query patterns, identify active source IP addresses, and find frequently requested domains.

### Activities Performed

- Ingested and analysed 1,200 JSON-formatted DNS events in Splunk.
- Analysed DNS query types: A, AAAA, CNAME, and PTR.
- Identified source IP addresses generating the highest number of DNS events.
- Identified the most frequently queried domains.
- Used SPL commands such as `stats`, `count`, and `sort`.

### Key Findings

- AAAA queries: 321 events.
- A queries: 313 events.
- PTR queries: 309 events.
- CNAME queries: 257 events.
- Highest source IP counts: 113 events each from two IP addresses.
- Most frequently queried names: `google.com` and `printer.local`, with 139 events each.

**Investigation files:** [DNS Log Analysis](DNS-Log-Analysis/README.md)

## Investigation 2: HTTP Log Analysis

**Objective:** Investigate HTTP traffic patterns, server errors, selected User-Agent strings, and large HTTP responses.

### Activities Performed

- Analysed HTTP logs using Splunk Enterprise.
- Identified the top source IP addresses generating HTTP traffic.
- Investigated HTTP server errors using status codes 500–599.
- Reviewed selected User-Agent strings associated with automated or scripted clients.
- Identified HTTP responses exceeding 500,000 bytes.
- Used SPL commands such as `stats`, `count`, `sort`, `head`, and `table`.

### Key Findings

- HTTP 5xx server-error events: 285.
- Events matching selected User-Agent values: 296.
- Large HTTP response events above 500 KB: 323.
- Top source IP: `10.0.0.28`, with 76 events in the displayed results.

**Investigation files:** [HTTP Log Analysis](HTTP-Log-Analysis/README.md)

## Tools and Technologies

- **Splunk Enterprise:** SIEM platform for log ingestion, searching, and analysis.
- **Zeek-style Logs:** Network metadata used for DNS and HTTP investigation.
- **SPL:** Searching, filtering, grouping, counting, and sorting security events.
- **VMware Workstation:** Virtual lab environment.

## Skills Demonstrated

- SIEM fundamentals and log analysis
- DNS traffic investigation
- HTTP traffic investigation
- Source IP analysis
- DNS query analysis
- HTTP status-code investigation
- User-Agent analysis
- Large response-size investigation
- SPL query writing
- Basic SOC investigation and reporting

## SOC Analyst Learning Outcomes

Through these labs, I practised how a SOC analyst can use network logs to understand normal traffic patterns, identify unusual activity, investigate potential indicators, and document findings for further analysis.

The findings are investigation leads and do not independently establish malicious activity or a confirmed security incident.

## Disclaimer

This repository is intended for educational and cybersecurity portfolio purposes. It uses sample lab datasets and does not represent a production security environment or a confirmed real-world security incident.
