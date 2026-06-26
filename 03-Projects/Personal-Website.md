---
type: project
status: active
tags:
  - website
  - blog
  - next-js
  - personal-branding
created: '2026-05-28'
updated: '2026-05-28'
---
# Personal Website

## Decisions & facts

### Why
- Portfolio signal: "ich hab mein eigenes Ding von null gebaut" > "ich nutze Substack" — relevant für VC/Founding-Weg
- Marginaler Aufwand, weil Next.js eh für [[Gabel-und-Glas]] gelernt wird
- Existierender Content: 5 Substack-Posts sind sofort migrierbar
- Timing: Grenoble-Kontakte werden googeln — besser eine eigene Site als ein Substack-Profil

### Was es nicht ist
- Kein Ersatz für Substack als Email-Distribution
- Kein Versuch nach Aufmerksamkeit zu schreien
- Kein klassisches Portfolio mit Projects-Seite und Screenshots

### Struktur: drei Seiten, nicht mehr

**1. Blog (Homepage)**
- Direkt die Posts, kein Hero, kein großer Claim
- Liste: Titel + Datum + ein Satz
- Stil: exakt wie bisherige Substack-Posts, keine Anpassung nötig
- Der Stil ist bereits das Differenzierende: casual aber nicht dumb, selbstironisch, echte Meinungen mit echtem Vorbehalt

**2. About (Changelog)**
- Kein "passionate about"-Text, keine Biografie
- Versionierte Einträge mit Datum: was gemacht, was rausgekommen ist, wo jetzt
- Vier bis fünf Einträge für Launch ausreichend
- Kommuniziert "jemand der in Iterationen denkt" ohne es auszusprechen
- Beispiel-Einträge schreiben bevor gebaut wird — einziger Content der neu erstellt werden muss

**3. G&G**
- Kein ausgewachsener Pitch, kein eigener Tab
- Ein Absatz auf der About-Seite oder Mini-Seite: ein Satz warum, ein Link
- Verbindet beide Projekte ohne sie zu vermischen

### Was bewusst weggelassen wird
- Projects-Seite: wirkt wie "schaut mich an", noch zu wenig Material
- Contrarian Index / Bet Ledger: zu hohes Risiko für jemanden der noch eine Reputation aufbaut
- Working Hypotheses als eigene Seite: redundant, weil das im Blog schon passiert
- Anti-Portfolio / Kill File: gute Idee, falscher Zeitpunkt — zu wenig Material
- Kommentarfunktion: Substack übernimmt das

### Stack
- Next.js + Vercel (gleich wie G&G, kein neues Terrain)
- MDX für Posts: Markdown mit der Option React-Components einzubetten wo gebraucht
- Google Antigravity als IDE (wie für G&G geplant)

### Substack-Verhältnis
- Substack bleibt als Email-Distribution aktiv
- Website ist der kanonische Home
- Discovery-Verlust ist vernachlässigbar (Substack-Discovery funktioniert erst ab ~500+ Subscribern wirklich)

### Offene Entscheidungen (vor Build zu klären)
- [ ] Domain: yonabeer.com vs yona.beer — noch nicht entschieden
- [ ] Analytics: Vercel Analytics (zero-config, empfohlen für Start) oder Plausible (privacy-first, kostenpflichtig)
- [ ] Dark/Light Mode: wahrscheinlich ja, Farbschema offen
- [ ] RSS Feed: technisch trivial in Next.js, aber explizit einplanen
- [ ] OG Images: statisch oder dynamisch via `next/og` — wichtig für Social Sharing
- [ ] Crosspost-Workflow: manuell oder automatisiert via Substack API

### Das einzige echte technische Problem: Embeds
Die bisherigen Posts sind media-heavy. Konkret zu lösen:
- **Twitter/X**: `react-tweet` Package (server-side, kein Layout-Shift, empfohlen)
- **Substack-Cards**: einfaches Link-Preview-Component mit `open-graph-scraper`
- **YouTube**: Standard iFrame mit `aspect-ratio: 16/9` Wrapper, kein extra Package nötig
- **Charts/Bilder**: als MDX-Assets, Vercel optimiert automatisch via `next/image`
- Diese Entscheidungen vor der ersten Post-Migration treffen, nicht nachher

### Timing
- Nicht parallel zur G&G Launch-Phase (Priorität hat G&G)
- Ziel: Juli oder August 2026, vor Grenoble
- Scope ist eng genug für ein langes Wochenende wenn fokussiert

### Changelog-Einträge (Entwurf, noch zu finalisieren)
```
v0.1 — 2025 · Fintech-Praktikum bei froots, erste echte Einblicke in VC-Logik
v0.2 — früh 2026 · Morningcrunch, Media-Startup, kurze aber dichte Episode
v0.3 — März 2026 · Substack gestartet: wöchentliche Gedanken zu Macro, AI, Startups
v0.4 — 2026 · Gabel & Glas: kuratierter Kulinarik-Newsletter für Wien, im Aufbau
v0.5 — Sept 2026 · Grenoble École de Management, Innovation & Entrepreneurship
v1.0 · noch offen
```
Konkrete Formulierungen noch ausschreiben — das ist der einzige wirklich neue Content für Launch.

---

## Log

### 2026-05-28 — Konzept finalisiert
**Was passiert:** Mehrstufige Konzeptarbeit: Ideation, Risikobewertung, Stil-Analyse der bisherigen Substack-Posts, finale Entscheidung für minimalen Scope.
**Decisions:**
- Drei Seiten: Blog, About (Changelog), G&G-Erwähnung
- Stack: Next.js + Vercel + MDX
- Substack bleibt für Email-Distribution
- Embeds als einziges echtes technisches Risiko identifiziert
- Timing: nach G&G Launch-Stabilisierung, Juli/August 2026
**Open threads:**
- Domain noch nicht entschieden
- Changelog-Einträge noch nicht fertig formuliert
- Crosspost-Workflow noch offen
