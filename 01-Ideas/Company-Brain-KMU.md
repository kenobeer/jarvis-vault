---
type: idea
status: watching
tags:
  - ai
  - kmu
  - b2b
  - saas
  - wissensmanagement
  - dach
created: '2026-05-26'
---
# Company Brain für KMUs

Ursprungsimpuls: YC Request for Startups (Tom Blomfield) — "Company Brain" als fehlende Schicht zwischen Rohdaten und zuverlässiger KI-Automatisierung. Die These: Modelle sind gut genug, der Flaschenhals ist jetzt das Domänenwissen.

---

## Konzept in einem Satz

Ein lebendes, maschinenlesbares Wissensarchiv, das das implizite Betriebswissen eines KMU aus E-Mails, Chats, Dokumenten und Köpfen extrahiert, strukturiert und aktuell hält, damit KI-Agenten auf dieser Grundlage zuverlässig arbeiten können.

---

## Bewertung

### Stärken

- **Echtes, akutes Problem.** Key-Person-Risk ist in jedem KMU real. Wenn die Person, die alles weiß, geht, bricht viel zusammen. Das ist kein hypothetischer Schmerzpunkt, sondern etwas, das Inhaber kennen und fürchten.
- **Timing ist gut.** LLMs sind erst seit 2023-24 gut genug für diesen Use Case. Der Markt ist noch früh, aber die technische Machbarkeit ist gegeben.
- **DACH-spezifische Marktlücke.** Enterprise-Tools (Notion AI, Atlassian Intelligence, Microsoft Copilot) zielen nicht auf KMUs. Sprachliche und regulatorische Eigenheiten (DATEV, BMD, österreichisches/deutsches Recht) machen DACH zu einem natürlichen Schutzwall gegen US-Konkurrenz.
- **Hohe Switching Costs.** Sobald das System läuft und das Unternehmen davon abhängt, ist der Wechsel extrem schmerzhaft. Retention-Potenzial ist stark.
- **Bruttomargen attraktiv.** LLM-Kosten bei ca. 30-80 EUR/Monat pro Kunde bei realistischer Nutzung. Bruttomargen von 70-80% erreichbar.
- **Natürlicher Land-and-Expand.** Einstieg mit einer Wissensart (z.B. Kundenausnahmen), dann Ausweitung auf gesamtes Betriebswissen.

### Risiken und Schwächen

- **Sales Motion ist brutal.** KMU-Inhaber kaufen keine Software über Cold Email. Die Akquise läuft über Vertrauen, Empfehlungen und Intermediäre (IT-Dienstleister, DATEV-Berater). Langer Sales Cycle, hohe Cost of Acquisition.
- **Technische Komplexität unterschätzt.** Die Extraktion ist nicht ein Schritt, sondern das gesamte Produkt. Konfliktauflösung, Confidence-Scoring, Dialect-Transkription (österreichisches Deutsch in WhatsApp-Sprachnachrichten) sind echte Ingenieurprobleme.
- **Vertrauen und Datenschutz.** KMU-Inhaber sind skeptisch, alle Unternehmensdaten in eine Cloud-Plattform zu laden. DSGVO-Konformität ist Pflicht, nicht Option. On-Premise-Option für manche Segmente notwendig.
- **Integration Hell.** DATEV und BMD haben beschränkte APIs. Echter ERP-Anschluss ist ein 6-Monats-Projekt pro System.
- **Support-Burden.** KMU-Kunden sind nicht self-serve. Die ersten 20 Kunden kosten je 10x mehr Zeit als der Preis impliziert.
- **Brain Staleness.** Wenn das System nicht sauber aktuell gehalten wird, liefert es falsche Prozesse. Dann ist es aktiv schädlich. Der Feedback-Loop ist technisch anspruchsvoll und entscheidend.
- **Wettbewerb durch Plattformen.** Microsoft Copilot für M365 könnte das Problem durch schiere Marktmacht absorbieren, sobald KMUs breiter auf M365 umsteigen.

### Marktgröße (DACH)

- Österreich: ca. 400.000 KMUs (WKO)
- DACH gesamt: ca. 3,5 Millionen KMUs
- Realistisches Preisband: 200-800 EUR/Monat flat
- Bei 0,1% Penetration in DACH und 400 EUR Durchschnitt: ca. 1,4 Mio. ARR erreichbar in 5 Jahren für ein frühes Team

