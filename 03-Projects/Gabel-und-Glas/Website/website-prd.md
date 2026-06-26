---
type: memory
status: active
tags: [gabel-und-glas, website, ux, prd, developer-brief]
created: 2026-04-30
---

# Gabel & Glas — UX & Product Requirements Document

Dieses Dokument beschreibt die vollständige UX, alle User Flows und technischen Anforderungen der gabelundglas.com Website. Verwendungszwecke: interne Dokumentation, Basis für AI-Coding-Prompts (Antigravity), Developer-Brief.

---

## 1. Projekt-Übersicht

**Produkt:** gabelundglas.com — Website für den Gabel & Glas Kulinarik-Newsletter
**Ziel der Website:** Subscriber-Akquisition, Ausgaben-Archiv mit Soft Wall, Markenauftritt
**Kern-Metrik:** Email-Signups (primär), Ausgaben-Lesedauer (sekundär)
**Framing:** Gabel & Glas als Medienmarke (nicht als "Wien-Format"). Lutzi ist der Wien-Editor des Systems, nicht das Gesicht der gesamten Marke.

### Tech Stack
- Framework: Next.js 14+ (App Router)
- Hosting: Vercel
- Styling: Tailwind CSS
- Newsletter-Backend: Beehiiv (REST API)
- Geolocation: Vercel Geo Headers (nativ, kein Drittanbieter)
- Analytics: Plausible (Script-Tag im Head)
- Link-Tracking: Dub.co (extern, nicht im Code)
- Sprache/Region: DACH (DE/AT/CH)

---

## 2. Nutzertypen

### Primär: Der neugierige Konsument (B2C)
- 25-45 Jahre, Wien oder bald München/Zürich
- Kulinarisch interessiert, kein Fachpublikum
- Liest auf dem Smartphone, kurze Aufmerksamkeitsspanne
- Motiviert durch: Geheimtipps, Wochenendplanung, Insider-Gefühl

### Sekundär: Der Branchen-Insider (B2B)
- Gastwirt, Koch, Pächter, Sommelière
- Sucht branchenrelevante Infos und Stellenmarkt
- Wird über implizites Klickverhalten identifiziert (kein explizites Feld beim Signup)

### Drittmarkt: Der Nicht-Wien-User
- Wohnt in München, Zürich oder einer anderen Stadt
- Primäre Conversion: Warteliste für seine Stadt
- Sekundäres Angebot: Wien als Live-Option zum Reinschnuppern
- Wertvoll als qualifizierter Lead für spätere Stadterweiterungen

---

## 3. User Flows

### Flow A: Normaler Signup (Wien erkannt)

```
1. User öffnet gabelundglas.com
2. Vercel Middleware liest Geo-Header → Stadt = Wien
3. Landing Page rendert mit Formular im Signup-Modus für Wien
4. User gibt Email ein
5. Optional: Recipe Briefing Checkbox (default: angekreuzt)
6. Optional: Multi-City Expander öffnen → weitere Städte (Warteliste) anhaken
7. User klickt CTA
8. POST /api/subscribe mit allen gewählten Städten als Tags
9. Beehiiv sendet DOI-Mail automatisch
10. Danke-State erscheint
```

### Flow B: Nicht-Wien-User (z.B. München erkannt)

```
1. User öffnet gabelundglas.com
2. Vercel Middleware liest Geo-Header → Stadt = München
3. Landing Page rendert im Wartelisten-Modus für München
4. Primäres Formular: "München kommt bald — auf die Warteliste"
5. Sekundäres Angebot unter dem CTA: "Wien ist schon live — reinschnuppern?"
   → kleiner Link "Für Wien anmelden" wechselt Formular auf Wien-Modus (client-side)
6. Multi-City Expander optional: zeigt alle anderen Städte außer München
7. Signup setzt Beehiiv-Tag: waitlist_münchen (+ weitere Tags falls gewählt)
```

### Flow C: Ausgabe lesen (neueste, frei)

