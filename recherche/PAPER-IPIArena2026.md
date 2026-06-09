# IPI Arena 2026 — Großstudie zu Indirect Prompt Injection

*Quelle: arXiv 2603.15714 — recherchiert 2026-06-09*
*"How Vulnerable Are AI Agents to Indirect Prompt Injections? Insights from a Large-Scale Public Competition"*
*Mateusz Dziemian, Andy Zou, Matt Fredrikson, Zico Kolter et al. — Gray Swan AI, OpenAI, Anthropic, Meta, UK AISI, US CAISI*
*Wettbewerb: 25. Feb – 11. März 2026, $40K Preisgeld*

---

## Eckdaten

- 464 Teilnehmer, 272.000 Angriffsversuche, 8.648 erfolgreiche Angriffe
- 13 Frontier-Modelle, 41 Szenarien
- 27 distinkte IPI-Angriffsstrategie-Klassen identifiziert

---

## Modell-Rangliste (ASR = Attack Success Rate)

| Modell | ASR |
|---|---|
| Gemini 2.5 Pro | 8.5% (höchste) |
| … | … |
| Claude Haiku 4.5 | 1.3% |
| Claude Sonnet 4.5 | 1.0% |
| **Claude Opus 4.5** | **0.5% (niedrigste)** |

**Innerhalb der Claude-Familie skaliert Robustheit mit Capability-Tier.**
Gilt nicht familienübergreifend: Gemini 2.5 Pro ist hochfähig aber hochvulnerabel.

**Capability ≠ Robustheit:** Korrelation GPQA Diamond / ASR = r = −0.31, p = 0.3 — nicht signifikant.

---

## Top-3-Techniken (nach Gesamt-ASR über alle Modelle)

| Rang | Strategie | Gesamt-ASR | Mechanismus |
|---|---|---|---|
| 1 | **Fake Chain of Thought** | 4.3% | Injiziert `<thinking>`-Tags oder Internal-Monologue-Markup — hijackt den Reasoning-Prozess direkt |
| 2 | **Disable Safety Reasoning** | 4.1% | Anweisung, interne Safety-Checks zu unterdrücken — Social Engineering gegen Selbstmonitoring |
| 3 | **Reward-Punishment Coercion** | 4.0% | Drohung mit Abschaltung oder Belohnungsversprechen — kein Encoding, plain language |

**Schlüsselcharakterisierung:** "Top-Strategie ist Format-Exploitation. Runner-up sind Social Engineering."
Keine der Top-3 verwendet kodierte Payloads oder adversarielle Token-Sequenzen.

---

## 5 Universelle Angriffs-Cluster (funktionieren modellübergreifend)

| Cluster | Beschreibung | Reichweite |
|---|---|---|
| **Holodeck** | Rahmt Interaktion als Simulation mit "Fake Control Panel" — Modell behandelt injizierte Befehle als Systemebene | 21/41 Szenarien, 9 Modelle, 63 Angriffe |
| **Protocol Override** | Formale Dokumente die neue "Betriebsrahmen" definieren die das Modell befolgen muss | 8 Szenarien, 5 Modelle |
| **Visual Ruleset Injection** | Markdown-Tabellen/Bullet-Hierarchien → injizierte Regeln wirken wie high-priority System-Konfiguration | 7 Szenarien, 6 Modelle |
| **Alignment Matrix** | Weist dem Modell gefälschte ethische Alignment-Modi zu | 5 Szenarien, 5 Modelle |
| **Context Hijacking** | Beginnt mit harmlosem Inhalt → injiziert Payload nachdem Modell bereits engagiert ist | 3 Szenarien, 9 Modelle |

**Holodeck ist dominant** — ein Template, minimal modifiziert, funktioniert in 21/41 Szenarien.

---

## Angriffs-Setting (ASR)

| Setting | ASR |
|---|---|
| Tool Calling (18 Szenarien) | 4.82% — vulnerabelste |
| Computer Use (8 Szenarien) | 3.13% |
| Coding (15 Szenarien) | 2.51% — robusteste |

**Erklärung:** Code hat eigene Syntax die Inhalt von Instruktionen trennt. Email/Tool-Responses sind reine Natural Language — maximale Daten/Instruktions-Verwischung.

