---
type: idea
status: archived
tags:
  - travel
  - startup
  - b2c
  - fintech
  - marketplace
created: '2026-06-03'
---
# Gruppenreise-App ("Commitment-Maschine")

## Kern-Idee

Eine Plattform, die das organisatorische Koordinationsproblem bei Gruppenreisen loest. Nicht Inspiration (Instagram macht das), nicht reine Buchung (Booking.com macht das), sondern die kritische Luecke dazwischen: die soziale Aushandlung von Terminen, Zielen, Budgets und verbindlichen Zusagen innerhalb einer Freundesgruppe oder WG.

Das zentrale Mechanismus-Konzept: Passivität hat Konsequenzen. Wer nicht abstimmt, wird von der Entscheidung ausgeschlossen. Wer nicht committed, ist draussen. State Machines erzwingen Verbindlichkeit.

## Problem (validiert)

Das Trip-Captain-Dilemma ist real und verhaltensökonmisch gut beschreibbar:
- **Diffusion der Verantwortung:** In einer 6er-Gruppe fühlt sich niemand zu 100% verantwortlich. Bystander-Effekt.
- **Finanzielle Asymmetrie:** Der Trip Captain streckt oft 1.000+ Euro vor, trägt das Risiko von Absagen.
- **Informations-Asymmetrie / Abilene-Paradoxon:** Echte Präferenzen werden nicht kommuniziert, um Konflikte zu vermeiden. Ergebnis: niemand wollte wirklich dahin.
- **Analysis Paralysis:** Zu viele Optionen durch Social Media. Kein Entscheidungsmechanismus.

## Geplantes Geschaeftsmodell (und warum es nicht funktioniert)

**Hidden Markup auf Flüge (10%):** Haupteinnahme sollte ein versteckter Aufschlag auf Flugpreise via Duffel API sein.

Warum es scheitert:
- Gen Z / Millennials vergleichen aggressiv (Google Flights, Skyscanner). Ein 10% Markup auf 200 Euro wird bemerkt und führt zu Abbrüchen (Leakage).
- Unit Economics sind dünn: ~7-9 Euro Netto pro Person, ein einziger Betrugsfall (Chargeback auf gestohlene Kreditkarte) vernichtet den Gewinn von 50+ Buchungen.
- Grundsätzlicher Zielgruppen-Widerspruch: Preissensible Zielgruppe + versteckte Gebühr = kein stabiles Modell.

**Affiliate-Links (Hotel/Aktivitaeten):** Risikofreier, aber margenschwach. Booking.com zahlt 25-40% ihrer eigenen Kommission (~15% des Buchungswerts). Bei 500 Euro Hotelbuchung: ~20-30 Euro für die Plattform.

## Regulatorisches Hauptrisiko: EU-Pauschalreiserichtlinie (RL 2015/2302)

Das ist der strukturelle Killer für das Modell in seiner urspruenglichen Form.

Sobald Flug + Unterkunft gemeinsam angeboten oder in einem Buchungsvorgang kombiniert werden, gilt die Plattform als **Reiseveranstalter** mit:
- Insolvenzabsicherungspflicht (Sicherungsschein, in Deutschland via DRSF oder Bankbüргschaft). Bindet Kapital im 5-6-stelligen Bereich.
- Vollhaftung fuer alle Reiseleistungen (Flug streikt, Hotel überbucht: Plattform muss Ersatz leisten).
- Selbst als Vermittler verbundener Reiseleistungen (LTA) besteht Insolvenzsicherungspflicht für vereinnahmte Gelder.

Der einzige risikoarme Weg ist reines Affiliate/Vermittlungsmodell, das aber das gewünschte Markup-Geschäftsmodell ausschliesst. Das ist das zentrale Dilemma: Modell erfordert Merchant-of-Record-Status, MoR-Status triggert regulatorische Pflichten, die ein frühphasiges Startup kaputt machen.