### Wettbewerbslandschaft

| Anbieter | Zielgruppe | Schwäche für KMU |
|---|---|---|
| Microsoft Copilot for Business | SMB mit M365 | Teuer, oberflächlich, kein Wissens-Graph |
| Notion AI / Confluence | Tech-affine Teams | Englisch, kein Connector zu KMU-Tools |
| Guru, Tettra, Slite | US-SaaS-Teams | Kein DACH-Fokus, Wiki-Logik statt Extraktion |
| Generische RAG-Tools | Entwickler | Kein KMU-Onboarding, kein Feedback-Loop |

Niemand macht: KMU + DACH + Extraktion aus E-Mail/WhatsApp + Deutsch + Steuerberater/Handwerk/Logistik.

---

## Technischer Kern (Kurzfassung)

**Ingestion:** Gmail/Outlook (OAuth), WhatsApp Business API, Google Drive/Dropbox, ERP-Exporte (CSV als Einstieg)

**Extraktion:** Semantic Chunking, Entity Extraction, Klassifikation in Wissenstypen (Prozess / Entscheidung / Ausnahme / Beziehung), Confidence-Scoring, Konfliktdetektion

**Speicherung:** Postgres + pgvector als Start, Graph-Schema mit Nodes (Entity, Process, Decision, Exception, Policy, Role) und typisierten Kanten

**Output:** Maschinenlesbares "Skills File" pro Prozess, inkl. Ausnahmen und Eskalationspfaden, konsumierbar durch KI-Agenten

**Feedback-Loop:** Passive Signale (Agent-Erfolg/-Fehler, Deviation Detection), aktive Signale (wöchentliche Review-Digests per Mail), kontinuierliches Re-Ingestion aus E-Mail-Stream

---

## GTM-Logik

**Wedge-Produkt:** "Exit Risk Audit" als Einmalleistung (500-1.500 EUR). 48h Lesezugriff auf E-Mail und Drive, Ergebnis: Bericht über Wissen-At-Risk, 3 extrahierte Musterprozesse. Danach Übergang in Abo.

**Vertikaler Fokus:** Steuerberatungskanzleien als erstes Segment. Prozessgetrieben, DATEV-affin, hohe Key-Person-Abhängigkeit, Softwareabo-gewohnt.

**Sales-Kanäle:** DATEV-Berater und IT-Dienstleister als Partner (Revenue Share), WKO-Veranstaltungen, 1 starke Fallstudie + Mundpropaganda.

**Preismodell:** Flat per Unternehmen, nicht per Seat. Onboarding-Fee 1.500-3.000 EUR einmalig. Monatlich: bis 10 MA 199 EUR, 10-50 MA 399 EUR, 50-200 MA 799 EUR.

---

## Persönliche Einschätzung

Starke Idee. Kombination aus echtem Schmerzpunkt, unterversorgtem Markt, technischem Moat und strukturell hohen Switching Costs. Die Gefahr liegt weniger im Wettbewerb als im Execution-Risiko: Sales an KMUs ist zermürbend, der Feedback-Loop ist schwer zu bauen, und ein schlechtes Brain ist schlimmer als keins.

Der Alpha liegt im vertikalen Einstieg. Wer versucht, horizontal von Tag 1 zu skalieren, verbrennt sich. Steuerberater in DACH, eine Wissensart, perfektioniert, dann expandieren.

Für mich aktuell nicht der richtige Moment (Gabel & Glas, Grenoble). Aber ein Thema, das ich verfolge. Könnte interessant werden als VC-Anlagethese oder als Gründungsoption ab 2028.

---

## Offene Fragen

- Wie löst man das Vertrauensproblem bei konservativem KMU-Inhaber konkret? On-Premise-Agent + lokale Verarbeitung?
- Welche Extraktionsqualität ist nötig, damit das System mehr Wert als Schaden anrichtet?
- Gibt es bereits gut-finanzierte Player in DACH, die ich nicht kenne?
- Wie integriert man Austrian-dialect WhatsApp-Voice-Messages sauber?
- Lässt sich der Audit als Standalone-Produkt profitabel betreiben, oder ist er immer Loss Leader?

---

**Linked to:** [[Ideen-Sammlung]] · [[AI-Incubator]]
