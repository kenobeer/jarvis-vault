---
type: memory
status: active
tags: [gabel-und-glas, website, tech-stack, next-js]
created: 2026-04-30
---

# Gabel & Glas — Website & Tech Stack

Permanente Referenz für Architektur, Stack-Entscheidungen und Seitenstruktur.

---

## Tech Stack

### Hosting & Framework
- **Framework:** Next.js (App Router)
- **Hosting:** Vercel — kostenloser Tier reicht für den Start
- **Styling:** Tailwind CSS
- **Deployment:** Git-Push auf GitHub → Vercel deployed automatisch
- **Kein separates Backend** — alles läuft über Vercel Serverless Functions (nativ in Next.js)

### Externe Services

| Service | Zweck | Anmerkung |
|---|---|---|
| Beehiiv | Newsletter-Backend: Subscriber-DB, Sending, DOI, Automations | REST API für Signup & Posts |
| ipapi.co | IP-basierte Geolocation (Stadt) | Kein Browser-Permission-Prompt; läuft serverseitig |
| Dub.co | UTM-Links & QR-Code-Management | Nicht direkt im Code; Link-Tool für Newsletter |
| Plausible | Privacy-first Analytics | Eingebunden als Script-Tag im Next.js Header |

### Domain
- **gabelundglas.com** — eigene Website auf Vercel
- **subscribe.gabelundglas.com** — Subdomain bleibt auf Beehiiv (DOI-Confirmation-Page, Unsubscribe-Links)

---

## Architektur

```
User öffnet gabelundglas.com
        ↓
Vercel Edge Middleware (läuft VOR dem Server)
  → IP-Lookup via ipapi.co → Stadt ermitteln
  → Request-Header mit Stadt-Info weitergeben
        ↓
Next.js App Router
  → Seite rendern mit vorausgefülltem Formular
        ↓
User schickt Signup ab
  → POST an /api/subscribe (Serverless Function auf Vercel)
  → Beehiiv API: Subscriber anlegen + Tags setzen
  → Beehiiv übernimmt DOI-Flow automatisch
```

### Beehiiv API Calls — Frequenz
- **Signup (POST):** 1x pro neuem Subscriber — nur on-demand
- **Ausgaben laden (GET):** gecached via `next: { revalidate: 3600 }` — in der Praxis ca. 20-30 Calls/Tag, nicht pro Visitor
- **Free Plan:** 10.000 API Calls/Monat, wird bei weitem nicht ausgeschöpft

---

## Website-Struktur

```
/ (Landing Page)
  → Hero + Signup-Formular         ← conversion-optimiert, above the fold
  → Lutzi-Sektion + Konzept        ← Vertrauen aufbauen, below the fold
  → Ausgaben-Preview (3 Issues)
  → Städte-Visual
  → Footer

/ausgaben
  → Archiv aller Ausgaben
  → Ältere Issues hinter Soft Wall

/ausgaben/[slug]
  → Letzte Ausgabe: 100% frei
  → Ältere: ~150 Wörter sichtbar, dann Blur + Email-CTA
```

Ausgaben-Daten werden automatisch von der Beehiiv Posts API gezogen — kein manuelles Pflegen nötig.

---

## Design-Entscheidungen

### Kein Städte-Selector above the fold
Kein Dropdown, kein "Coming Soon"-Banner. Stadt wird per IP-Geolocation ermittelt (serverseitig, ohne Browser-Permission). Formular ist vorausgefüllt. Nicht verfügbare Stadt → automatisch Warteliste-Modus. User kann Stadt manuell via dezenten Link wechseln. Neue Stadt hinzufügen = ein Eintrag in Config-Datei.

### Recipe Briefing
Pre-checked Checkbox direkt unter Email-Feld. Default: angemeldet (opt-out statt opt-in).

### Soft Wall
Kein Account, keine Paywall — nur Email. Letzte Ausgabe vollständig frei als Beweis. Ältere Ausgaben mit Blur + Overlay.

### Medienmarke-Framing
Gabel & Glas als übergeordnete Marke — nicht als "Wien-Format". Wien ist der aktuelle Kontext, aber die Marke skaliert. Above the fold: generische Headline, Stadt nur im Formular. Below the fold: Städte-System als Ambitions-Signal.

### Above/Below the Fold
- **Above:** Headline, 2-Zeiler max, Email-Feld, CTA. Kein Fließtext, kein Konzept.
- **Below:** Lutzi, Briefing-Struktur, Städte-Karte, Ausgaben-Preview.

---

## SVG Asset-Konventionen

Format: SVG für alle Brand-Assets (Logo, Icons, Illustrationen) — skaliert verlustfrei, in Next.js als React-Komponenten importierbar.

Naming-Schema:
```
logo-full.svg     — Hauptlogo mit Schriftzug
logo-mark.svg     — Gabel+Glas Icon only
icon-fork.svg
icon-glass.svg
icon-ear.svg      — Stadtgeflüster
```

Vor Antigravity-Übergabe prüfen: viewBox korrekt gesetzt, keine hardcodierten Farben im SVG-Code.

---

## Build-Tool: Google Antigravity

Agent-first IDE (VS Code Fork), kostenlos in Public Preview. Gemini 3 Pro als Primarymodell, Claude Sonnet 4.6 als Alternative. Browser-in-the-Loop: Agent schreibt Code, öffnet Seite im Browser und verifiziert visuell. Kein Usage Limit im Free Tier — effizienter für Design-Iteration als Claude Code.

---

## Offene TODOs

- [ ] SVG-Dateien aufräumen + nach Schema benennen
- [ ] Design-Spec Markdown erstellen (Hex-Codes, Fonts, Abstände, Button-Stil)
- [ ] Copy schreiben: Headline, 2-Zeiler, Lutzi-Bio, CTA-Text, Footer
- [ ] Beehiiv Publication ID + API Key bereithalten (aus Beehiiv Settings)
- [ ] Initialprompt für Antigravity schreiben lassen
- [ ] Subdomain subscribe.gabelundglas.com auf Beehiiv einrichten

---

*Linked to:* [[preserve]] · [[compress]]