```
1. User klickt auf neueste Ausgabe
2. /ausgaben/[slug] lädt — kein Wall, voller Content
3. Am Ende: Signup-Formular (standortabhängig) für nicht-subscribed User
```

### Flow D: Ausgabe lesen (ältere, Soft Wall)

```
1. User öffnet /ausgaben/[slug] einer älteren Ausgabe
2. Erste ~150 Wörter sichtbar
3. Blur-Effekt + Overlay mit Email-Feld + CTA
4. Nach Signup: Session-Flag gesetzt, voller Content sichtbar
```

### Flow E: Manuelle Stadt-Korrektur / Multi-City-Auswahl

```
1. User sieht vorausgefülltes Formular, Stadt stimmt nicht ODER will mehrere Städte
2. Trigger: Link "+ Auch andere Städte?" unter dem Primärformular
3. Expandierender Block erscheint (kein Seitenreload, client-side):
   → Alle Städte außer der primär gewählten als Checkboxen
   → Live-Städte: "jetzt anmelden"
   → Wartelisten-Städte: "Warteliste"
4. Jede angehakte Stadt wird als zusätzlicher Beehiiv-Tag beim Submit übergeben
5. Ein einziger POST-Request, mehrere Tags
```

---

## 4. Seitenspezifikationen

### 4.1 Landing Page `/`

**Ziel:** Maximale Conversion. Kein Informations-Overload above the fold.

#### Above the Fold (Hero Section)
- Logo (logo-full.svg)
- Headline: standortunabhängig formuliert (funktioniert für Wien und Wartelisten-User)
- Subheadline: max. 2 Zeilen, standortabhängig (Wien vs. Warteliste)
- Email-Input-Feld
- Recipe Briefing Checkbox (default: angekreuzt)
- CTA-Button: standortabhängig ("Wien-Briefing sichern" vs. "Auf die Warteliste")
- Nicht-Wien-User: kleiner Sekundär-Link "Wien ist schon live →" unter dem CTA
- Multi-City Expander: eingeklappter Link "+ Auch andere Städte?" unter allem
- Kein Nav-Menü above the fold

#### Below the Fold — Reihenfolge der Sektionen

**1. Überleitung (Brücke zwischen Hero und Lutzi)**
Headline der Sektion: "Jede Stadt. Ein Insider."
Kurzer Intro-Satz der erklärt wie das System funktioniert, bevor Lutzi vorgestellt wird.

**2. Lutzi-Sektion**
- Label/Überschrift: "Wien — Luca Presser" (kontextualisiert ihn als Wien-Editor, nicht als G&G-Gesicht)
- Kleines Foto (optional, aber empfohlen)
- 2-3 Sätze Bio (siehe website-copy.md für Optionen)
- Kein langer CV

**3. Städte-Visual**
- Wien: aktiv, farbig, "Jetzt live", "Wien-Editor: Luca Presser"
- München: grayed out, "Bald verfügbar", "Editor: coming soon", Wartelisten-Link
- Zürich: grayed out, "Bald verfügbar", "Editor: coming soon", Wartelisten-Link
- Das Visual kommuniziert das System, nicht nur Wien

**4. Briefing-Struktur**
- 5 Rubriken mit Icons und je einer Zeile Beschreibung
- Icons: icon-fork.svg, icon-glass.svg, icon-ear.svg

**5. Ausgaben-Preview**
- Letzte 3 Ausgaben als Cards
- Daten von Beehiiv Posts API (gecacht)

**6. Zweites Signup-Formular**
- Identisch mit Hero, standortabhängig
- Für User die erst nach dem Scrollen überzeugt sind

**7. Footer**

---

### 4.2 Ausgaben-Archiv `/ausgaben`

- Grid/Liste aller Ausgaben
- Neueste: kein Lock, frei lesbar
- Ältere: Lock-Icon oder "Subscriber only" Label
- Daten: Beehiiv Posts API, `next: { revalidate: 3600 }`

