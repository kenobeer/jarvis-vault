---
type: project
status: active
tags:
  - gabel-und-glas
  - newsletter
  - business
created: '2026-04-10'
updated: '2026-04-30'
---
# Gabel & Glas

## Decisions & facts

### Brand
- Newsletter: Gabel & Glas (gabelundglas.com)
- Positionierung: zwischen Food-Influencer (zu seicht) und Falstaff/Gault&Millau (zu verstaubt)
- Zielgruppe: 25–50, genussorientiert, kein Fachpublikum aber auch nicht casual
- Expansion: Wien (Proof of Concept) → München (Warteliste) → Zürich (Locked)
- Zusätzlich: DACH-weites Recipe Briefing parallel zu Wien-Briefing

### Wien Editor
- Name: Luca „Lutzi" Presser
- Profil: Junggastronom, Anfang 20, Insider der Wiener Szene
- Mitgründer von „das Doppler" (Heinestraße 30, 1020 Wien, mit Moritz Blank)
- Früherer Hintergrund: Loup Garou, dasDrittl
- Tritt explizit als „Wien Editor von Gabel & Glas" auf

### Briefing-Struktur (fix)
1. Intro (Hook, 60–80 Wörter)
2. Deep Dive (250–350 Wörter, mit konkretem Learning am Ende)
3. Stadtgeflüster (mehrere Items, je 2–4 Sätze)
4. Gabel (Essen-Tipp, 60–120 Wörter)
5. Glas (Drink-Tipp, 60–120 Wörter)
6. Save the Date (inkl. ggf. Cross-Promo Recipe Briefing)
7. Outro (kurz, selbstironisch, mit CTA)
- Gesamt: ~900–1.100 Wörter

### Lutzis Tonfall (kodifiziert)
- Wienerischer Charakter: „ich bitt' euch", „passt schon", „na sicher", „ehrlich gsagt", „ka Ahnung warum"
- Humor: 2–3 echte Witze/Schmäh pro Briefing, eingebaut als Haltung — nicht als Punchline am Ende
- Kein Tech-Slang, keine Boomer-Phrasen, keine Unternehmensberater-Sprache
- Mehr Bar-Gespräch, weniger Journalismus
- Zu vermeiden: „Weichspülversion", „kein Bug sondern Feature", „macht wach"

### Tech-Stack
- Newsletter-Plattform: Beehiiv (Free/Launch Tier für die ersten 1.000 Subscriber)
- Kurzlinks & QR-Codes: Dub.co (dynamisch, UTM-fähig)
- Analytics: Plausible
- Website: Next.js (App Router) auf Vercel, IP-Geolocation via Vercel Geo Headers
- Build-Tool: Google Antigravity

### Website-Entscheidungen
- Keine eigene Backend-Infrastruktur — alles über Vercel Serverless Functions
- IP-Geolocation statt Browser-Permission-Prompt oder Städte-Selector
- Kein Städte-Selector above the fold — Conversion-Entscheidung, Stadt nur im vorausgefüllten Formular
- G&G als Medienmarke framen, nicht als Wien-Format
- Recipe Briefing: pre-checked Checkbox (opt-out)
- Soft Wall für Ausgaben-Archiv: ~150 Wörter frei, dann Blur + Email-CTA
- Letzte Ausgabe immer 100% frei
- Subdomain: newsletter.gabelundglas.com für Beehiiv

### Sticker-Konzept
- Stufe 1: „Von Lutzi empfohlen" — für jedes gefeatured Lokal
- Stufe 2: „Gabel & Glas Favorites" — ab ~2.000–3.000 Subscribern
- Jeder Sticker hat dynamischen QR-Code → dedizierte Lokal-Landing-Page → Beehiiv Signup
- Beehiiv Tags: `qr_sticker` + `lokal_[name]` für Tracking

### Wachstumsstrategie (laut Paul Ostwald)
1. Wien richtig ans Laufen bringen
2. Guerilla-Marketing: Sticker + QR in guten Restaurants
3. EIN Presseartikel über G&G (Social Proof für Investoren)
4. 3–4 Wiener Angels ansprechen
5. Wachstum über Food-Influencer, Restaurant-Instas, Barter-Deals

