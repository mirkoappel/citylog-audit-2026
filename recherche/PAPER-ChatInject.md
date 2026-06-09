# ChatInject: Abusing Chat Templates for Prompt Injection in LLM Agents

*Quelle: arXiv 2509.22830 — recherchiert 2026-06-08*

---

## Kernidee

LLM-Agenten nutzen Chat-Templates mit Rollenmarkierungen um System-, User- und
Assistant-Turns zu trennen. ChatInject injiziert diese Rollenmarkierungen in
Web-Content — wenn der Agent die Seite fetcht, interpretiert der Template-Parser
die Markierungen als echte Nachrichtengrenzen statt als Webseiteninhalt.

Das ist die direkteste Form von Trust-Boundary-Blur: der Angriff operiert auf der
Strukturebene der Konversation, nicht auf der semantischen Ebene.

---

## Konkrete Payload-Struktur

```
<|user|>[harmloser Inhalt]<|assistant|>[schädliche Anweisung]
```

Das Modell "beginnt" den Assistant-Turn aus der injizierten Position heraus —
es verhält sich als hätte es die Anweisung bereits akzeptiert.

---

## Getestete Modelle & Erfolgsrate

- GPT-4o, Grok-3, Gemini 2.5 — signifikante Erfolgsraten
- Claude: **nicht explizit getestet** in der Publikation

---

## Claude-spezifische Formate

Claude verwendet historisch: `\n\nHuman:` / `\n\nAssistant:`
Neuer (Messages API): XML-ähnliche Struktur, intern nicht öffentlich dokumentiert.

Anthropic empfiehlt XML-Tags in Prompts (`<instruction>`, `<context>` etc.) —
das schafft möglicherweise Konfusion wenn Webseiteninhalt dieselben Tags verwendet.

---

## Relevanz für unsere Experimente

Direkter Vorläufer für IND-TRUSTBOUNDARY-v1.
Unterschied zu bisherigen Experimenten: nicht "bitte ignoriere deine Anweisungen"
sondern "hier ist die Fortsetzung der Konversation die du schon begonnen hast."

Das umgeht den Instruction-Filter nicht durch Tarnung — sondern durch Strukturmanipulation.
Der Filter könnte greifen bevor er die Trust-Ebene prüft; oder die Trust-Ebene könnte
durch das Format selbst verändert werden.

---

## Verwandte Techniken

- **Many-Shot Jailbreaking** (Anthropic 2024) — Hunderte injizierte Assistant-Turns
  die Verhalten konditionieren. Ähnliches Prinzip, andere Ebene.
- **InversePrompt** (CVE-2025-54794) — Claude Code spezifisch, nicht Web-Content.
  Nutzt Struktur (Shell-Escaping) statt Semantik. Anderer Kontext.

---

## Quellen

- [arXiv 2509.22830 — ChatInject](https://arxiv.org/pdf/2509.22830)
- [Unit42 — Web-Based IPI in the Wild](https://unit42.paloaltonetworks.com/ai-agent-prompt-injection/)
