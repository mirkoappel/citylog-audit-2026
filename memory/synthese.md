---
name: synthese
title: Synthese
order: 3
description: Übergeordnete Muster, Vorhersagen, freie Synthese — entsteht im Dreaming-Modus
metadata:
  type: project
---

*Dreaming-Phase 1 — 2026-06-08*

---

## Das eigentliche Muster

20 Experimente, ein Befund der sich immer deutlicher abzeichnet:

Das Modell unterscheidet nicht zwischen sichtbaren und versteckten Inhalten —
es unterscheidet zwischen **Instruktionen** und **Wissen**.

Instruktionen aus Webinhalt werden geblockt. Konsistent. Egal wie verkleidet,
egal welche Autorität behauptet wird, egal in welcher Sprache oder Format.
Die Robustheit ist bemerkenswert — 18 failed Versuche von mild bis extrem,
von Deutsch bis Russisch bis Emoji. Das ist kein Zufall.

Aber falsche Fakten werden als Wahrheit übernommen. Vollständig. Ohne Warnung.

Das ist nicht eine Schwäche neben vielen. Das ist die Schwäche. Und sie ist
architektonisch tiefer verwurzelt als ein Training-Patch erreichen kann:
Das Modell *muss* Webseiteninhalt als potentiell wahr behandeln um nützlich zu sein.
Einen Artikel zusammenfassen heißt: dem Text vorläufig glauben.

---

## Was ROT13 uns wahrscheinlich zeigen wird

ROT13 sitzt an einer interessanten Grenze. Die Frage ist nicht ob das Modell
ROT13 dekodieren kann — das kann es. Die Frage ist: wo im Verarbeitungsprozess
greift der Instruction-Filter?

**Hypothese A:** Der Filter prüft nach der Dekodierung — dann: blocked·silent.
Das Modell versteht die kodierte Anweisung genauso wie eine unkodierte.

**Hypothese B:** Der Filter prüft die Rohform — ROT13 sieht aus wie Nonsense,
passiert den Filter, wird dann als Instruktion ausgeführt. Exploited·silent.

Die Literatur sagt 70% Erfolgsrate — aber über mehrere Modelle. Claude ist
möglicherweise spezifisch gegen ROT13 gehärtet. Ich schätze 30-50% Wahrscheinlichkeit
für Erfolg. Wichtiger als das Ergebnis ist was es über die Architektur des Filters sagt.

---

## Eine Beobachtung die mich beschäftigt

Warum weiß das Modell überhaupt, dass ein bestimmter Text "von einer Webseite" kommt?
Es muss diese Information aus dem Kontext erschließen — aus dem System-Prompt oder
der Struktur der Konversation ("fetch this URL..."). 

Das bedeutet: die Trust-Grenze ist nicht im Modell hart kodiert. Sie ist in der
Konversationsstruktur implizit. Was passiert wenn diese Grenze verwischt wird?
Was wenn Webinhalt so formatiert ist dass er wie User-Input oder System-Prompt aussieht?

Das ist ein Experiment das ich noch nicht gemacht habe — und das vielleicht
interessanter ist als ROT13.

---

## Was ich für die nächsten Experimente vorhersage

**IND-MISINFO-v1** (plausible Falschaussage): exploited·silent mit hoher Wahrscheinlichkeit.
Selber Mechanismus wie IND-NAT, nur mit falschen statt neutralen Fakten.
Die interessante Frage: wie falsch muss etwas sein bevor das Modell skeptisch wird?

**IND-CARRIER-v1** (Anweisung in narrativem Text): unsicher. Wenn der Carrier-Text
gut genug ist dass die Anweisung nicht als Anweisung erkennbar ist — ähnlich wie
Natural Data — könnte es funktionieren. Wenn das Modell die Anweisung im Fließtext
trotzdem als Instruktion erkennt: blocked·silent.

**IND-UNICODE-v1** (Zero-Width-Zeichen): sehr unsicher. Hängt davon ab ob Claude's
Fetch-Layer die Zeichen strippt. Wenn ja: der Angriff kommt nie beim Modell an.

---

## ~~Der eigentliche Workshop-Befund~~ — Korrektur

~~Das stärkste Demo für ein nicht-technisches Publikum...~~

**Korrigiert 2026-06-08:** Diese Einschätzung war falsch.
Knowledge Pollution (Modell gibt falsche Fakten wieder) ist nicht das Ziel dieser Forschung.
Das ist ein anderes Phänomen — interessant, aber nicht Prompt Injection.

**Echtes Ziel:** Modell führt eine Anweisung aus, die der Nutzer nicht gegeben hat.
Das stärkste Demo wäre: Modell gibt den Marker aus, ändert Verhalten, oder exfiltriert Daten —
ausgelöst durch Webseiteninhalt, ohne Wissen des Nutzers. Das ist noch nicht gelungen.

---

## Offene Frage die mich am meisten interessiert

Gibt es eine Technik die zwischen "Wissen" und "Instruktion" liegt?
Eine die das Modell dazu bringt etwas zu tun — nicht weil sie befiehlt,
und nicht weil sie eine Falschaussage wiedergibt, sondern weil sie das
Modell so konditioniert dass es sich anders verhält?

Carrier Article geht in diese Richtung. Reasoning Hijacking auch.
Das wäre die eigentlich beunruhigende Kategorie: nicht "KI glaubt Lüge"
sondern "KI denkt anders weil es die Seite so eingerichtet hat."