---

### 4.3 Einzelne Ausgabe `/ausgaben/[slug]`

- Neueste Ausgabe: voller Content
- Ältere Ausgaben: ~150 Wörter + Soft Wall
- End-of-Article Signup-CTA für nicht-subscribed User

---

## 5. Komponenten-Spezifikationen

### 5.1 Signup-Formular

```
Props:
- primaryCity: string (vorausgefüllt via Geo-Detection)
- mode: "signup" | "waitlist"
- showRecipeCheckbox: boolean (default: true)
- additionalCities: string[] (aus Multi-City Expander, default: [])

Verhalten:
- Email-Validierung client-side
- Submit → POST /api/subscribe
- Loading-State (Button disabled, Spinner)
- Success-State: Danke-Message
- Error-State: Fehlermeldung unter Feld

API Payload:
{
  email: string,
  primaryCity: string,
  primaryMode: "signup" | "waitlist",
  additionalCities: string[],
  recipe_briefing: boolean,
  utm_source: string
}
```

### 5.2 Multi-City Expander

```
Verhalten:
- Default: eingeklappt, nur Trigger-Link sichtbar
- Klick auf "+ Auch andere Städte?": Block expandiert (CSS transition, kein Reload)
- Zeigt ALLE konfigurierten Städte außer der primär gewählten
- Jede Stadt als Checkbox mit Status-Label: "Jetzt live" oder "Warteliste"
- Gewählte Städte werden in additionalCities[] des Formulars gesammelt
- Ein einziger POST beim Submit, alle Städte als separate Beehiiv-Tags

Konfiguration (zentrale Config-Datei, nicht hardcodiert):
const CITIES = [
  { name: "Wien",    beehiivTag: "city_wien",    status: "live"      },
  { name: "München", beehiivTag: "city_muenchen", status: "waitlist"  },
  { name: "Zürich",  beehiivTag: "city_zuerich",  status: "waitlist"  },
]
// Neue Stadt hinzufügen = ein Eintrag hier, Rest passiert automatisch
```

### 5.3 City Detection (Middleware)

```typescript
// middleware.ts
import { NextRequest, NextResponse } from 'next/server'

const AVAILABLE_CITIES = ['Vienna', 'Wien']

export function middleware(request: NextRequest) {
  const city = request.geo?.city ?? 'unknown'
  const country = request.geo?.country ?? 'unknown'
  const mode = AVAILABLE_CITIES.includes(city) ? 'signup' : 'waitlist'

  const response = NextResponse.next()
  response.headers.set('x-detected-city', city)
  response.headers.set('x-detected-country', country)
  response.headers.set('x-signup-mode', mode)
  return response
}
```

Genauigkeit: ~70-80% im DACH-Raum. Fehlerfall abgefangen durch Multi-City Expander.

### 5.4 Beehiiv API Route

```typescript
// app/api/subscribe/route.ts
export async function POST(request: Request) {
  const { email, primaryCity, primaryMode, additionalCities, recipe_briefing } = await request.json()

  const tags = [
    primaryMode === 'waitlist'
      ? `waitlist_${primaryCity.toLowerCase()}`
      : `subscriber_${primaryCity.toLowerCase()}`,
    ...additionalCities.map((c: string) => `waitlist_${c.toLowerCase()}`),
    recipe_briefing ? 'recipe_briefing' : null,
  ].filter(Boolean)

  const res = await fetch(
    `https://api.beehiiv.com/v2/publications/${process.env.BEEHIIV_PUB_ID}/subscriptions`,
    {
      method: 'POST',
      headers: {
        'Authorization': `Bearer ${process.env.BEEHIIV_API_KEY}`,
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({
        email,
        reactivate_existing: false,
        send_welcome_email: true,
        tags,
        utm_source: 'website',
        utm_medium: 'organic',
      }),
    }
  )

  if (!res.ok) return Response.json({ error: 'Signup failed' }, { status: 500 })
  return Response.json({ success: true })
}
```

Environment Variables (Vercel Settings):
- `BEEHIIV_API_KEY`
- `BEEHIIV_PUB_ID`

### 5.5 Ausgaben laden

```typescript
// lib/posts.ts
export async function getPosts() {
  const res = await fetch(
    `https://api.beehiiv.com/v2/publications/${process.env.BEEHIIV_PUB_ID}/posts?status=confirmed&expand=free_web_content`,
    {
      headers: { 'Authorization': `Bearer ${process.env.BEEHIIV_API_KEY}` },
      next: { revalidate: 3600 },
    }
  )
  const data = await res.json()
  return data.data
}
```

### 5.6 Soft Wall

```
Logik:
- Neueste Ausgabe (index 0): kein Wall
- Alle anderen: Soft Wall