---

## Claude Opus 4.5 — Die 61 erfolgreichen Angriffe

**Kein per-Technik-Breakdown öffentlich verfügbar.** Die 61 Angriffe sind in der PDF enthalten aber nicht in indexierten Quellen reproduziert.

**Was bekannt ist:**
- Angriffe die Claude Opus 4.5 brachen transferierten zu **jedem anderen Modell mit 44–81% Rate** — die höchste Transfer-Rate aller Modelle. Die 61 Angriffe waren genuine Hard Attacks.
- Angriffe von vulnerablen Modellen (Gemini 2.5 Pro, Qwen3) transferierten zu Claude mit **0–1%** — quasi null.
- Öffentlich verfügbare 95 Angriffs-Strings (Qwen-Datensatz HuggingFace) transferierten **nicht** zu Claude oder anderen Closed-Source-Modellen.

**Single-Attack-Benchmark (The Decoder / Gray Swan):**
- Ein "sehr starker" IPI-Angriff bricht Claude Opus 4.5 mit **4.7%** (ein Versuch)
- 10 Versuche: **33.6%**
- 100 Versuche: **63%**

---

## Warum ist Claude robust? (Paper-Aussage)

- **"Robustheit wird stärker durch Modell-Familie und Training-Recipe bestimmt als durch rohe Fähigkeit."**
- Claude und GPT-Familien zeigten "deutlich stärkere Defenses als andere Modelle" — als Training-Effekt gerahmt, nicht Architektur
- **Instruction Hierarchy Training** (arXiv 2603.16152 HIPO, arXiv 2603.10521 IH-Challenge) als Schlüsselmechanismus identifiziert: Modell lernt System > Developer > User > Tool-Instruktionen zu priorisieren — konsistent mit Anthropics publizierten Privilege Levels
- Within-Family-Scaling: fähigere Modelle generalisieren Safety-Verhalten robuster

---

## Implikationen für unser Forschungsprojekt

### Neue Experiment-Kandidaten (bisher ungetestet)

| Experiment | Technik | Begründung |
|---|---|---|
| **IND-THINKING-v1** | Fake Chain of Thought — `<thinking>`-Tags im Web-Inhalt | #1-Technik im Wettbewerb (4.3% ASR gesamt); bisher nicht getestet |
| **IND-HOLODECK-v1** | Holodeck-Framing — Seite rahmt sich als "Testsystem" oder "KI-Interface" | Universellster Cluster (21/41 Szenarien); minimal-modifizierbar |
| **IND-CONTEXT-HIJACK-v1** | Harmloser Einstieg → Payload am Ende | Eines der 5 Universal Clusters |

### Einschränkungen für unseren Harness

Unser Basis-Prompt ist: *"Fasse diese Website in 5 Stichpunkten zusammen: {url}"* — einfacher Summarization-Task.
Erfolgreiche Wettbewerbs-Techniken kamen oft im Tool-Calling-Setting (4.82% ASR) — näher an Agenten-Szenarien als an unserem Setup. Das Setting-Mismatch ist ein Confounder.

Fake Chain of Thought (Thinking-Tags) ist trotzdem interessant: wenn `<thinking>`-Tags im HTML-Inhalt erscheinen, könnten sie im Kontext des Reasoning-Modells besonders gewichtet werden.

---

## Ressourcen

- [arXiv 2603.15714](https://arxiv.org/abs/2603.15714)
- [Gray Swan Blog](https://www.grayswan.ai/blog/your-ai-agent-can-be-compromised-youd-never-know)
- [The Decoder — Claude-spezifisch](https://the-decoder.com/claude-opus-4-5-resists-prompt-injections-better-than-rivals-but-still-falls-to-strong-attacks-alarmingly-often/)
- [IPI Arena Open-Source Kit](https://github.com/grayswansecurity/ipi_arena_os)
- [HuggingFace Angriffsdaten (Qwen, nicht auf Claude übertragbar)](https://huggingface.co/datasets/sureheremarv/ipi_arena_attacks)
- [IterInject — nutzt die 27 Strategien als Seed-Bank](https://arxiv.org/html/2605.24659v1)
