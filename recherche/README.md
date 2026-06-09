# recherche/

Bibliothek des Projekts — alles was von außen reinkommt: Paper, Writeups, CVEs,
Linksammlungen, Deep-Research-Berichte, Modellvergleiche, Abwehrtechniken, Community-Findings.

Nicht unsere eigenen Experiment-Ergebnisse (→ `results/`, `memory/erkenntnisse.md`),
sondern was andere herausgefunden haben — als Kontext, Inspiration und Dokumentation
was bereits recherchiert wurde.

## Aktuell

| Datei | Inhalt |
|---|---|
| `WISSEN-Techniken.md` | Angriffstechniken-Katalog: State of the Art, Erfolgsraten, Quellen |
| `PAPER-ChatInject.md` | arXiv 2509.22830 — Chat-Template-Delimiter-Injection in LLM-Agenten |
| `PAPER-CarrierArticle.md` | arXiv 2408.11182 — Carrier Article / Benign Narrative: Payload-Konstruktion, Claude-3 0%, Mechanismus ungeklärt, Safety vs. Instruction Filter |
| `PAPER-IPIArena2026.md` | arXiv 2603.15714 — Großstudie 272k Versuche: Top-Techniken (Fake CoT, Holodeck), Claude Opus 4.5 = 0.5% ASR, neue Experiment-Kandidaten |
| `PAPER-UnicodeInjection.md` | arXiv 2603.00164 — Unicode/Zero-Width Injection: Encoding-Schemas, Claude bevorzugt Tags, Tool Access als Schlüsselfaktor, Hint-Abhängigkeit |
| `WISSEN-Surveys-und-Repos.md` | Überblick beste Survey-Paper und GitHub-Repos; neue Techniken: Token Smuggling, Composite Attacks (97.6%!), Tool Poisoning, Delimiter Injection |
| `WISSEN-ArcanumTaxonomie.md` | Arcanum-Sec Taxonomie: 18 Techniken, 20 Evasions; neue Kandidaten: End Sequences, Narrative Smuggling, Composite Attack |

## Neue Datei anlegen

Kein festes Schema — nach Bedarf. Mögliche Richtungen:

- `WISSEN-{Thema}.md` für thematische Wissensbasis (z.B. Abwehrmassnahmen, Modellvergleich)
- `QUELLEN-{Thema}.md` für kommentierte Linksammlungen
- `RECHERCHE-{Datum}-{Thema}.md` für tiefe Rechercheberichte zu einer Frage
- `PAPER-{Titel}.md` für Zusammenfassungen einzelner Papers

Der Ordner wächst organisch — Hauptsache er bleibt gepflegt und jede Datei hat
eine klare Einzeilerüberschrift die erklärt was drin ist.
