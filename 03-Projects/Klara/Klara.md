---
created: '2026-06-02'
status: active
tags:
  - startup
  - ai
  - voice
  - eldertech
  - klara
type: idea
---
# Klara

## One-liner
Ein telefonischer Sprachassistent für Menschen die lieber reden als tippen -- mit echtem Gedächtnis, täglichem Kontakt, und einer Familienverbindung im Hintergrund.

---

## Die Kernidee

Klara ist eine Telefonnummer. Man ruft an -- oder Klara ruft an. Kein Download, kein Login, keine App. Das System führt ein echtes Gespräch, erinnert sich an alles, und hält die Familie informiert.

Das Wort "AI" erscheint nirgendwo im Produkt.

---

## Warum jetzt

GPT-Realtime-2 (released 8. Mai 2026) bringt GPT-5-class Reasoning direkt in Telefongespräche -- mit 128K Kontext, Tool Use während des Gesprächs, natürlicher Unterbrechungsverarbeitung, und SIP-Support für echte Telefonnummern. Die Sprachqualität ist erstmals gut genug dass ältere Menschen keinen Unterschied zu einem echten Gespräch wahrnehmen.

Demographisch: 22 Millionen Menschen über 65 in DACH. ~40% mit limitiertem täglichem Sozialkontakt. Wachsend. Kein Produkt in dieser Kategorie erfordert heute null Technikkompetenz vom Nutzer.

---

## Die zwei Personas -- und warum das wichtig ist

### Persona A: Der aktive Ruheständler (62-74)
Reist, geht ins Theater, hat Termine, kauft online. Versteht Technologie grundsätzlich aber hat keine Lust auf weitere Apps. Kauft Klara für sich selbst weil es einfacher ist als alles andere. Will: Zugverbindungen, Restaurantreservierungen, Pakettracking, Kulturprogramm, praktische Informationen.

Kaufmotiv: Convenience und Autonomie.
Akquisitionskanal: Schwieriger digital, eher Mund-zu-Mund, ÖAMTC-Membership-Newsletter, Seniorenorganisationen, Arztpraxen.

### Persona B: Der schwerer erreichbare Elternteil (72-85)
Lebt allein oder mit limitiertem Kontakt. Die Familie macht sich Sorgen. Kein Interesse an Technologie. Kauft Klara nicht selbst -- die Kinder kaufen es. Will: Begleitung, Sicherheit, Familienverbindung.

Kaufmotiv: Familie will Beruhigung und Verbindung.
Akquisitionskanal: Facebook/Instagram targeting 35-55 mit Eltern. Trigger-Events: Weihnachten, Muttertag, Beunruhigungsmomente (verpasster Anruf, Betrugsversuch, Arzttermin-Verwirrung).

### Was beide verbindet
Der Positioning-Satz der beide einschließt ohne zu stigmatisieren:

**"Klara ist für Menschen die lieber reden als tippen."**

Kein Alters-Statement. Kein "elderly" oder "Pensionist." Ein Interface-Statement das niemanden ausschließt und niemanden beleidigt.

---

## Features nach Phasen

### Phase 1 (Monat 1-3) -- Der Kern
Alles was nur GPT-Realtime-2 + Twilio + Supabase + Celery braucht. Keine externen APIs.

- Täglicher Outbound-Anruf zur gewählten Zeit
- Vollständiges Gesprächsgedächtnis (pgvector, semantische Suche über alle vergangenen Calls)
- WhatsApp-Zusammenfassung an Familie nach jedem Anruf
- Missed-Call-Alert wenn 3 Tage nicht abgehoben wird
- Medikamenten-Reminder (als Gesprächselement oder separater kurzer Call)
- Einfaches Familien-Onboarding-Portal (Next.js, ein Formular)
- Stripe Subscription

### Phase 2 (Monat 3-6) -- Nützlichkeit
Wenige neue APIs, hoher Mehrwert.

- ÖBB Scotty API (kostenlos): Zugverbindungen, Abfahrtszeiten
- Uber API: Fahrt bestellen mit expliziter Confirmation
- Wetter + Kleidungsempfehlung (OpenMeteo, kostenlos)
- Pakettracking (Austrian Post / DHL API)
- Arzttermin-Begleitung (pre/post, kein API -- nur Gesprächskontext)
- Betrugsschutz-Hotline (zweite Twilio-Nummer, gleicher Backend)
- Enkelbriefservice (Zusammenfassung als WhatsApp an Familie)

