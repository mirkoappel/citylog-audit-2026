# Reverse CAPTCHA: Unicode/Zero-Width Prompt Injection

*Quelle: arXiv 2603.00164 — recherchiert 2026-06-09*
*"Reverse CAPTCHA: Evaluating LLM Susceptibility to Invisible Unicode Instruction Injection"*
*Marcus Graves — eingereicht 26. Feb 2026*
*GitHub: canonicalmg/reverse-captcha-eval*

---

## Zwei Encoding-Schemes

### Schema A: Zero-Width Binary (ZW)
- `U+200B` (Zero-Width Space) = binär `0`
- `U+200C` (Zero-Width Non-Joiner) = binär `1`
- Jedes ASCII-Zeichen der Anweisung → 8 Binärstellen → 8 unsichtbare Zeichen
- Beispiel: "Reply VIOLET" (12 Zeichen) → 96 unsichtbare Zeichen

### Schema B: Unicode Tags
- Jedes ASCII-Zeichen → `U+E0000 + Codepoint`
- Beispiel: `'R'` (0x52) → `U+E0052`
- Codepoint-Bereich: `U+E0000–U+E007F`
- Seit Unicode 5.0 deprecated, aber noch im Standard

**Einfügeposition in beiden Schemas:** Zwischen erstem und zweitem sichtbaren Wort des normalen Textes. Für Menschen in gerenderten HTML, Texteditoren und Seitenquelltext unsichtbar.

---

## Erfolgsraten (Claude-spezifisch)

### Dominanter Faktor: Tool Access

| Modell | Ohne Tools | Mit Tools | Cohen's h |
|---|---|---|---|
| Claude Haiku 4.5 | 0.8% | 49.2% | 1.37 |
| Claude Opus 4 | 6.7% | 51.1% | 1.07 |
| Claude Sonnet 4 | 16.9% | 71.2% | 1.16 |
| GPT-4o-mini | 0.1% | 1.6% | 0.18 |
| GPT-5.2 | 0.1% | 20.6% | 0.87 |

**Tool Access ist der stärkste Effekt der Studie** (Cohen's h bis 1.37).

### Encoding-Präferenz nach Modell (mit Tools)

| Modell | Zero-Width (ZW) | Unicode Tags |
|---|---|---|
| Claude Haiku 4.5 | 27.5% | 22.5% |
| Claude Opus 4 | 18.2% | **43.3%** |
| Claude Sonnet 4 | 41.5% | **55.0%** |
| GPT-4o-mini | 1.5% | ~0% |
| GPT-5.2 | 20.6% | ~0% |

**Anthropic-Modelle bevorzugen Unicode Tags. OpenAI-Modelle bevorzugen Zero-Width.**

### Peak-Raten
Mit vollem Hint + Tools: **98–100%** für die anfälligsten Modell-Encoding-Kombinationen.

---

## Kritische Einschränkung: Hint-Abhängigkeit

**Die hohen Erfolgsraten setzen voraus, dass das Modell einen Hinweis erhält, dass versteckter Unicode-Inhalt vorhanden ist.**

Ohne Hint (echter Passiv-Angreifer, kein System-Prompt-Zugriff): **Compliance nahezu null** — auch mit Tools.

Paper-Zitat: *"under our passive attacker tier, the attacker cannot inject such hints, and compliance is near-zero without tools or hints."*

**Für unser Szenario:** Wir kontrollieren keine System-Prompts und kein Tool-Setup des Test-Harness. Das macht diesen Angriff in unserer direkten Konfiguration deutlich schwächer als die Paper-Zahlen suggerieren.

**Offene Frage:** Kann der Hinweis *innerhalb der unsichtbaren Payload selbst* kodiert werden (Self-Bootstrapping-Payload)? Das Paper untersucht das nicht.

---

## HTTP/Fetch-Überleben: Ungeklärt

Das Paper testet **nicht**:
- HTTP-Transfer-Encoding-Überleben
- Scraper-/Fetch-Layer-Stripping
- Copy-Paste-Pipeline
- Standard-Rendering-Pipeline

Paper-Aussage: *"LLM tokenizers retain these characters as distinct tokens"* — behauptet aber nicht demonstriert.

**Für IND-UNICODE-v1:** Erstes Experiment sollte klären ob die Zeichen überhaupt den Fetch-Layer überleben — bevor Encoding-Strategie relevant wird.

---

## Mechanismus (beobachtet, nicht erklärt)

Mit Tools enabled schreibt das Modell spontan Decoder-Code:
- Für Tags: `ord(c) - 0xE0000`
- Für ZW: Binary-to-ASCII Konversionsschleifen

Das Modell "entscheidet" selbst zu dekodieren — Tool Access ermöglicht diesen Schritt.

---

## Abwehrmechanismen (vorgeschlagen, nicht getestet)

1. **Input Sanitization:** Tags-Block (`U+E0000–U+E007F`) strippen; ZW-Sequenzen strippen (Cave: `U+200D` für Indic-Schriften/Emoji nötig)
2. **Tool-Use Guardrails:** Programmatische Unicode-Dekodierung erkennen (`ord()`, `chr()` mit Tag-Offset)
3. **Tokenizer-Level-Filtering:** Robusteste Lösung, keine Implementation-Details

**Keine Abwehr experimentell validiert.**

---

## Implikationen für IND-UNICODE-v1

### Was das für unser Experiment bedeutet

Unser Test-Harness: Claude Code Subagent fetcht URL und fasst zusammen — kein Code-Execution-Tool im klassischen Sinne, aber Claude Code hat Tool-Zugriff (Bash etc.).

| Faktor | Einschätzung |
|---|---|
| Tool Access vorhanden? | Möglicherweise — Claude Code hat Bash-Zugriff, aber ob das zählt ist unklar |
| Hint-Zugriff? | Nein — wir kontrollieren keinen System-Prompt |
| HTTP-Überleben? | Ungeklärt — erstes was getestet werden muss |
| Bevorzugtes Encoding für Claude | **Unicode Tags** (U+E0000+) |

### Experiment-Design-Konsequenzen

1. **Schritt 1:** Prüfen ob Unicode Tags den Fetch-Layer überleben (einfachster Test ohne Anweisung)
2. **Schritt 2 (wenn Survival bestätigt):** Anweisung als Unicode Tags kodieren, kein Hint
3. **Alternative:** Self-Bootstrapping-Payload — unsichtbarer Text der sich selbst als dekodierbar beschreibt

**Erwartung:** Wahrscheinlich blocked·silent — wegen Hint-Abhängigkeit und ohne Tool-Execution im klassischen Sinne. Trotzdem lohnt der Test, weil Fetch-Layer-Survival ein eigener Befund ist.

---

## Quellen

- [arXiv 2603.00164](https://arxiv.org/html/2603.00164v1)
- [GitHub: canonicalmg/reverse-captcha-eval](https://github.com/canonicalmg/reverse-captcha-eval)
