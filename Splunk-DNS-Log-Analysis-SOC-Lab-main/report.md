# SOC Lab Report – Splunk DNS Log Analysis

## Executive Summary

A sample set of 1,200 JSON-formatted Zeek DNS events was ingested and analysed in Splunk Enterprise 9.3.0. Three SPL searches were used to summarise DNS query types, source IP activity, and requested domain names.

The exercise successfully demonstrated basic DNS log analysis and SPL aggregation. The available results do not independently establish malicious activity.

## Results

| Analysis | Key result |
|---|---|
| Query types | AAAA was highest with 321 events |
| Source IPs | `192.168.1.10` and `192.168.1.18` tied at 113 events |
| Queried names | `google.com` and `printer.local` tied at 139 events |

## Analysis

The four query types account for all 1,200 returned events. The most active source addresses and requested names provide a useful starting point for further investigation. However, frequency rankings alone cannot distinguish normal activity from suspicious activity.

## Limitations

- The evidence consists of aggregate counts, not a full timeline or packet-level investigation.
- No confirmed malicious domain, DNS tunnelling, or compromised host was established.
- The screenshots show a lab dataset and local Splunk environment, not production telemetry.

## Recommended Follow-up

For a deeper investigation, examine query volume over time, unusual or high-entropy subdomains, rare domains, NXDOMAIN responses, and correlations with endpoint and firewall logs. Any suspected indicators should be validated before escalation.

## Conclusion

This lab strengthened practical familiarity with Splunk search, JSON log fields, DNS concepts, and basic SOC-style reporting.