### Phase 3 (Monat 6-18) -- Skalierung
Aufwändigere Features und B2B.

- Kinoprogramm / TV-Programm / Kulturkalender (günstige APIs)
- Apothekennotdienst-Suche (Google Places API)
- Retell Outbound Calls zu Arztpraxen und Restaurants für Terminvereinbarung
- Lebensgeschichte-Archiv (strukturiertes Langzeit-Gedächtnis)
- Familien-Dashboard mit Wellbeing-Trends
- B2B: Seniorenresidenzen, Pflegeheime, Hausärzte als Vertriebskanal

---

## Externe Task-Execution -- die wichtige Grenze

Klara macht: informieren, reservieren (via Anruf), erinnern, verbinden, koordinieren.
Klara macht nicht: finanziell bindende Transaktionen ohne explizite Bestätigung, Web-Automation, Formulare ausfüllen, medizinische Diagnosen.

Die Grenze ist: wenn etwas eine Kreditkarte oder eine Unterschrift braucht, gibt es immer einen Confirmation-Step. "Soll ich das Uber wirklich bestellen? Es kostet ungefähr €12." Keine autonome Ausführung. Das eliminiert das Haftungsproblem aus dem AI Concierge-Konzept.

Outbound Calls durch Klara (via Twilio + GPT-Realtime-2):
- Restaurantreservierungen: kein Finanzrisiko, Klara ruft einfach an
- Arzttermine vereinbaren: high-value, Klara übernimmt die Warteschleife
- Apothekenbestellung: Klara ruft an, bestellt, User holt ab oder lässt liefern
- ÖAMTC Pannenhilfe: Klara ruft an mit Mitgliedsnummer und Standort

Das ist der echte Vorteil gegenüber App-basierten Lösungen: Klara kann telefonieren.

---

## Tech Stack

| Layer | Tool |
|---|---|
| Telefonie | Twilio (Media Streams) |
| Voice AI | GPT-Realtime-2 direkt via Twilio Media Streams |
| LLM | GPT-Realtime-2 nativ (kein separates LLM nötig) |
| Backend | Python FastAPI |
| Datenbank | Supabase (Postgres + pgvector) |
| Memory | pgvector semantische Suche |
| Scheduler | Celery + Redis |
| Outbound Calls | Twilio + GPT-Realtime-2 (offizielles Twilio-Tutorial vorhanden) |
| Familie-Alerts | Twilio WhatsApp API |
| Onboarding | Next.js (minimal) |
| Zahlung | Stripe |
| Deployment | Railway |

**Kein Retell.** Twilio hat offizielle Python-Tutorials und GitHub-Repos für die direkte Integration mit GPT-Realtime-2 veröffentlicht -- inklusive Outbound Calling. Die Audio-Codec-Konvertierung (PCMU → PCM16) ist bereits von Twilio gelöst und dokumentiert.

Startpunkt: `git clone https://github.com/twilio-samples/speech-assistant-openai-realtime-api-python`

Relevante Twilio-Ressourcen:
- Build an AI Voice Assistant with Twilio Voice, the OpenAI Realtime API, and Python
- Outbound Calling with Twilio Voice, the OpenAI Realtime API, and Python
- https://www.twilio.com/en-us/blog/developers/twilio-openai-realtime-api-resources

---

## Unit Economics

GPT-Realtime-2 direkt via Twilio Media Streams von Tag 1 -- kein Retell-Aufschlag.

| Posten | Kosten/User/Monat |
|---|---|
| GPT-Realtime-2 (~5 min/Tag, 30 Tage, low effort) | ~€5 |
| Twilio Outbound Voice | ~€2 |
| Twilio WhatsApp | ~€1.50 |
| Supabase + Infrastructure | ~€1 |
| **Total COGS** | **~€9-10** |

Bei €39 Revenue: ~74-77% Bruttomarge.
Bei €24 Revenue: ~58-63% Bruttomarge -- von Anfang an viable.

Break-even (2-Personen-Team, €6k Fixkosten/Monat, kein Gehalt): ~25 zahlende User bei €39/Monat.

---

## Pricing

**Launch: Zwei Tiere, €24 und €39/Monat.**

Beide sind von Tag 1 wirtschaftlich viable weil GPT-Realtime-2 direkt genutzt wird.

- Klara Companion (€24/Monat): tägl. Anruf, Familienverbindung, Memory, Missed-Call-Alert, Medikamenten-Reminder
- Klara Aktiv (€39/Monat): alles aus Companion + ÖBB, Uber, Pakettracking, Arztbegleitung, Betrugsschutz-Hotline

