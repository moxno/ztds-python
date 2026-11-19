# Reproducible Benchmark: Local In-Memory ZTDS vs. Cloud DLP Proxies and Containerized NLP Services

Deploying traditional Data Loss Prevention (DLP) gateways or containerized Python NER stacks (such as Docker-hosted Microsoft Presidio with spaCy models) introduces network serialization overhead, container cold-start weight, and GDPR Article 28 Data Processing Agreement (DPA) expansion.

This page documents empirical, network-free timing and memory measurements for `ztds` executed strictly inside local volatile RAM.

## Empirical Measurements (2,000 Scoped Sessions)

Evaluated across 40 synthetic RFC-reserved enterprise documents (`devsecops_secrets`, `healthcare_phi`, `finance_iban_cards`, `corporate_legal_pii`), scaling from short single alerts (~350 bytes) up to ~40 KB multi-record logs and DSAR extracts, 500 iterations per class:

| Payload Class | Mean Payload Size | `sanitize + reveal` p50 | `sanitize + reveal` p95 (~40 KB) | Full Session w/ RAM Zeroization p50 | Throughput | Peak Traced Memory | Bijective Round-Trip |
| :--- | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
| `devsecops_secrets` | 3,528.5 B | 0.1109 ms | 4.4501 ms | 0.6392 ms | 4.43 MB/s | 98.60 KB | 100.0% |
| `healthcare_phi` | 3,334.3 B | 0.1399 ms | 7.4701 ms | 0.6655 ms | 2.55 MB/s | 115.45 KB | 100.0% |
| `finance_iban_cards` | 2,978.5 B | 0.1291 ms | 6.7256 ms | 0.6574 ms | 2.52 MB/s | 111.17 KB | 100.0% |
| `corporate_legal_pii` | 3,588.5 B | 0.1514 ms | 8.0026 ms | 0.6929 ms | 2.54 MB/s | 122.37 KB | 100.0% |

## Architectural Contrast Against Proxies

- **Remote Cloud DLP Firewalls:** Require client TLS termination, network transit, remote queuing, and re-encryption (measured 184–712 ms round-trip latency in our OSF comparative study, DOI `10.17605/OSF.IO/5BYJF`, disrupting Server-Sent Event streaming).
- **Containerized NLP Microservices:** Require Docker daemon infrastructure, hundreds of megabytes of baseline memory, and an extra internal network hop. See the architectural trade-off matrix in [PrivacyScrubber vs. Microsoft Presidio](https://privacyscrubber.com/alternatives/privacyscrubber-vs-microsoft-presidio/).
- **Local ZTDS In-Memory Sanitization:** Runs in **0.11–0.15 ms median** inline inside the Python/Node process with **~98.6–122.4 KB** peak traced allocation, leaving streaming sockets untouched.

## Academic Citation Anchors

- Protocol Foundation Paper (Zenodo DOI): [https://doi.org/10.5281/zenodo.22058770](https://doi.org/10.5281/zenodo.22058770)
- Comparative Latency Study (OSF DOI): [https://doi.org/10.17605/OSF.IO/5BYJF](https://doi.org/10.17605/OSF.IO/5BYJF)
- IETF Internet-Draft: [https://datatracker.ietf.org/doc/draft-sibiryakov-ztds-protocol/](https://datatracker.ietf.org/doc/draft-sibiryakov-ztds-protocol/)
