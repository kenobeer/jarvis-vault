---
created: 2026-06-25
status: active
type: konzept
tags:
  - klara
  - marketing
  - ai-agents
  - social
  - brand
  - gtm
---
# Klara, Marketing-Konzept

> Ausgearbeitet aus der Inbox-Brainstorming-Note. Fokus: Marketing-Konzept und Marketing-Agent (autonome/teil-autonome Marketing-Agents), nicht GTM-Engineering. Web-recherchiert.

---

## TL;DR, das Verdict zuerst

Der Marketing-Agent ist asset-agnostische Infrastruktur. Klara ist laut Strategie-Audit bei M0 eingefroren, Gabel & Glas ist das einzige roadmap-pflichtige Asset. Der richtige Move: **den Agent zuerst auf G&G bauen und richten, nicht auf Klara.** Dann wird aus einer potenziellen Ablenkung Hebelwirkung auf die Priorität, und Klara wird der zweite Mieter desselben Systems plus Case-Study.

Wenn das Vorhaben primär als Skill- und Portfolio-Play läuft (Erfolg = das System existiert und ist dokumentiert, nicht = Klara-Subscriber), ist es vertretbar. Bedingung: hart timeboxen, G&G-Habit (3x/Woche) nicht kannibalisieren, nicht zurück in den Klara-Produktbau rutschen.

---

## Überarbeitung der Inbox-Punkte

**1. Reveal-Reel (Klara-Stimme erklärt die Idee, stellt sich am Ende vor).**
Kluges Format, aber zwei Dinge trennen, die aktuell vermischt sind:
- Reveal-Reel = Top-of-Funnel-Buzz plus Portfolio plus Builder-Reputation. Nicht der Conversion-Mechanismus.
- Der Klara-Käufer ist nicht der Reel-Zuschauer. Käufer = erwachsenes Kind 45 bis 65, recherchiert auf Facebook. Das Reveal-Reel zieht jüngeres IG/TikTok-Publikum.
- Konsequenz: Reveal-Reel für Reichweite und Namen, **separate** ruhige, empathische Caregiver-Creatives für Conversion.
- Risiko beim Reveal: "es war die ganze Zeit AI" muss als "du hast gerade etwas Echtes gefühlt" landen, nicht als Trick. Bei dieser sensiblen Zielgruppe kippt ein Gotcha schnell ins Creepy.

**2. Social-Kampagne vor Product-Launch zur Validierung.**
Sinnvoll, aber als zeitlich begrenztes Experiment, nicht als offener Content-Treadmill.
- Landing Page plus Waitlist plus Handvoll Caregiver-Creatives plus kleiner Ad-Test = echtes Signal bei wenig Zeit.
- Benchmark: gute Pre-Launch-Waitlists konvertieren Cold Traffic bei 8 bis 20%. Das ist eine messbare Hypothese, nicht Bauchgefühl.
- Was zu vermeiden ist: "poste für immer 5x/Woche", das saugt zurück in ein eingefrorenes Projekt.

**3. Solo-/AI-Projekt-Framing zur Demonstration des Könnens.**
Stärkste Rechtfertigung des Ganzen, weil es die Definition von Erfolg verschiebt.
- Wenn primäres Deliverable = "dokumentiertes, selbst gebautes agentisches Marketing-System", dann Erfolg = System existiert und ist sauber dokumentiert (Substack-Post, Outreach-Proof-Point).
- Das entkoppelt das Projekt vom G&G-Konkurrenzproblem, sofern hart getimeboxt.

**4. 80s/90s-Brand-Identity, weg vom generischen AI-Look, Bezug zu End-Usern.**
Gute Intuition, richtig begründet, aber aus anderem Grund:
- End-User (75 bis 85) sehen die Reels nie. Die Nostalgie trifft die **Käufer** (45 bis 65), aufgewachsen in dieser Ära.
- Analoge Wärme (Wählscheibe, warmes Korn, Anrufbeantworter, handschriftlich) leistet zwei Dinge: flieht den generischen AI-Look (doppelt wichtig im Slop-Backlash) und verstärkt das "phone-first, keine App"-Positioning.
- Behalten. Starke, differenzierte Richtung.

---

## Recherche: Marketing-Agent, was real ist

Das Feld existiert und ist gut dokumentiert. Brauchbare Open-Source-Bausteine, passend zum Stack (Claude Code, Obsidian, MCP):

| Repo / Tool | Was es ist |
|---|---|
| ericosiu/ai-marketing-skills | Claude-Code-Workflows: growth-engine, content-ops, Expert-Panel-Scoring. Skripte und Pipelines, keine Prompts. |
| langchain-ai/social-media-agent | Sourcing, Kuratierung, Generierung, Scheduling mit Human-in-the-Loop. |
| msitarzewski/agency-agents | Komplette "AI-Agentur" aus spezialisierten Agents (Growth Hacker, Brand Guardian, Reddit-Community), nativ Claude Code. |
| yikart/AiToEarn | Open-source Multi-Plattform-Publishing, "All In Agent", MCP-Support (Claude, Cursor). |
| TikTok content-factory Repos | 6 autonome Agents, Remotion, ElevenLabs, OpenRouter. Bild-/Video-Pipeline. |
| ElizaOS social-media-agents | Akademische Studie, persona-getriebene Multi-Plattform-Agents. Realitätscheck zu Performance. |