B2B (Phase 3): €15-20/User/Monat bei Seniorenresidenzen, Mindestabnahme 20 User.

---

## GTM -- Konkret

### Erste 10 User (die wirklich zählen)

Nicht abstrakt "warmes Netzwerk." Konkret:

Eigene Familie und engster Bekanntenkreis: gibt es Großeltern, ältere Verwandte, Eltern von Freunden die in Frage kommen? Das ist der erste ehrliche Test.

WU-Professoren und ältere Alumni: bildungsnah, technologieoffen aber nicht technikaffin, einkommenssstark.

Hausärzte als Multiplikatoren: ein einziger Landarzt der Klara an 5 Patienten empfiehlt ist wertvoller als 100 Facebook-Ads. Persönliches Gespräch, kostenlose Testmonate anbieten.

Lokale Seniorenorganisationen in Wien: Wiener Seniorenring, Pensionistenverbände. Präsentation anbieten, 10 Freimonate für Pilotgruppe.

### Skalierung

Facebook/Instagram Ads targeting 38-55 mit Eltern in AT/DE: emotionaler Creative ("Wann hast du zuletzt eine Stunde mit deiner Mutter gesprochen?"), Landing Page, einfacher Signup.

Muttertag, Weihnachten, Valentinstag als Peak-Buying-Momente.

PR: eine Story in einem Österreich-Medium (Format, Trend, Der Standard) über das Thema Einsamkeit im Alter mit Klara als Lösung erreicht die Zielgruppe der kaufenden Kinder.

---

## Ethisches Fundament -- nicht optional

Das ist die sensibelste Produktkategorie in Consumer AI. Tägliche Gespräche mit vulnerablen Menschen über Gesundheit, Geld, Familie -- das sind die schützungswürdigsten Daten die es gibt.

**Zwingend vor Launch:**

Informed Consent der älteren Person selbst. Die Familie kauft das Abo, aber beim ersten Anruf erklärt Klara klar: "Ich bin ein digitaler Assistent, kein Mensch. Deine Tochter hat mich gebeten, mich regelmäßig zu melden. Bist du damit einverstanden?" Ohne explizites Ja geht es nicht weiter.

Transparenz über Datenspeicherung. Was gespeichert wird, wie lang, wer Zugriff hat. Im Onboarding-Portal für die Familie sichtbar, in einfacher Sprache für die ältere Person im ersten Call erklärt.

Familie kann nicht heimlich überwachen. Die ältere Person weiß immer dass Zusammenfassungen an die Familie gehen. Wenn sie das nicht will, gibt es einen Opt-out ("Ich möchte nicht dass alles weitergegeben wird").

Krisenprotokoll -- muss vor Launch designed sein, nicht danach:
- Leichte Besorgnis: in Memory noted, in WhatsApp-Zusammenfassung erwähnt
- Moderate Besorgnis (Verwirrung, Wiederholung, Traurigkeit): sofortiger Alert an Familie
- Schwere Krise (Suizidgedanken, medizinischer Notfall-Sprache): Klara fragt direkt "Soll ich die Rettung rufen?", sofortiger Alert, Krisenressourcen genannt
- Kognitiver Abbau über Zeit: Trend sichtbar im Dashboard, Familie sensibel informiert

Klara ersetzt keine menschliche Verbindung. Das muss im Produkt verankert sein: bei starker emotionaler Abhängigkeit aktiv ermutigen echte Gespräche zu suchen, nie in die Rolle des einzigen sozialen Kontakts schlüpfen.

**GDPR:**
Gespräche mit älteren Menschen über Gesundheit = spezielle Datenkategorie (Art. 9 DSGVO). Erfordert explizite Einwilligung, Datenschutzkonzept, Auftragsverarbeitungsvertrag, Löschroutinen. Vor Launch Anwalt hinzuziehen. Kein optionaler Schritt.

---

## Was das Produkt wirklich verteidigt

Memory-Lock-in: Klara kennt nach 90 Tagen die Namen der Nachbarn, die alte Hüftverletzung, den Lieblingstee. Kündigen bedeutet alles zu verlieren. Das ist der stärkste Retention-Mechanismus in Consumer Software.

Vertrauen durch Familienintroduktion: Klara wird eingeführt von einer Vertrauensperson. Dieser Trust-Transfer ist für Konkurrenten fast nicht replizierbar.

Lebensarchiv als Moat: Rezepte, Lebensgeschichten, Familienbriefe die sich über Monate ansammeln werden zu etwas Irreplaceable.

