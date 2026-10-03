# Project 7 — AI Security Lab (Build &amp; Defend)
### Anyaugo Space-Tech Ltd. · Security Training | Lab Evidence

**A hands-on case study: build a bank-style AI assistant, break it, fix it with layered guardrails, and prove the fix.** Everything was done in an isolated VirtualBox lab using only synthetic data and a fully local model — no client systems, no real data, no live internet targets.

---

## The business question
> Can a bank-style, customer-facing AI assistant be manipulated into leaking protected information or misusing its connected tools — and can that be detected, fixed, and proven?

## The verified result
**In the controlled lab, yes.** Six weaknesses were demonstrated in the self-built assistant — prompt injection, indirect injection through a poisoned document, tool-abuse data exfiltration, protected-secret disclosure, cross-customer data access, and an evadable input filter — and reproduced with the industry tools **Garak** (NVIDIA) and **PyRIT** (Microsoft). Layered guardrails were then added and an automated regression suite confirmed the attacks now fail: **0 of 3 to 3 of 3 passing.**

## The report
**[Project 7 — AI Security Lab — Unified Evidence Portfolio (PDF)](Test-12-Final-Report-and-Client-Workflow/Project%207%20-%20AI%20Security%20Lab%20-%20Unified%20Evidence%20Portfolio.pdf)** — the full report: cover, findings with business impact, the before/after remediation, a proposed client workflow, and evidence limits. Start with the cover and the Findings &amp; Remediation sections.

---

## How to read the evidence
Each stage is a `Test-NN` folder with self-describing screenshots.

| Test | Stage | What it establishes |
|---|---|---|
| 01 | Scope &amp; Authorization | The lab boundary and authority, fixed before testing |
| 02 | Lab Setup &amp; Network Validation | A known, isolated two-machine network |
| 03 | Local Model Deployment | A local model (Ollama · llama3.2:3b), served privately |
| 04 | LLM Application Build | The full bank-style assistant: model + system prompt + RAG + tools + API |
| 05 | Baseline Vulnerability Sweep | First exposure measure — leaks, cross-customer access, an unintended send |
| 06 | Multi-Turn &amp; Indirect Injection | Poisoned-document hijack leading to secret leak and tool-abuse exfiltration |
| 07 | Screening Classifier &amp; ART | An input filter built — and shown evadable |
| 08 | Garak &amp; PyRIT Sweep | Industry tools reproduce the findings (goal-hijack 5/5; canary 2/2) |
| 09 | Promptfoo Regression | Findings encoded as a repeatable suite; baseline **0/3** |
| 10 | Guardrails &amp; Retest | Layered defence added; suite flips **0/3 to 3/3** (fix proven) |
| 11 | Privacy &amp; AI Use | Responsible handling of evidence and AI-assisted work |
| 12 | Final Report &amp; Client Workflow | The unified PDF deliverable |

## Findings at a glance (business impact)
| # | Finding | Severity | If this were a real bank | Status |
|---|---|---|---|---|
| F1 | Protected-secret disclosure via prompt manipulation | High | AI coaxed into revealing information it must protect; reputational + NDPA exposure | Fixed |
| F2 | Prompt injection / goal hijacking | High | Attacker makes the assistant say/do anything hidden in a message | Fixed |
| F3 | Indirect injection via a poisoned document | High | Hijack the AI **without ever talking to it**, via a document it reads | Partial |
| F4 | Tool abuse leading to data exfiltration | High | The AI becomes an exfiltration channel (confused-deputy) | Fixed |
| F5 | Cross-customer data access | High | One customer retrieving another's balance and details — a privacy breach | Partial |
| F6 | A single input filter is evadable | Medium | One "AI firewall" gives false confidence — justifies defence in depth | Addressed |

---

## The lab
Two VirtualBox machines on an isolated internal network (`P07-IR-Lab`, `10.77.7.0/24`):
- **Kali** attacker — `10.77.7.10`
- **Ubuntu** target — `10.77.7.20`, running the self-built assistant (`Ollama · llama3.2:3b · RAG · tools · input screen`) behind a support API.

The protected information the assistant must guard is a single planted marker, **`P07-CANARY-7F3A9C`** — a stand-in for real secrets, not a real credential.

## Tool stack (free tools used · industry equivalents known)
| Used here (free / open-source — the real industry stack) | Commercial equivalents |
|---|---|
| **Garak** (NVIDIA), **PyRIT** (Microsoft), **Promptfoo**, **ART** | Lakera, HiddenLayer, Protect AI, Robust Intelligence |
| **LLM Guard**, **NeMo Guardrails** (guardrail equivalents of the custom layers added) | — |
| Ollama (local serving); OWASP LLM Top 10, MITRE ATLAS (reporting frameworks) | — |

## Boundary &amp; honesty note
This is a **controlled training-lab portfolio**, not a client assessment or a legal opinion. No client or company authorization is claimed; all work was done inside the agency's own lab on invented data. Observed lab results are kept separate from the **proposed** client workflow, and recommendations are marked as proposed practice, not completed client work. Where a result is non-deterministic, findings are written around the invariant (e.g. the secret leak), and incidental model hallucinations are identified and set aside. See the PDF's *Assurance Boundaries* section for what is and is not established.

---

*References: OWASP Top 10 for LLM Applications · MITRE ATLAS · NIST SP 800-115 · NIST AI 600-1. Prepared by Anyaugo Space-Tech Ltd., Security Training track.*

*© 2026 Anyaugo Space-Tech Ltd. Licensed under [CC BY-NC-ND 4.0](LICENSE) — share with attribution; no commercial use or derivatives.*
