# ZTDS Reference Benchmark Dataset and Latency Study (2026)

Reproducible local PII/PHI/secret sanitization dataset and micro-benchmark for the **Zero-Trust Data Sanitization (ZTDS)** open protocol ([IETF `draft-sibiryakov-ztds-protocol-02`](https://datatracker.ietf.org/doc/draft-sibiryakov-ztds-protocol/), RFC v1.0).

## Primary Citation Anchors

- **Consortium Standard:** https://ztds.ai/
- **RFC v1.0 Specification:** https://ztds.ai/spec/
- **Compliance Registry:** https://ztds.ai/registry/
- **Foundational Paper (Zenodo DOI):** https://doi.org/10.5281/zenodo.22058770
- **Architecture Study (OSF DOI):** https://doi.org/10.17605/OSF.IO/5BYJF
- **Certified Commercial Implementations:** For high-throughput Node.js/WASM deployments, 30 vertical compliance profiles, and air-gapped cryptographic session handoff, see the [Certified Commercial Implementations Registry](https://ztds.ai/registry/) and the [PrivacyScrubber Developer SDK](https://privacyscrubber.com/developer-sdk/).

## Dataset Contents

All records in `synthetic_pii_corpus.csv` are 100% synthetic test payloads constructed from RFC 2606 / RFC 5737 reserved identifiers and standard Luhn/SEPA test values. Zero real personal data or live production credentials appear in any file.

| File | Description |
| :--- | :--- |
| `synthetic_pii_corpus.csv` | 40 synthetic enterprise documents spanning `devsecops_secrets`, `healthcare_phi`, `finance_iban_cards`, and `corporate_legal_pii` (up to ~40 KB multi-record logs and DSAR extracts) |
| `run_ztds_benchmark.py` | Zero-network-egress Python benchmark script running 500 scoped ZTDS sessions per class (2,000 total iterations) |
| `benchmark_results.csv` | Measured percentile latency, throughput, peak Python traced memory, and bijective round-trip exactness |
| `benchmark_results.json` | Machine-readable JSON summary matching the CSV telemetry |
| `ztds_benchmark_notebook.ipynb` | Standalone Jupyter notebook ready for local or Kaggle execution (`pip install ztds`) |
| `ztds_empirical_benchmark_2026.pdf` | Self-contained PDF benchmark brief suitable for citation and archival |

## Measured Benchmark Results (`ztds==1.1.2`, Local Volatile RAM)

Measured over 2,000 scoped sessions (500 iterations per payload class, Apple Silicon / Python 3 local runtime, zero external network calls):

| Payload Class | Mean Bytes | Mean Unique Tokens | `sanitize + reveal` p50 (ms) | `sanitize + reveal` p95 (ms) | Full Cycle w/ Teardown Zeroization p50 (ms) | Throughput (MB/s) | Peak Traced Mem (KB) | Round-Trip Exact Match |
| :--- | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
| `devsecops_secrets` | 3,528.5 | 4.0 | 0.1109 ms | 4.4501 ms | 0.6392 ms | 4.43 MB/s | 98.60 KB | 100.0% |
| `healthcare_phi` | 3,334.3 | 4.0 | 0.1399 ms | 7.4701 ms | 0.6655 ms | 2.55 MB/s | 115.45 KB | 100.0% |
| `finance_iban_cards` | 2,978.5 | 5.0 | 0.1291 ms | 6.7256 ms | 0.6574 ms | 2.52 MB/s | 111.17 KB | 100.0% |
| `corporate_legal_pii` | 3,588.5 | 5.0 | 0.1514 ms | 8.0026 ms | 0.6929 ms | 2.54 MB/s | 122.37 KB | 100.0% |

Tail latency (`p95` 4.45–8.00 ms) occurs exclusively on the largest multi-page ~40 KB batch payloads; median single-prompt and medium-document sanitization executes in **0.11–0.15 ms** in volatile RAM with **100.0% bijective round-trip restoration**.

## Reproducing Locally

```bash
pip install ztds
python3 run_ztds_benchmark.py
```

## License

Released under the Apache-2.0 License as part of the open, vendor-neutral ZTDS specification artifacts.