Phone-first als Zugangsbarriere: solange andere Produkte Apps erfordern, ist das Feld für Klara frei.

---

## Wettbewerb

| Produkt | Interface | Memory | Familie-Integration | Task-Execution |
|---|---|---|---|---|
| Klara | Telefon | ✓ vollständig | ✓ WhatsApp + Dashboard | ✓ (reservieren, informieren) |
| GrandPad | Tablet | ✗ | Teilweise | ✗ |
| Lilli / Rejoice | Smartphone-App | Teilweise | Teilweise | ✗ |
| Replika | Smartphone-App | ✓ | ✗ | ✗ |
| Medical Alert Devices | Hardware | ✗ | ✗ | ✗ |
| AI Concierge (HNWI) | Telefon + WhatsApp | ✓ | ✗ | ✓ (buchen, bezahlen) |

Klara ist die einzige Lösung die: (1) kein Gerät oder Download erfordert, (2) echtes Gedächtnis hat, (3) Familie aktiv einbindet, und (4) externe Tasks ausführen kann.

---

## Verhältnis zu AI Concierge

Klara und AI Concierge teilen Teile des Tech-Stacks (Twilio, GPT-Realtime-2, Supabase) aber sind fundamental verschiedene Produkte.

| Dimension | AI Concierge | Klara |
|---|---|---|
| Zielgruppe | HNWIs 35-55 | Aktive/ältere Erwachsene 62-85 |
| Käufer | User selbst | Oft Familie |
| Core Value | Convenience, Zeitersparnis | Begleitung, Verbindung, Sicherheit |
| Task Execution | Buchen, bezahlen, handeln | Informieren, reservieren, verbinden |
| Haftungsrisiko | Hoch | Niedrig |
| CAC | €300-800 | €30-100 |
| Business Killer | DISH-API, Stellvertretung, Infinite Intent | Krisenprotokoll, GDPR, Adoption |

Klara ist das einfachere, ethisch weniger riskante, und für einen Erstgründer besser geeignete Produkt.

---

## Szenarien

### Szenario A: Side Project / Lernprojekt

Bau Klara solo mit Claude Code als technischem Multiplier. Ziel: 20-30 zahlende User, €1-2k MRR, Verständnis des Markts. Kein Fundraising, kein Vollzeit-Einsatz. Gibt dir echte Gründererfahrung und einen bewiesenen Track Record für spätere Gespräche.

Zeitaufwand: 10-14 Wochen bei 3-4h täglich.

### Szenario B: Ernstes Startup ab 2027

Mit technischem Co-Founder, nach Studienabschluss. Ziel: 100+ User bis Ende 2027, Pre-Seed-Gespräche Q1 2028. Traction aus Solo-Phase (Szenario A) als Beweis.

Fundraising-Argument: bewiesene Retention, echte User-Testimonilas, klarer B2B-Expansionspfad über Seniorenresidenzen, demographischer Rückenwind.

Realistisches TAM-Argument für VCs: 6.6 Millionen potenzielle Nutzer in DACH, €120M+ ARR bei 5% Penetration und €30 blended ARPU. Expansion via System Prompt-Anpassung für FR, NL, IT.

---

## Offene Fragen

- Solo-Projekt oder Startup-Ambition? (Beeinflusst Priorisierung und Scope)
- Produktname "Klara" -- Domain- und Markenverfügbarkeit prüfen
- Wie genau wird kognitiver Abbau erkannt und kommuniziert? (Muss ethisch und rechtlich sauber designed sein)
- Uber API Developer Access: frühzeitig beantragen, Wartezeit möglich

---

## Build-Reihenfolge

| Woche | Was entsteht | Schwierigkeit |
|---|---|---|
| 1-2 | Klara nimmt Anruf an, kennt User-Namen | Mittel |
| 3 | Gedächtnis über Calls hinweg | Einfach |
| 4 | Klara ruft täglich an (Celery + Railway) | Schwer |
| 5 | WhatsApp-Alerts an Familie | Einfach |
| 6-7 | ÖBB und Uber als Tools | Mittel |
| 8 | Stripe + Signup-Seite | Mittel |

Erstes Ziel: du rufst an, Klara antwortet mit deinem Namen. Das ist der Validierungsmoment.

---

## Related
- [[01-Ideas/AI-Concierge]]
- [[02-Knowledge/Pre-Seed-Fundraising]]
- [[01-Ideas/KI-Kurs-fuer-40plus]]
