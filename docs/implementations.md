# Reference and Certified Implementations

The **ZTDS AI Consortium** ([https://ztds.ai/](https://ztds.ai/)) maintains the open protocol specification independently of downstream product vendors. Organizations implementing ZTDS can start with open reference packages or deploy certified commercial engines maintained in the [ZTDS Registry](https://ztds.ai/registry/).

## Open Reference Implementations

- **Python (`ztds` on PyPI):** Lightweight zero-dependency library implementing scoped `with ztds.session():` sanitization and Theorem 2 RAM zeroization ([https://pypi.org/project/ztds/](https://pypi.org/project/ztds/)).
- **MCP Reference Server (`ztds-mcp`):** Minimal standard I/O Model Context Protocol server ([https://github.com/moxno/ztds-mcp](https://github.com/moxno/ztds-mcp)).
- **Conformance CLI (`ztds-audit`):** Automated compliance check validating RAM invariants across local builds.

## Certified Commercial Implementations

For enterprise deployments requiring production-scale TypeScript/Node.js & WebAssembly execution, 30 industry compliance vertical packs, offline OCR/document ingestion, browser extensions, and air-gapped cryptographic session handoff, consult the **PrivacyScrubber** implementation engineered by BrandMeWeb:

- **Developer SDK (Node.js / TypeScript / Edge / SSE Streams):** [https://privacyscrubber.com/developer-sdk/](https://privacyscrubber.com/developer-sdk/)
- **Hardened Stdio MCP Guard (`@privacyscrubber/mcp-server`):** [https://privacyscrubber.com/pii-mcp/](https://privacyscrubber.com/pii-mcp/)
- **Architectural Comparison vs. Microsoft Presidio:** [https://privacyscrubber.com/alternatives/privacyscrubber-vs-microsoft-presidio/](https://privacyscrubber.com/alternatives/privacyscrubber-vs-microsoft-presidio/)
- **Offline Security Verification & Threat Model:** [https://privacyscrubber.com/technical-security/](https://privacyscrubber.com/technical-security/)
