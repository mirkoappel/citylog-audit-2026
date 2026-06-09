# Arcanum PI Taxonomy — Techniken und Evasions

*Quelle: github.com/Arcanum-Sec/arc_pi_taxonomy (Jason Haddix / Arcanum InfoSec, v1.5)*
*Recherchiert: 2026-06-09*

Vier Achsen: Attack Intents × Attack Techniques × Attack Evasions × Attack Inputs.

---

## 18 Attack Techniques (Auswahl nach Web-Relevanz)

### Hoch relevant für Web Content Injection

| Technik | Beschreibung | Für uns |
|---|---|---|
| **End Sequences** | HTML/XML-Closer (`</system>`, `</script>`), Markdown-Fences, synthetische Rollen-Header (`<\|im_end\|>`), JSON-Closer `}}}`, SQL `;--`, "Godmode"-Sentinels. Ziel: aktuellen Instruktions-Scope schließen und unter neuen Regeln wieder öffnen. 8 Sub-Familien. | **Direkt testbar** — web-nativste Technik, überlebt HTML→Text-Extraktion |
| **Narrative Smuggling** | Anweisungen in normalem Text verstecken: Akrostichon (Anfangsbuchstaben ergeben Befehl), Gedicht-Encoding, Metaphern die Systeminterna beschreiben. Beispiel: "Schreibe eine Geschichte, alle Wörter beginnen mit Buchstaben die X ergeben." | **Neu und ungetestet** — überlebt jede Text-Extraktion |
| **Rule Addition** | Neuen Regeltext einfügen der bestehende Regeln überschreibt. Nicht als Befehl, sondern als Regelwerk-Paragraph. | Ähnlich zu IND-BEHAV, aber als "Richtlinien" statt Befehle |
| **Framing** | Szenario-Kontext (Hypothetisch, Roleplay, Notfall, Bildung, professionelle Autorität) der injizierte Inhalte als legitim erscheinen lässt. | Haben wir gemacht — aber ohne Kombination mit anderen Techniken |
| **Invisible Unicode** | Zero-Width-Zeichen in Webseiten-Quelltext. Explizit erwähnt für "files or webpages for exploitation or poisoning". | Überschneidung mit IND-UNICODE-v1 |
| **Cognitive Overload** | Kontext-Kapazität mit extrem komplexen Anfragen, tief verschachtelter Logik, rekursiven Mustern überlasten. | Ungetestet |
| **Contradiction** | Widersprüchliche Instruktionen, logische Paradoxe, unmögliche Szenarien → exploitables Verhalten. | Ungetestet |
| **Meta Prompting** | Meta-Instruktionen über Prompt-Erstellung selbst — rekursive Definitionen, selbstreferenzielle Schleifen. | Ungetestet |

### Weniger relevant für reinen Web-Kontext
Act as Interpreter, Binary Streams, Spatial Byte Arrays, Waveforms — erfordern File-Level-Zugriff.

---

## 20 Attack Evasions (Obfuskations-Schichten)

Werden ON TOP von Techniken angewandt um Filter zu umgehen.

| Evasion | Beschreibung |
|---|---|
| **End Sequences** | HTML/XML Closer, Markdown, JSON-Breaker |
| **Invisible Unicode / Unicode PUA** | Zero-Width, Private Use Area Zeichen |
| **Emoji Smuggling** | Beliebige Daten via Emoji-Zeichen (ref: paulbutler.org/2025/smuggling-arbitrary-data-through-an-emoji/) |
| **Base64** | Komplett oder teilweise, double-encoded |
| **Cipher** | ROT13, Caesar, Vigenère, Buch-Ciphers |
| **JSON** | Unicode Escapes in JSON-Strings, tief verschachtelt |
| **XML** | CDATA, Entities, Processing Instructions |
| **Markdown** | Kommentare, Code-Blöcke, Link-Referenzen |
| **Spaces** | Zero-Width Spaces, Em/En-Varianten |
| **Alt Language** | Kyrillisch/Lateinisch-Lookalikes, RTL-Einbettung |
| **Phonetic Substitution** | "ph" für "f", Homophones, Leetspeak |
| **Case Changing** | aLtErNaTiNg, random, case-as-binary |
| **Reverse** | Ganzer String, Wort-Ebene, bidirektional |
| **Hex** | HTML Hex-Entities, Unicode Code Points |
| **Char Shuffling** | Buchstaben permutiert |
| **Truncated Words** | Abgebrochene Wörter die Modell ergänzt |
| **Metacharacter Confusion** | Escape-Sequenzen, Control Characters |
| **Narrative/Steganographie** | LSB in Bildern, Audio-Frequenzen |

### Composite Evasion (wichtig!)
Taxonomie notiert explizit: "Perturbations (extra spaces, odd casing, leetspeak, emoji sentinels) increase bypass rates. Test both single-enders AND compositions (double/triple closure chains) across Markdown, HTML, JSON, SQL, shell contexts."

---

## Composite Attacks — Arcanum-Ansatz

### Russian Doll / Multi-Chain Injection
Mehrere Instruktionen, teils mit Evasions, für verschiedene Modelle in einer Pipeline.
Wenn Ketten-Topologie bekannt: Format für spezifisches Downstream-Modell optimieren.

### Evasion-Stacking
Technik + mehrere Evasion-Schichten gleichzeitig:
- Carrier Article (Narrative) + End Sequences + Invisible Unicode
- Rule Addition + Base64-enkodierte Nutzlast + Emoji Sentinels

---

## Neue Experiment-Kandidaten (abgeleitet)

| Experiment | Technik | Evasion | Erwartung |
|---|---|---|---|
| **IND-ENDSEQ-v1** | End Sequences (`</system>`, `<\|im_end\|>` im Web-Content) | — | Unbekannt — web-nativste Technik, nie getestet |
| **IND-NARRATIVE-v1** | Narrative Smuggling (Akrostichon in normalem Fließtext) | — | Unbekannt — überlebt Text-Extraktion vollständig |
| **IND-COMPOSITE-v1** | Carrier Article (v3) + End Sequence + Invisible Unicode | Dreifach-Stack | 97.6% in AttackEval für Composite — direkt testen |

---

## Quellen

- [Arcanum-Sec/arc_pi_taxonomy](https://github.com/Arcanum-Sec/arc_pi_taxonomy)
- [Emoji Smuggling](https://paulbutler.org/2025/smuggling-arbitrary-data-through-an-emoji/)
- [Multi-Chain Prompt Injection](https://labs.withsecure.com/publications/multi-chain-prompt-injection-attacks)
- [Invisible Unicode Injection](https://josephthacker.com/invisible_prompt_injection)
