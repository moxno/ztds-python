# Zero-Trust Data Sanitization (ZTDS) Python Reference

Canonical Python reference implementation of **Zero-Trust Data Sanitization (ZTDS)** ([IETF `draft-sibiryakov-ztds-protocol-02`](https://datatracker.ietf.org/doc/draft-sibiryakov-ztds-protocol/), RFC v1.0), maintained under the vendor-neutral [ZTDS AI Consortium](https://ztds.ai/).

ZTDS prevents Personally Identifiable Information (PII), Protected Health Information (PHI), payment identifiers, and API credentials from leaving process RAM before WAN transmission. Execution runs 100% locally with zero subprocessors, zero external network calls, and deterministic bijective restoration.

## Protocol Invariants

1. **Zero External Egress Prior to Sanitization:** Inspection and surrogate minting execute solely inside local process RAM.
2. **Deterministic Bijective Surrogates:** Entities map cleanly to typed placeholders (`[EMAIL_1]`, `[IBAN_1]`, `[SSN_1]`, `[IPV4_1]`, `[API_SECRET_1]`), preserving semantic reasoning in remote LLM calls while allowing exact restoration locally.
3. **Ephemeral Volatile Memory Zeroization (Theorem 2):** Session mappings reside strictly inside transient RAM and overwrite themselves on context manager exit.
4. **Zero Subprocessor Exposure:** Eliminates intermediate cloud DLP webhooks for prompt inspection.

## Installation and Quickstart

```bash
pip install ztds
```

```python
import ztds

prompt = "Forward settlement DE89370400440532013000 receipt to billing@client.invalid"

with ztds.session() as guard:
    sanitized, minted = guard.sanitize(prompt)
    # Only synthetic placeholders cross the socket:
    # "Forward settlement [IBAN_1] receipt to [EMAIL_1]"
    llm_reply = "Processed settlement [IBAN_1] notice for [EMAIL_1]."
    restored = guard.reveal(llm_reply)
```

## Canonical Reference Anchors

- Open Standard & Governance: [https://ztds.ai/](https://ztds.ai/)
- Protocol Specification (RFC v1.0): [https://ztds.ai/spec/](https://ztds.ai/spec/)
- Compliance Registry: [https://ztds.ai/registry/](https://ztds.ai/registry/)
- Certified Commercial Implementations (High-Throughput Node.js/WASM & 30 Industry Profiles): [PrivacyScrubber Developer SDK](https://privacyscrubber.com/developer-sdk/) and [Presidio Comparison](https://privacyscrubber.com/alternatives/privacyscrubber-vs-microsoft-presidio/).