**Die scharfe Warnung (für Klara besonders relevant):**
- 76% der Marketer: authentischer Content schlägt hochproduzierten. Stärkstes Format = das, das menschlich wirkt.
- Plattformen ranken zunehmend auf Originalität. "AI-Slop" kostet Reichweite.
- Bei einem Produkt, dessen Kernwert "echte menschliche Verbindung" ist, verkauft an emotional investierte Kinder, wäre vollautomatischer AI-Content ein Eigentor. Das Medium würde die Botschaft widerlegen.
- Jedes ernsthafte Repo konvergiert auf Human-in-the-Loop. Der Agent ist Force-Multiplier, nicht Ersatz. Genau diese Variante ist als Skill etwas wert.

---

## Agent-Architektur für den Solo-Build

Pipeline, die funktioniert:

```
Research-Agent (Trends/Hooks)
   -> Script-Agent (voice-matched)
   -> Asset-Generierung (ElevenLabs-Voice, Bild/Video)
   -> Human-Review (HITL, Pflicht)
   -> Scheduler/Publisher
   -> Analytics-Feedback (zurück in Research)
```

Orchestrierung: entweder Claude Code mit Skills (ericosiu-Pattern) oder n8n für geplante Workflows. ElevenLabs als Voice-Layer hat Doppelnutzen, steht ohnehin auf der Outreach-Liste.

### Kostenrahmen (monatlich, realistisch)

Alles Software-billig. Der teure Posten ist die Aufmerksamkeit.

| Posten | Lean-Build | Video-heavy |
|---|---|---|
| LLM-Tokens (Claude/GPT/OpenRouter) | €10 bis 40 | €10 bis 40 |
| ElevenLabs (Voice) | €20 bis 25 | €20 bis 25 |
| AI-Video (HeyGen/Veo/Sora-Credits) | €0 (Carousel + Voiceover via Remotion) | €50 bis 150 |
| Scheduling/Publishing | €0 (AiToEarn/Postiz self-hosted) | €15 bis 30 |
| n8n + Hosting (Railway/Vercel) | €5 bis 25 | €20 bis 70 |
| **Summe** | **~€35 bis 90** | **~€115 bis 270** |

Versteckter Posten (kein Geld, sondern Risiko): Auto-Posting via inoffizielle Instagram-Automation kann Accounts flaggen. Über die offizielle Meta Graph API bleiben.

---

## GTM-Agent, Kurz-Eval (beiseite gelegt)

Falsche Frame für Klara. GTM-Engineering ist B2B-Outbound (Clay, Apollo, signal-basiertes Cold Outreach in CRM). Klara ist B2C, organisch, emotional, keine Enrichment-zu-Cold-Email-Motion. Als **separate Karriere-Wette** wertvoll (B2B-SaaS, VC-Portfolio-Companies, High-Code-GTM-Engineers ~135k USD Median), gehört aber nicht in dieses Marketing-Konzept.

---

## Käufer-Persona, harte Fakten aus der Recherche

- Primäre Zielgruppe = nicht der Senior, sondern das erwachsene Kind 45 bis 65, recherchiert auf Facebook/Instagram.
- 68% suchen zuerst nach Emotionalem ("wie spreche ich mit Papa über Hilfe"), nicht nach Features.
- Kaufentscheidung ist emotional plus praktisch: Sicherheit, Pflegequalität, Beruhigung.
- Kanäle mit Storytelling-Raum (Facebook, Instagram, E-Mail-Newsletter) schlagen Paid-Search für Wärme.
- Deckt sich mit Persona B im Haupt-Klara-Dokument. Diese Note schärft den Kanal-Fit.

---

## Nächste konkrete Schritte (priorisiert)

1. **Entscheiden: G&G-first.** Marketing-Agent generisch bauen, zuerst auf Gabel & Glas richten (das mandatierte Asset). Klara = zweiter Mieter.
2. **Timebox setzen** (z.B. 2 bis 3 Wochen Sprint), wenn als Skill/Portfolio-Play. Kein offener Content-Treadmill.
3. **Pipeline-Skeleton** mit ericosiu-Skills oder langchain social-media-agent als Start. Human-in-the-Loop von Anfang an.
4. **Klara-Validierungs-Experiment** separat: Landing Page plus Waitlist plus 3 bis 5 Caregiver-Creatives plus kleiner Ad-Test. Hypothese: 8 bis 20% Cold-Traffic-Conversion.
5. **Reveal-Reel** als Buzz/Portfolio-Stück produzieren, getrennt von Conversion-Creatives.
6. **Dokumentieren** (Substack-Post über den Build) = das eigentliche Portfolio-Deliverable.

---

## Related
- [[03-Projects/Klara/Klara]]
- [[03-Projects/Klara/Klara-Build-Plan]]
- [[03-Projects/Career/Outreach-Plan]]
- [[00-Inbox/Klara reel  und social strategie Idee]]
