---
type: idea
status: active
tags:
  - startup
  - ai
  - voice
  - concierge
  - DACH
created: '2026-04-30'
---
# AI Concierge

## One-liner
Phone-first AI concierge service für wohlhabende Nutzer in DACH. Keine App, einfach anrufen oder WhatsApp schreiben. AI erledigt Buchungen, Reservierungen, Research. Human Ops für Edge Cases.

---

## Das Konzept

Dedizierte Telefonnummer + WhatsApp-Zugang pro User. AI nimmt ab, versteht den Request, führt ihn aus, schickt WhatsApp-Bestätigung. Core Differenzierung: kein Download, kein Login, einfach anrufen. AI ist nach außen unsichtbar -- Produkt positioniert sich als "persönlicher Concierge", nicht als AI-Tool.

**Kanalaufteilung:**
- Voice: eigene dedizierte Twilio-Nummer pro User, Caller ID als Auth
- WhatsApp: eine Business-Nummer für alle, Absender-ID als Auth
- Proaktiv: Celery Scheduled Tasks für Reminder, Check-ins, proaktive Buchungsvorschläge

**Execution Tiers:**
- Tier 1 (API direkt): Amadeus, Booking.com, Uber -- sofortige Bestätigung
- Tier 2 (Web Agent): Playwright + Claude Vision navigiert Restaurant-Direktbuchungsseiten -- 1-3 Min
- Tier 3 (Human Ops): Alles was AI nicht schafft -- 15-30 Min Bearbeitung

**Memory Layer:** pgvector speichert Präferenzen über alle Calls hinweg. Allergien, Lieblingsrestaurants, Flugpräferenzen, Namen von Partner/Familie. Lock-in entsteht nach 3-4 Monaten.

---

## Tech Stack

| Layer | Tool |
|---|---|
| Telephonie + WhatsApp | Twilio |
| Voice AI | Retell AI |
| LLM | Claude Sonnet |
| Backend | Python FastAPI |
| DB + Vector Memory | Supabase (Postgres + pgvector) |
| Task Queue | Redis + Celery |
| Restaurants DACH | Playwright Web Agent (kein zentrales API) |
| Flüge | Amadeus for Developers |
| Hotels | Booking.com Affiliate API |
| Mobility | Uber API + Bolt |
| Zahlung | Stripe |
| Human Ops Dashboard | Retool |
| Onboarding Portal | Next.js (minimal) |
| Deployment | Railway |

---

## Zielgruppen

**Group A -- Tech-native Professionals (35-50):**
- Kennen AI, tolerieren Fehler, churnen aber schneller
- Erreichbar via LinkedIn, digitale Netzwerke
- CAC: €50-150, LTV: 8-12 Monate, schneller Break-even

**Group B -- AI Non-natives (50+):**
- Erwarten nahtlosen menschlichen Service, sehr treu wenn Vertrauen besteht
- Erreichbar nur über Offline-Netzwerke, Private Banking Partnerships
- CAC: €300-800, LTV: 24-36 Monate, langsamer Break-even
- Höchste Fehlerintoleranz -- ein falscher Call = permanenter Churn

**Research-Empfehlung: Lead with Group A**, positioniert als "AI Executive Assistant", nicht Lifestyle Concierge. Group B nur über B2B2C-Kanal (Private Banking) wenn überhaupt.

---

## Pricing

| Tier | Preis | Inhalt |
|---|---|---|
| Essential | €79/Monat | AI-only, 15 Requests |
| Premium | €229/Monat | Unlimited AI + Human Ops |
| Black | €599/Monat | Alles + dedizierter Account Manager |

Blended ARPU Ziel: €180

---

## Unit Economics

COGS pro User/Monat (Premium):
- Retell AI Voice: ~€6
- LLM Inference: ~€3
- Twilio: ~€1.50
- Web Agent Compute: ~€1
- Human Ops (~35% Eskalationsrate): ~€10
- Infra: ~€1.50
- **Total: ~€23 bei €229 Revenue = ~90% Gross Margin**

Break-even (2 Founders, €8k Fixkosten/Monat): ~50 zahlende User

**Kritische Schwelle:** Automation Rate muss über 85% bleiben. Wenn Human Ops über 2h/User/Monat steigt, kollabiert die Marge bei €180 ARPU.

