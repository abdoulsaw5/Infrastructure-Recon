# Infrastructure

Multi-engine dork generator for infrastructure/banner search engines **Shodan, Censys, ZoomEye, and FOFA**.

A companion to [dork-generator](https://github.com/abdoulsaw5/dork-generator), which targets Google-style web search engines. This one targets a different layer entirely: engines that index open ports, service banners, and TLS certificates rather than crawled web pages. Different data source, different bug classes exposed databases, misconfiguration services, and subdomains discovered through real certificate data instead of DNS wordlists.

**[Live tool →](#)** 

## What it does

Enter a domain or org name and get a curated list of recon queries, each pre-built in the correct native syntax for all four engines and ready to open in a new tab.


| Engine | Syntax style | Example |
|---|---|---|
| Shodan | `field:"value"` | `org:"Target Inc"` |
| Censys | dotted paths with `=`/`:` (Platform syntax) | `host.autonomous_system.name: "Target Inc"` |
| ZoomEye | `field:"value"` | `org:"Target Inc"` |
| FOFA | `field="value"`, query base64-encoded in the URL | `org="Target Inc"` |

## Highlight: certificate-based subdomain discovery

The standout query in the list reads real issued TLS certificate data (`ssl.cert.subject.cn` / `host.services.cert.names` / `cert=`) instead of guessing from a search index. This often surfaces subdomains that DNS-based tools (subfinder, amass, wordlist brute-forcing) miss entirely, since it only requires that a cert was issued not that the subdomain is in a public zone file or a common wordlist.

## Query categories

- Certificate-based subdomain discovery
- Organization-based asset discovery
- Hostname / domain matching
- Exposed admin panels and login pages
- Directory listing exposure
- Exposed databases (MongoDB, Elasticsearch, Redis)
- Exposed CI/CD and dev tooling (Jenkins, Kibana, Grafana, Jupyter)
- Exposed container infrastructure (Docker API, Kubernetes dashboard/API)
- Exposed remote access (RDP, VNC, anonymous FTP)
- Country-scoped organization search

## Confidence tagging

Every query is tagged:

- **● verified** field name and syntax confirmed against each engine's current documentation
- **▲ best-effort** relies on that engine's internal product/app fingerprint naming, which isn't fully stable or publicly documented across all four engines. If a best-effort query returns nothing, check the exact fingerprint string against that engine's own search-bar autocomplete before assuming the target isn't running that service.


## Design

Single self-contained HTML file. Open it in a browser and it works. Same philosophy as the rest of this recon toolkit: simple, dependency-free tools that do one job well.

## Why this approach

Infrastructure search engines see things web crawlers never will, such as misconfigured databases with no web interface, exposed management ports, devices that were never meant to be public. Google dorking finds what got indexed because it has a URL; this class of tool finds what got scanned because it has an open port. Running both against a target covers materially different attack surface.

## Related projects

- [dork-generator](https://github.com/abdoulsaw5/dork-generator) — the Google/Bing/DuckDuckGo/Yahoo/Brave/Startpage counterpart to this tool
- [wp-exposure-map](https://github.com/abdoulsaw5/wp-exposure-map) — static tool cataloging known-sensitive WordPress endpoints

## Disclaimer

For authorized security research and bug bounty hunting only. Always confirm a target is in scope before querying, and follow the program's rules of engagement.