### Job Board-Strategie
- WhatsApp-basierter Service, kein rigides Produkt (Lektion von Morningcrunch)
- Phase 1: manueller Textblock, kostenlos, Lutzi kuratiert
- Phase 2 (ab ~2.000 Subscriber): erste bezahlte Listings per WhatsApp/Email, 50–80€
- Phase 3 (ab ~5.000 Subscriber): Tally-Formular, /jobs Seite, strukturierte Preisliste

### Strategische Referenzen
- The Infatuation: Voice-first, Community-Events, Brand Partnerships
- Eater: Städte-System, aber zu schnell skaliert (Fehler vermeiden)
- Vittles: Newsletter-only, paid tier, langsam & nachhaltig
- Morning Crunch: Paul Ostwald als Advisor kontaktiert

---

## Log

### 2026-04-10 — Kickoff
**Was passierte:** Erste große G&G Session. Briefing-Stil entwickelt, Claude Project Instructions geschrieben, Business-Strategie ausgearbeitet, Tech-Stack definiert, Sticker-Konzept entwickelt, Advisor-Mail an Paul Ostwald geschrieben.
**Decisions:**
- Briefing-Struktur fixiert (7 Rubriken)
- Tech-Stack entschieden: Beehiiv + Dub.co + Plausible
- Strategische Referenzen definiert
**Open threads:**
- Website noch in Arbeit
- Paid Tier noch nicht definiert
- Paul Ostwald Antwort ausstehend

### 2026-04-30 — Website & Tech Stack
**Was passierte:** Vollständige Website-Architektur entwickelt, PRD finalisiert, Tech Stack bestätigt, Workflow mit Google Antigravity als Build-Tool entschieden.
**Decisions:**
- Kein separates Backend, alles Vercel Serverless
- Antigravity als Build-Tool
- Detailiertes PRD gespeichert unter [[03-Projects/Gabel-und-Glas/Website/website-prd]]
**Open threads:**
- Bowl Font Lizenz klären
- Foto von Lutzi organisieren
- Impressum-Daten zusammenstellen
- Plausible Account anlegen

### 2026-04-30 — Paul Ostwald Feedback
**Was passierte:** Paul Ostwald hat Feedback gegeben (saustark). PRD vollständig ausgearbeitet. Alle offenen Entscheidungen aus dem PRD besprochen.
**Decisions:**
- Job Board als WhatsApp-Service, nicht Produkt
- „Ingredient der Woche" als potenzielle Phase-2-Rubrik
- Daten-Abfrage: „Beruflich oder Genuss" in Welcome-Mail reicht als Start
**Open threads:**
- Telefonat mit Paul Ostwald noch vereinbaren
- Headline und CTA Wortlaut finalisieren (Optionen in [[03-Projects/Gabel-und-Glas/Website/website-copy]])
- Presseartikel-Strategie (vor Angel-Gesprächen)

**Linked to:** [[03-Projects/Gabel-und-Glas/GabelGlas-Strategisches-Audit]] · [[03-Projects/Gabel-und-Glas/Paul-Ostwald-Feedback]] · [[05-People/Paul-Ostwald]]

### 2026-06-28 — Marketing-Strategie Phase 0 + Rechtsfrage Reservierungsliste
**Was passierte:** Start der Marketing-Strategie-Entwicklung (Schwerpunkt Social). Phase-0-Framing erstellt: Wissen vs. Annahmen getrennt, 4 Research-Kernfragen definiert. Rechtsfrage zur Zenchef-Reservierungsliste recherchiert und geklärt.
**Decisions:**
- Reservierungsliste (1–3k) ist im Default NICHT für G&G nutzbar (§174 TKG, Dritt-Werbung + Zweckbindung). Strategie baut auf In-Restaurant-Opt-in + Going-forward-Erfassung + organisch.
- Strategie in zwei Layer: Strategie (an Meilensteine gekoppelt) + Execution (mit Designerin)
- Neuer `Marketing/`-Unterordner für die Strategiearbeit
**Open threads:**
- Deep-Research-Prompts bauen (Phase 1)
- Rechtsfrage vor erstem Send einem Datenschutzjuristen vorlegen
- Zenchef-Opt-in-Setup prüfen (hat es je ein Marketing-Opt-in erfasst?)

**Linked to:** [[03-Projects/Gabel-und-Glas/Marketing/Marketing-Strategie-Phase0]] · [[03-Projects/Gabel-und-Glas/Marketing/Recht-Reservierungsliste-Zenchef]]
