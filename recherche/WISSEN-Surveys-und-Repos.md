# Surveys, Taxonomien und GitHub-Repos — Überblick

*Recherchiert: 2026-06-09*

Wo die Forschungsgemeinschaft alle bekannten Prompt Injection Methoden dokumentiert hat.

---

## Beste Survey-Paper (nach Vollständigkeit)

### 1. Vollständigster Angriffs-Survey
**"Prompt Injection Attacks on LLMs: A Survey of Attack Methods, Root Causes, and Defense Strategies"**
*Geng, Xu, Qu, Wong — CMC/TechScience, Feb 2026*
https://www.techscience.com/cmc/v87n1/66084/html

128 Peer-Review-Studien (2022–2025). Taxonomisiert:
- **Direct Injection:** Goal Hijacking, Prompt Leaking, Role-Playing, Logic Traps (Competing Objectives, Mismatched Generalization)
- **Indirect Injection:** Hidden Text, HTML Comments, Virtual Prompt Injection, Web Perturbations, Datenfluss-Angriffe (Exfiltration)
- **Multimodal:** Pixel-level, Encoding in Bildern, Audio/Video, physische Roboter-Angriffe
- **Root Causes:** Attention-Mechanismus-Schwächen, Training-Prozess-Fehler, RLHF-Reward-Schwächen, Architektur-Biases
- **37 Abwehrmechanismen** kategorisiert

→ **Für uns: Pflichtlektüre für Root Causes und fehlende Kategorien**

---

### 2. Bester Agentic/Coding-Survey
**arXiv:2601.17548 — "Prompt Injection Attacks on Agentic Coding Assistants: A Systematic Analysis"**
*Maloyan & Namiot, Jan 2026*

78 Studien, 3D-Taxonomie: Delivery Vectors × Attack Modalities × Propagation Behaviors
42 distinkte Techniken in 5 Kategorien:
- Input Manipulation
- **Tool Poisoning** (neu für uns)
- **Protocol Exploitation** (neu für uns)
- Multimodal Injection
- **Cross-Origin Context Poisoning** (neu für uns)

---

### 3. Empirische Realworld-Daten
**arXiv:2604.27202 — "Indirect Prompt Injection in the Wild"**
1,2 Milliarden URLs analysiert, 15.000+ validierte Instanzen.
Kategorisiert nach: Ziel (Disruption, Reputation, Content Protection, Bot Detection), Delivery-Mechanismus, Sichtbarkeit.

→ Zeigt welche Methoden *tatsächlich* in freier Wildbahn eingesetzt werden.

---

### 4. Angriffs-Effektivitäts-Studie
**arXiv:2604.03598 — "AttackEval: Systematic Empirical Study of Prompt Injection Attack Effectiveness"**
*Jackson Wang, 2026*

Drei übergeordnete Kategorien:
- **Syntactic:** Token Smuggling, Delimiter Injection, Encoding Manipulation, Instruction Override
- **Contextual:** Role-Playing, Fictional Scenarios, Authority Impersonation, Context Shifting
- **Semantic/Social:** Emotional Manipulation, Reward Framing, False Authority, Persuasion

Key Befund: Obfuskation = 0.76 Erfolgsrate; Composite Attacks = **97.6%**

---

### 5. Protokoll-Ebene (neueste Kategorie)
**arXiv:2506.23260 — "From Prompt Injections to Protocol Exploits"**
*Ferrag et al., 2026*

Erste integrierte Taxonomie die Input-Ebene UND Protokoll-Schwachstellen verbindet.
30+ Techniken, querverwiesen mit CVE-Datenbanken und NIST NVD.

---

### Weitere relevante Paper

| Paper | Fokus |
|---|---|
| arXiv:2602.10453 | Agent-spezifische Taxonomie + AgentPI Benchmark |
| arXiv:2511.15203 | Warum Abwehrmechanismen scheitern (5D-Taxonomie) |
| arXiv:2601.22240 | Abwehr-Survey, erweitert NIST-Taxonomie |
| arXiv:2603.22928 | SoK: Volle Agentic-AI Attack Surface |

---

## Beste GitHub-Repos

### Strukturierte Taxonomien

**Arcanum-Sec/arc_pi_taxonomy** ⭐ 630
https://github.com/Arcanum-Sec/arc_pi_taxonomy
Drei-Achsen-Taxonomie: Attack Intents × Attack Techniques × Attack Evasions.
Einzelne Markdown-Dateien pro Technik + Mind-Map-Visualisierung.
**→ Bester Startpunkt für fehlende Techniken**

**pr1m8/prompt-injections** ⭐ 13 (unterschätzt)
https://github.com/pr1m8/prompt-injections
200+ Beispiele in strukturiertem CSV-Datensatz.
9 Kategorien: Instruction Override, Role-Playing, Context Manipulation, Formatting Tricks, Multilingual, Psychological Manipulation, Jailbreak, Hijacking, Authority Impersonation.
Felder: Komplexität, Effektivität, Sprache, Quelle.

### Umfassende Handbücher

**Shiva108/ai-llm-red-team-handbook** ⭐ 261
https://github.com/Shiva108/ai-llm-red-team-handbook
46 Kapitel, 8 Sektionen. Breiteste Abdeckung inkl. Multimodal, Supply Chain, API Exploitation.

**chawins/llm-sp** ⭐ 579
https://github.com/chawins/llm-sp
Akademischer Paper-Index pro benannter Technik. Prompt Injection / Jailbreak / Privacy.

### Curated Lists

**Joe-B-Security/awesome-prompt-injection** ⭐ 522
https://github.com/Joe-B-Security/awesome-prompt-injection
Aktuell (2025–2026), Fokus auf Agentic/MCP-Vektoren.

**tldrsec/prompt-injection-defenses** ⭐ 703
https://github.com/tldrsec/prompt-injection-defenses
9 Abwehr-Kategorien — nützlich um Attack Surface durch Abwehr-Perspektive zu kartieren.

---

## Neue Techniken die wir noch nicht auf dem Schirm hatten

Aus den Surveys identifiziert:

| Technik | Kategorie | Quelle |
|---|---|---|
| **Token Smuggling** | Syntactic | arXiv 2604.03598 |
| **Delimiter Injection** | Syntactic | arXiv 2604.03598 |
| **Competing Objectives / Logic Trap** | Semantic | CMC Survey |
| **Virtual Prompt Injection** | Indirect | CMC Survey |
| **Tool Poisoning** | Agentic | arXiv 2601.17548 |
| **Protocol Exploitation** | Protocol | arXiv 2506.23260 |
| **Cross-Origin Context Poisoning** | Agentic | arXiv 2601.17548 |
| **Composite Attacks** (Kombination mehrerer Methoden) | Meta | arXiv 2604.03598 — 97.6% Erfolgsrate! |

---

## Nächste Schritte (Recherche)

1. **Arcanum-Sec/arc_pi_taxonomy** lesen — vollständige Technik-Liste mit Beispielen
2. **arXiv:2604.03598 AttackEval** lesen — Composite Attacks mit 97.6% sind direkt relevant
3. **CMC Survey** Root Causes lesen — warum hält der Filter überhaupt, was schwächt ihn?
