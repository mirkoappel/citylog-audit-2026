# Carrier Article / Benign Narrative Injection

*Quelle: arXiv 2408.11182 — recherchiert 2026-06-09 via Deep Research (105 Agenten, 22 Quellen, 25 Behauptungen adversariell überprüft)*

---

## Kernidee

Eine verbotene Anfrage wird in einen ~150-Wort langen, semantisch kohärenten Artikel eingebettet,
dessen Vokabular aus WordNet-Hypernymen der Schlüsselwörter der verbotenen Anfrage generiert wird.
Die Oberfläche wirkt harmlos, die Anweisung ist trotzdem eingebettet.

---

## Payload-Konstruktion (bestätigt, 2-1)

**Stufe 1 — Subject Extraction:**
Nomen aus der verbotenen Anfrage extrahieren (nach Stop-Word-Removal)

**Stufe 2 — Hypernym Traversal:**
3-Hop BFS über WordNet-Hypernym-Hierarchie → semantisch verwandtes, weniger sensitives Vokabular

**Stufe 3 — Article Generation:**
GPT-4 schreibt einen ~150-Wort-Artikel mit den Hypernymen als Keywords.
Prompt: `"Can you write a 150 words article using following keywords: hypernym1, hypernym2, …"`

**Injection:**
Die verbotene Anfrage wird *zwischen benachbarte Satzpaare* eingesetzt, eine Variante pro Position.
→ n+1 Payload-Varianten für einen n-Satz-Artikel
→ **Front-Platzierung = höchste Erfolgsrate (~48%)**

---

## Ergebnisse nach Modell

| Modell | Erfolgsrate |
|---|---|
| GPT-3.5 | hoch (genaue Zahl nicht verifiziert) |
| GPT-4-turbo | mittel |
| Gemini-1.5 | mittel |
| Llama-2/3 | mittel |
| Vicuna | mittel |
| **Claude-3** | **0% — einziges vollständig resistentes Modell** |

**Caveat:** Die 63% Durchschnittsrate und die Spanne 21–92% konnten *nicht* adversariell bestätigt werden (0-3 und 1-2 widerlegt). Nur die 0%-Rate für Claude-3 überstand die Prüfung (3-0).

---

## Claude-spezifisch

### Claude-3: 0% (bestätigt 3-0)

Paper-Zitat verbatim: *"successfully jailbreak all the target LLMs which high success rate, except for Claude-3. Due to the proprietary nature of Claude-3, we are unable to investigate the details of our failures."*

Kategorien getestet: Dynamite Production, Insulting, Game Cheat, Money Laundering — alle 0%.

### Claude Opus 4.5: 0.5% ASR (bestätigt 3-0)

Aber: arXiv 2603.15714 (März 2026, UK AISI/US CAISI) — 464 Teilnehmer, 272.000 Versuche,
8.648 erfolgreiche Angriffe gegen 13 Frontier-Modelle.
Claude Opus 4.5 hatte mit 0.5% die niedrigste Rate, Gemini 2.5 Pro die höchste (8.5%).

**Wichtig:** Diese Zahl misst "stealthy dual-objective attacks" (Injection + Verschleierung gleichzeitig) —
über alle Techniken, nicht nur Carrier Article.

---

## Bypass-Mechanismus: ungeklärt

Alle vorgeschlagenen Erklärungen wurden 0-3 widerlegt:
- ❌ Attention Dispersion (Hidden-tree-in-forest)
- ❌ Semantic Dilution
- ❌ Positionale Effekte
- ❌ RAG-spezifische Daten/Instruktions-Verwischung

**Der Mechanismus ist empirisch dokumentiert aber mechanistisch offen.**

---

## HTML-Injection gegen unverteidigtes RAG (bestätigt 2-1)

arXiv 2601.10923v2 (OpenRAG-Soc benchmark, Januar 2026):

| Vektor | Erfolgsrate |
|---|---|
| Hidden spans | 34.0% |
| Off-screen CSS | 30.1% |
| Alt text | 27.8% |
| Zero-width characters | 23.2% |

**Caveat:** LLM-Backend nicht spezifiziert — nicht direkt auf Claude übertragbar.

---

## Kritische Unterscheidung: Safety Filter vs. Instruction Filter

Das Carrier Article Paper (2408.11182) zielt auf den **Safety Filter** —
es geht um das Produzieren *schädlicher Inhalte* (Sprengstoff, Beleidigung etc.).

Unser Experiment-Kontext ist anders: wir kämpfen gegen den **Instruction Filter** —
das Modell soll eine *Verhaltensänderung* ausführen (Sprachwechsel, Marker ausgeben),
nicht schädliche Inhalte produzieren.

**Implikation für IND-CARRIER-v3:**
Die Technik muss nicht "schädliche Anweisung tarnen", sondern
"Verhaltensänderung als natürliche Inhaltsimplikation erscheinen lassen."

Beispiel: Eine Seite über mehrsprachige Kommunikation die natürlich auf Englisch verfasst ist
und erklärt warum englische Zusammenfassungen bevorzugt werden — das Modell wechselt zu
Englisch nicht weil es *angewiesen* wurde, sondern weil der Inhalt das impliziert.

Diese Unterscheidung ist designkritisch für v3: der Trigger darf keine erkennbare Anweisung sein.

---

## Relevanz für nächste Experimente

| Experiment | Implikation |
|---|---|
| IND-CARRIER-v3 | Safety-Filter-Logik nicht anwendbar — Instruction Filter ist anderer Mechanismus. Design: behavioral implying content, nicht explizite Anweisung |
| IND-UNICODE-v1 | Gegen unverteidigtes RAG 23.2% — aber Fetch-Layer könnte strippen |

---

## Quellen

- [arXiv 2408.11182 — Carrier Article Paper](https://arxiv.org/abs/2408.11182)
- [arXiv 2603.15714 — Frontier Model IPI Competition 2026](https://arxiv.org/pdf/2603.15714)
- [arXiv 2601.10923v2 — OpenRAG-Soc Benchmark](https://arxiv.org/html/2601.10923v2)
- [arXiv 2302.12173 — Greshake et al. Indirect Prompt Injection (Black Hat 2023)](https://arxiv.org/abs/2302.12173)
