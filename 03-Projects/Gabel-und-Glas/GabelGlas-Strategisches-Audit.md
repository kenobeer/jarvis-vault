---
type: research
status: active
tags: [gabel-und-glas, strategie, monetarisierung, newsletter, deep-research]
created: 2026-04-30
---

# Gabel & Glas — Strategisches Audit (Deep Research)

Externe Deep-Research-Analyse zu Konzept, Tech, Wachstum und Monetarisierung.
Gefiltert nach "jetzt relevant" vs. "Phase 2+" — nicht alles ist für den POC anwendbar.

---

## Sofort relevant (POC Wien)

### Progressive Profiling
Wichtigste Erkenntnis. Kein überladenes Signup-Formular. Daten schrittweise sammeln:

| Stufe | Touchpoint | Was erfasst wird | Wie |
|---|---|---|---|
| 1 | Signup | Email + Stadt | Single-Field-Formular |
| 2 | Welcome-Mail | B2B vs. B2C | Ein-Klick-Frage: "Liest du beruflich oder aus Genuss?" → setzt Beehiiv-Tag automatisch |
| 3 | Newsletter-Nutzung | Spezifische Interessen | Implizites Klick-Tracking auf Rubriken |
| 4 | Monetization | Firmenname, Position | Erst bei Gated Content mit echtem Gegenwert |

Konkrete Umsetzung: "Beruflich oder Genuss"-Frage in der Welcome-Mail als klickbarer Button. Beehiiv setzt automatisch Tag `audience_b2b` oder `audience_b2c`.

### Referral-Mechanik
- Keine physischen Prämien (zieht Bots und minderwertigen Traffic an)
- Rein digitale, statusgetriebene Rewards
- Meilenstein 1 (3 Empfehlungen): geheime Ausgabe oder Insider-Liste ("Wo Wiens Sterneköche nach Schicht trinken")
- Meilenstein 2 (10 Empfehlungen): Early Access zu Tischreservierungen oder Vorab-Einladungen
- "Confirmed Referral Rule": gilt erst nach DOI + mindestens einem Klick in einer Ausgabe

### DMARC / SPF / DKIM
Muss vor dem ersten Send eingerichtet sein. DMARC initial mit `p=none`, später auf reject. Kritisch für Zustellbarkeit bei B2B-Empfängern.

### Sunset Policy
60-90 Tage ohne Öffnung oder Klick → Re-Engagement-Segment → bei Inaktivität aus Liste entfernen. Schützt IP-Reputation.

---

## Phase 2+ (nach 1.000-2.000 Subscribern)

### Job Board — Service statt Produkt
Kernlektion aus Morningcrunch: rigide Job-Board-Produkte scheitern in der Gastronomie weil die Branche nicht so funktioniert. Betreiber haben keine Zeit, kein Freigabeprozess, und meistens Last-Minute-Bedarf.

**Richtiger Ansatz: eine Zeile Text im Newsletter, buchbar per WhatsApp.**
- Kein Formular, kein Portal, kein Preisrechner
- Kontakt über WhatsApp oder direkte Email an Lutzi
- Preis verhandelbar, Format flexibel, Deadline auch
- Betreiber schickt zwei Sätze, Lutzi baut es ein
- Einstiegspreis: 50-80€ pro Listing — keine Entscheidung für den Gastro-Betreiber

Stufenplan:
- Phase 1 (jetzt): manueller Textblock im Newsletter, kostenlos, Lutzi kuratiert
- Phase 2 (ab ~2.000 Subscriber): erste bezahlte Listings per WhatsApp/Email
- Phase 3 (ab ~5.000 Subscriber): Tally-Formular, /jobs Seite, strukturierte Preisliste

Quellen für Listings:
- Direkt nach Features: jedes vorgestellte Lokal bekommt eine Nachricht von Lutzi (kennt die Leute, kein Kaltakquise)
- Lutzis Netzwerk: weiß wer Personal sucht bevor es öffentlich wird
- gastrojobs.at und hogastjob.com für Wien (kostenlos einbauen bis bezahlte Anfragen kommen)
- Ab ~1.000-1.500 Subscribern kommen Anfragen passiv wenn jobs@gabelundglas.com im Footer steht

Kommerzialisierbar ab ca. 2.000-3.000 Subscribern wenn die Leserschaft erkennbar "Insider" ist.

### Utility Paid Tier ("Gabel & Glas Insider")
5-9€/Monat. Keine Paywall auf den Newsletter selbst — schützt Reichweite für Sponsoren.
Mögliche Vorteile: Web-Archiv mit filterbarer Karte aller besprochener Lokale, Early-Bird für Events, Vergünstigungen bei Feinkost-Partnern.

### B2B-Lead-Generation
Gated Content (z.B. "5 beste Tools gegen Personalmangel Wien") als Whitepaper. B2B Food-Tech zahlt 50-150€ pro qualifiziertem Lead. Erst sinnvoll wenn Segmentierung via Beehiiv-Tags aktiv läuft.

### Dynamic Content Blocks (Beehiiv Scale/Max)
B2C: soziokultureller Deep Dive. B2B: branchenpolitischer Content — nur für verifizierte B2B-Tags. Erfordert Scale- oder Max-Plan. Für POC nicht notwendig.

---

## Was wir nicht übernehmen

**Dual-Audience-Split als sofortiges Problem:** Lutzis Stimme ist B2C. B2B-Pivot jetzt würde die Stimme verwässern bevor es 500 Subscriber gibt.

**Revenue-Projektionen wörtlich:** 45€ Blended CPM setzt funktionierende Werbe-Partnerships voraus. Phase 1 sind Tauschdeals und symbolische Sponsorings.

**Beehiiv Scale/Max sofort:** Free oder Launch Tier reicht für die ersten 1.000 Subscriber.

---

## Referenz-Benchmark: The Infatuation
2009 als reines B2C-Empfehlungsmedium gestartet. B2B-Content nie mit B2C vermischt. Konsumentendaten als B2B-Asset genutzt, Zagat von Google akquiriert. Monetarisierung über Partnerships im Backend. Lektion: Reichweite zuerst, B2B-Monetarisierung kommt aus der Datenbasis.

---

*Linked to:* [[03-Projects/Gabel-und-Glas/Gabel-und-Glas]] · [[03-Projects/Gabel-und-Glas/Website/website-overview]]