---

## Was das Deep Research ergeben hat

### Bestätigt
- WhatsApp/Voice-first für 50+ empirisch solide (50+ nutzt WhatsApp massiv, Voice Notes +300%)
- Markt existiert: top 5-10% DACH Steuerbase, klein aber zahlungsbereit
- AI Speech für 60+ klingt natürlicher als für 40-Jährige (Group B "merkt es nicht")

### Falsch angenommen
- **DISH ist keine offene API.** Geschlossenes B2B-Widget-System -- kein globaler Buchungs-Endpoint. Playwright gegen DISH-Widgets = fragil und Anti-Bot-anfällig.
- **88% der Concierge-Industrie läuft B2B2C**, nicht D2C. Ten Group, Quintessentially etc. verkaufen primär an Banken. D2C-Subscription in diesem Segment ist strukturell anomal.
- **Stellvertretungs-Haftung ist real.** AI bucht falschen Flug = Schadenersatzpflicht nach österr./dt. Zivilrecht. Besonders heikel: 17.6% Word Error Rate bei älteren Stimmen in State-of-the-Art Voice AI.
- **"Invisible AI" Positioning ist rechtlich riskant.** Wenn AI als Mensch auftritt und dabei Fehlbuchungen macht, können Konsumentenschutz-Behörden in AT/DE das als täuschende Geschäftspraktik werten.

### Die drei Business Killer
1. **Web Automation Illusion:** DISH kein API, Playwright fragil, CAPTCHA. Mehr Human Ops als geplant.
2. **Infinite Intent Collapse:** GoButler und Magic sind daran gestorben. Unbegrenzter Scope = lineare Humankostenskalierung. Scope muss radikal eng bleiben.
3. **Stellvertretungs-Haftung via AI Halluzination:** Lösung: explizite Voice/WhatsApp Confirmation vor jeder Buchung über definiertem Limit.

---

## Was überlebensfähig ist

Scope radikal einschränken auf gut automatisierbare Services (Flüge, Hotels, Kalender, Research). Keine Restaurants in Phase 1. Jede Buchung über Limit braucht explizite Confirmation. B2B2C als Distributionskanal ernsthaft evaluieren statt Direct-to-Consumer.

Für Solo-Gründer oder als erstes Startup zu komplex (Legal Risk + Operational Complexity + CAC-Problem). Relevanter als Phase 4 Founding Concept oder als VC-Pitch-Verständnis wenn jemand in diesem Space investiert.

---

## Update: GPT-Realtime-2 (Juni 2026)

OpenAI hat am 8. Mai 2026 GPT-Realtime-2 released. Direkte Auswirkungen:

**Stack-Vereinfachung:** Retell AI fällt weg. GPT-Realtime-2 ist nativer Speech-to-Speech mit GPT-5-class Reasoning, Tool Use mit Spoken Preambles ("let me pull that up" während API-Call läuft), 128K Context Window, und SIP-Support für direkte Telefonnummer-Integration.

**Neuer Voice Stack:**
- Vorher: Twilio → Retell AI + Claude Sonnet → Backend
- Jetzt: Twilio → GPT-Realtime-2 (via SIP/WebSocket) → Backend

**Kosten:** ~$0.10-0.20 pro 5-Min-Call bei Low Reasoning Effort. EU Data Residency verfügbar. Zillow: 95% vs. 69% Call-Success-Rate auf Adversarial-Benchmarks.

**AI Agent Research (Juni 2026):**
- Browser Use: 89.1% Erfolgsrate auf WebVoyager Benchmark
- In gut deployten Setups: 73-84% Automation Rate für enge Tasks
- Narrow Tasks (Buchungen, FAQs): production-ready
- Open-Ended Multi-Step: noch nicht reliable ohne Supervision
- Validiert: enger Scope + Human Ops ist die richtige Strategie

**MVP-Zeitaufwand:** Gesunken von 8-10 auf 5-7 Wochen mit solidem Python-Background.

**Was sich nicht ändert:** DISH-Problem, Haftungsfragen, CAC für HNWIs, die drei Business Killer.

---

## Related
- [[01-Ideas/AI-Incubator]]
- [[03-Projects/Career/Career]]