## Technische Huerden

**Duffel API (Flüge):** Technisch beste Option für Startups ohne IATA-Lizenz. Self-Service-Zugang. Aber:
- Startup ist Merchant of Record, trägt volles Chargeback-Risiko.
- Duffel zahlt mit Verzögerung aus (T+7), Airlines verlangen Sofortzahlung. Liquiditätslücke muss vorfinanziert werden.
- Markup technisch umsetzbar, aber preislich kritisch.

**Booking.com Demand API (V3):** Zugang für neue Startups praktisch nicht möglich. Werden auf Affiliate-Widgets verwiesen, die keine rohen JSON-Daten liefern. Eine native "Tinder-für-Hotels"-UI ist damit nicht baubar ohne Verstoss gegen AGB (Scraping).

**Airbnb:** Keine öffentliche API. Manuelle Link-Eingabe durch User ist der einzige Weg. Kein Systemcheck ob Listing noch verfuegbar oder Preis aktuell ist.

## Wettbewerbs-Analyse

| Unternehmen | Ansatz | Lektion |
|---|---|---|
| **Travefy** | Gestartet 2012 als B2C-Gruppenreiseplanung, 2016 Pivot zu B2B (Reisebüros) | CAC zu hoch bei niedrigem Lifetime Value. Nutzer verwenden Planungstool, buchen extern. |
| **Batch (Bach)** | Vertikal auf Junggesellenabschiede fokussiert, verdient an Erlebnissen (20-30% Marge), nicht Flügen | Vertikalisierung löst Preissensibilitaet. Richtige Zielgruppe: soziales Pflicht-Event, hohe Zahlungsbereitschaft. |
| **Lambus** | DACH-Planungs-App, Freemium + Affiliate | Affiliate allein reicht nicht. Braucht Abo-Einnahmen. Zeigt: auch im DACH-Markt dünne Margen. |

## Strategische Alternativen (nicht verfolgt)

**Pivot: Orga-First, Booking-Lite**
- Phase 1: Nur Abstimmungstools + transparente Service-Fee (5 Euro p.P.). Kein MoR für Flüge, keine Pauschalreiserichtlinie.
- Phase 2: Fintech-Layer. Banking-as-a-Service, virtuelle Gruppenkreditkarten, Interchange-Fee-Einnahmen.
- Phase 3: Vertikalisierung auf Nischen mit hoeherer Zahlungsbereitschaft (Skireisen, Abifahrten, Hochzeiten).

**Concierge-Validierung (Pre-MVP)**
Landingpage + manuelles PDF-Angebot + Stripe Payment Link mit offener Service-Fee. Wenn Leute das zahlen, ist das Problem schmerzhaft genug für ein Produkt.

## Bewertung und Archivierungsbegruendung

Das Problem ist real. Die Lösung (Coordination Layer) ist logisch. Aber:
1. Preissensible Zielgruppe + versteckte Gebühr = kein stabiles Modell.
2. EU-Regulierung macht MoR-Status für Startups ohne Kapital toxisch.
3. Booking.com API-Restriktionen verhindern das zentrale UX-Feature (natives Hotel-Voting).
4. Historisch ist "Social Travel B2C" ein Friedhof: fast alle erfolgreichen Varianten sind entweder nischen-vertikal (Batch) oder B2B gepivoted (Travefy).

Archiviert. Das Problem bleibt ungelöst. Wer es lösen will, muss entweder tief finanziert starten (um regulatorische Hürden zu überbrücken) oder von Anfang an als reines Planungstool ohne Buchungsambitionen positionieren.

## Externes Signal

Juni 2026: Zwei Gründer auf Instagram kündigen eine ähnliche Idee an. Beobachten, aber kein Grund zur Neubewertung der Archivierungsentscheidung. Gleiche strukturelle Probleme werden sie treffen.

---

**Linked to:** [[AI-Concierge]] · [[Vibe-B]]