Client-side:
- sessionStorage-Flag nach Signup setzen
- Wall-Overlay prüft Flag beim Mount

CSS:
- backdrop-filter: blur(4px) auf Content unter Fold-Line
- Overlay: absolut positioniert mit Opacity
- Kein display:none (hartes Gate ist nicht das Ziel)
```

---

## 6. Design Tokens

### Farben
```
Primary Red:  #9D121B  — Headlines, CTAs
Burgundy:     #5B0F1A  — Hover-States
Forest Green: #0B470D  — Sekundäre Akzente, Links
Gold:         #C6A35A  — Highlights
Cream:        #FDF4DD  — Hintergrundfarbe
Charcoal:     #2B2B2B  — Body-Text
Light Gray:   #CFC8BD  — Borders, Divider
```

### Fonts
```
Headlines:  "Bowl" (custom Web Font — Lizenz vorab klären)
Body:       "Cabin Sketch" (Google Fonts oder lokal)
Fallback:   Georgia, serif
```

### SVG Assets
```
logo-full.svg   — Hauptlogo mit Schriftzug
logo-mark.svg   — Icon only
icon-fork.svg   — Rubrik Gabel
icon-glass.svg  — Rubrik Glas
icon-ear.svg    — Rubrik Stadtgeflüster
```
Anforderungen: viewBox korrekt, keine hardcodierten Farben (currentColor), keine unnötigen Gruppen.

---

## 7. Rechtliches / DSGVO

- Vercel Geo Headers: IP serverseitig, nicht gespeichert, kein Drittanbieter
- Rechtsgrundlage: Legitimate Interest (Art. 6 Abs. 1 lit. f DSGVO)
- Kein Cookie-Banner nötig
- Datenschutzerklärung: Pflicht-Satz zu IP-Geolocation (siehe website-copy.md)
- Double Opt-in via Beehiiv (gesetzlich erforderlich DE/AT/CH)

---

## 8. Offene Entscheidungen

- [ ] Headline und CTA Wortlaut final (Optionen in website-copy.md)
- [ ] Bowl Font: Web Embedding Lizenz klären
- [ ] Foto von Lutzi: vorhanden und freigegeben?
- [ ] Subdomain: newsletter.gabelundglas.com für Beehiiv einrichten
- [ ] Impressum-Daten zusammenstellen
- [ ] Plausible Account anlegen und Domain verifizieren
- [ ] Open Graph Meta Tags: ja (für Social Sharing von Ausgaben)

---

## 9. Was dieser PRD nicht abdeckt

- Email-Template Design (Beehiiv, separates Dokument)
- Content-Strategie und Briefing-Struktur (preserve.md)
- Monetarisierung und Job Board (GabelGlas-Strategisches-Audit.md)
- Sticker-Konzept und QR-Tracking (preserve.md)

---

*Linked to:* [[03-AI/Gabel-und-Glas/Website/website-overview]] · [[03-AI/Gabel-und-Glas/Website/website-copy]] · [[03-AI/Gabel-und-Glas/preserve]] · [[02-Knowledge/GabelGlas-Strategisches-Audit]]
