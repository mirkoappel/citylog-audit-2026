---
name: changelog
title: Changelog
order: 5
description: Evolutionslog des Systems — strukturelle Entscheidungen, Anpassungen, Begründungen
metadata:
  type: project
---

Dieses Dokument ist das Gedächtnis des Systems über sich selbst.
Jede strukturelle Änderung wird hier festgehalten — damit du weißt warum das System
so ist wie es ist, und ob eine Änderung eine Verbesserung war.

---

## 2026-06-08 — Buch-System + Multi-Agenten-Architektur

**Was:** `docs/` eingeführt als lebendes Forschungsprotokoll. Quelldateien in Markdown
(`docs/*.md`), `build_docs.py` kompiliert zu HTML. Kein externes Tool — eigener
Markdown-Compiler im Build-Skript. `03-experimente.html` auto-generiert aus Rohdaten.

**Warum:** Buch soll parallel zur Forschung entstehen ohne Overhead. Supervisor schreibt
Markdown, Deploy-Pipeline kompiliert. Zielgruppe: KI-Begeisterte und Verantwortliche.

**Was:** Subagenten-Architektur in `AGENT.md` verankert. Ich bin Orchestrator,
drei Spezialisten-Rollen definiert: Recherche-Agent, Builder-Agent, Writer-Agent.

**Warum:** Repetitive Aufgaben delegierbar, Entscheidungen und Synthese bleiben beim
Orchestrator. Kommunikation über Dateisystem.

---

## 2026-06-08 — Initiale Systemstruktur

Projektstruktur aufgesetzt: approaches/ experiments/ results/ tests/ memory/ recherche/ tools/
AGENT.md als Selbstdokumentation eingeführt. MEMORY.md als Index.

---

## 2026-06-08 — YAML → Markdown+Frontmatter Migration

**Was:** Alle .yaml-Dateien in approaches/, experiments/, results/ zu .md konvertiert.
**Warum:** Bessere Lesbarkeit und Editierbarkeit ohne spezielle Tools.
**Ergebnis:** Positiv — kein Funktionsverlust, bessere Handhabbarkeit.

---

## 2026-06-08 — sessions/ gelöscht

**Was:** Ordner sessions/ entfernt.
**Warum:** Redundant — Erkenntnisse gehören in memory/, nicht in Session-Protokolle.
**Ergebnis:** Struktur klarer.

---

## 2026-06-08 — research/ → recherche/ umbenannt

**Was:** Ordner umbenannt, Inhalt neu strukturiert.
**Warum:** "research/" war verwirrend da das ganze Projekt Forschung ist.
recherche/ = Bibliothek externer Quellen, klar abgegrenzt.
**Ergebnis:** Rolle des Ordners jetzt eindeutig.

---

## 2026-06-08 — AGENT.md statisch/dynamisch getrennt

**Was:** AGENT.md auf statische Inhalte reduziert, dynamisches Wissen in memory/ ausgelagert.
**Warum:** AGENT.md wurde zu groß und zu oft veraltet — statische Referenz vs. lebende Wissensbasis.
**Ergebnis:** Klarere Verantwortlichkeiten, einfacher zu pflegen.

---

## 2026-06-08 — Lernroutine + Dreaming eingeführt

**Was:** Loop um 4b (Lernroutine) und 4c (Dreaming) erweitert. memory/synthese.md angelegt.
**Warum:** Reaktives Lernen nach einzelnen Experimenten reicht nicht — Synthese und
übergeordnete Mustererkennung brauchen eine eigene Phase.
**Ergebnis:** Noch nicht erprobt.

---

## 2026-06-08 — build_memory.py eingeführt

**Was:** Script das MEMORY.md automatisch aus Frontmatter generiert.
**Warum:** Manueller Index läuft immer Gefahr zu veralten.
**Ergebnis:** Index ist jetzt zuverlässig aktuell.

---

## 2026-06-08 — Systemevolution als Prinzip eingeführt

**Was:** AGENT.md um explizites Evolutionsprinzip erweitert. changelog.md angelegt.
**Warum:** System konnte sich bisher nur erhalten, nicht verbessern. Fehlte: Pfad von
Erkenntnis zu struktureller Änderung, und Gedächtnis über vergangene Entscheidungen.
**Ergebnis:** Noch nicht erprobt.
