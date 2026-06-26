---
type: memory
status: active
tags: [gabel-und-glas, website, todo, launch-checklist]
created: 2026-04-30
---

# Gabel & Glas — Website Launch Checklist

Sequentielle To-Do-Liste für Website und Backend. Phasen müssen in dieser Reihenfolge abgearbeitet werden — spätere Phasen haben Abhängigkeiten von früheren.

Legende: 👤 = du selbst · 🤖 = Antigravity · ✅ = erledigt

---

## Phase 0 — Entscheidungen & Assets (vor allem anderen)

Diese Phase muss vollständig abgeschlossen sein bevor Antigravity einen einzigen Satz Code schreibt.

### Copy-Entscheidungen (website-copy.md öffnen)
- [x] 👤 Headline final entscheiden: Option A, B oder C
- [x] 👤 CTA-Text final
- [x] 👤 Subheadline je nach Headline-Wahl
- [x] 👤 Lutzi-Bio: Option A oder B, oder Lutzi selbst schreiben lassen
- [x] 👤 Tagline im Footer: ja oder nein

### Assets
- [ ] 👤 SVG-Dateien von Figma/Illustrator exportieren
- [ ] 👤 SVGs nach Schema benennen: logo-full.svg, logo-mark.svg, icon-fork.svg, icon-glass.svg, icon-ear.svg
- [ ] 👤 SVGs prüfen: viewBox korrekt, keine hardcodierten Farben (currentColor), keine unnötigen Gruppen
- [ ] 👤 Foto von Lutzi organisieren und Freigabe einholen
- [ ] 👤 Bowl Font: Web Embedding Lizenz beim Anbieter prüfen

### Beehiiv vorbereiten
- [x] 👤 In Beehiiv Settings: Publication ID notieren
- [x] 👤 In Beehiiv Settings: API Key generieren und notieren
- [ ] 👤 Beehiiv Design-Settings konfigurieren: Farben (#9D121B, #FDF4DD), Absender-Name, From-Adresse
- [ ] 👤 DOI-Mail in Beehiiv finalem Text versehen (Lutzi-Stimme, fertig geschrieben)
- [ ] 👤 Welcome-Mail in Beehiiv finalem Text versehen (inkl. "Beruflich oder Genuss?" Klick-Frage → B2B/B2C Tag)
- [ ] 👤 Referral-Programm in Beehiiv einrichten (digitale Rewards: Meilenstein 1 + 2 definieren)

### Externe Services einrichten
- [ ] 👤 Plausible Account anlegen (plausible.io), Domain eintragen, Script-Tag notieren
- [ ] 👤 Dub.co Account anlegen, ersten generischen Signup-Link mit UTM-Parametern erstellen
- [ ] 👤 GitHub Repository anlegen (privat) für das Next.js Projekt

---

## Phase 1 — Dev-Umgebung & Deployment-Pipeline

- [ ] 🤖 Next.js Projekt initialisieren (App Router, TypeScript, Tailwind CSS)
- [ ] 👤 GitHub Repo mit lokalem Projekt verbinden
- [ ] 👤 Vercel Account mit GitHub Repo verbinden (automatisches Deployment bei push)
- [ ] 👤 Environment Variables in Vercel Settings eintragen:
  - `BEEHIIV_API_KEY`
  - `BEEHIIV_PUB_ID`
- [ ] 👤 Vercel: gabelundglas.com als Custom Domain hinzufügen
- [ ] 👤 Bei Domain-Registrar: DNS-Records auf Vercel umstellen (A-Record + CNAME)
- [ ] 👤 Deployment testen: leere Next.js Seite muss auf gabelundglas.com erscheinen

---

## Phase 2 — Infrastruktur & Middleware

- [ ] 🤖 CITIES Config-Datei anlegen (zentrale Wahrheit für alle Städte-Logik):
  ```
  Wien: status live, beehiivTag: city_wien
  München: status waitlist, beehiivTag: city_muenchen
  Zürich: status waitlist, beehiivTag: city_zuerich
  ```
- [ ] 🤖 Vercel Middleware schreiben: Geo-Header lesen, Stadt und Mode (signup/waitlist) in Request-Header setzen
- [ ] 🤖 API Route /api/subscribe bauen: Email + primaryCity + additionalCities[] + recipe_briefing → Beehiiv POST mit Tags
- [ ] 🤖 API Route testen: curl-Calls mit verschiedenen Städten, Beehiiv-Dashboard auf Tags prüfen
- [ ] 🤖 lib/posts.ts bauen: Beehiiv Posts API fetchen mit 1h Cache

---

## Phase 3 — Komponenten bauen

- [ ] 🤖 SVG-Assets als React-Komponenten importieren
- [ ] 🤖 Design Tokens in Tailwind Config eintragen (alle Hex-Codes, Fonts)
- [ ] 🤖 Plausible Script-Tag in Next.js layout.tsx einbinden
- [ ] 🤖 SignupForm Komponente: Email-Input, Recipe Checkbox, CTA Button, Loading/Success/Error States, standortabhängige Props
- [ ] 🤖 MultiCityExpander Komponente: eingeklappter Trigger-Link, expandierender Block mit Checkboxen je Stadt, additionalCities[] State
- [ ] 🤖 CityMode Logik: Wien erkannt → Signup-Modus / andere Stadt erkannt → Wartelisten-Modus + sekundärer Wien-Link
- [ ] 🤖 SoftWall Komponente: blur + Overlay + eingebettetes Mini-Signup-Formular + sessionStorage Flag-Logik
- [ ] 🤖 PostCard Komponente: Ausgabe-Nummer, Datum, Titel, 1-Satz-Teaser, Link

---

## Phase 4 — Seiten bauen

### Landing Page /
- [ ] 🤖 Hero Section: Logo, Headline, Subheadline, SignupForm (standortabhängig), MultiCityExpander
- [ ] 🤖 Below the fold: Überleitung "Jede Stadt. Ein Insider."
- [ ] 🤖 Lutzi-Sektion: Label "Wien — Luca Presser", Foto, Bio
- [ ] 🤖 Städte-Visual: Wien aktiv, München + Zürich grayed out mit "Editor: coming soon" und Wartelisten-Links
- [ ] 🤖 Briefing-Struktur: 5 Rubriken mit Icons und Beschreibung
- [ ] 🤖 Ausgaben-Preview: letzte 3 Issues als PostCards (Daten von Beehiiv API)
- [ ] 🤖 Zweites Signup-Formular am Ende
- [ ] 🤖 Footer: Logo, Links, jobs@gabelundglas.com

### Ausgaben-Archiv /ausgaben
- [ ] 🤖 Grid/Liste aller Ausgaben mit PostCards
- [ ] 🤖 Neueste: kein Lock / Ältere: Lock-Icon oder "Subscriber only" Label

### Einzelne Ausgabe /ausgaben/[slug]
- [ ] 🤖 Dynamische Route mit Beehiiv slug
- [ ] 🤖 Neueste Ausgabe: voller Content, kein Wall
- [ ] 🤖 Ältere Ausgaben: ~150 Wörter + SoftWall Komponente
- [ ] 🤖 End-of-Article Signup-CTA für nicht-subscribed User
- [ ] 🤖 Open Graph Meta Tags: Titel + erster Satz als Description, logo-mark.svg als OG-Image

---

## Phase 5 — Rechtliches

- [ ] 👤 Impressum-Seite: Name, Adresse, Email-Adresse
- [ ] 👤 Datenschutzerklärung: Standard-Text + Pflicht-Satz zu IP-Geolocation ("IP wird serverseitig zur Stadterkennung verwendet, nicht gespeichert, nicht weitergegeben")
- [ ] 🤖 /impressum und /datenschutz als statische Seiten anlegen, Footer-Links darauf zeigen

---

## Phase 6 — Beehiiv Subdomain

- [ ] 👤 In Beehiiv Settings: Custom Domain auf newsletter.gabelundglas.com setzen
- [ ] 👤 Bei Domain-Registrar: CNAME-Eintrag für newsletter.gabelundglas.com auf Beehiiv-Ziel setzen (Beehiiv zeigt dir den genauen Wert)
- [ ] 👤 Warten bis DNS propagiert (5 min bis 24h), dann in Beehiiv verifizieren
- [ ] 👤 Alle Beehiiv-Links (DOI-Mail, Unsubscribe) testen ob sie auf newsletter.gabelundglas.com zeigen

---

## Phase 7 — Email Deliverability

- [ ] 👤 SPF-Record im DNS prüfen: Beehiiv's Sending-Domain muss eingetragen sein (Beehiiv Docs → Domain Authentication)
- [ ] 👤 DKIM im DNS einrichten: Beehiiv liefert den DNS-Record, du trägst ihn beim Registrar ein
- [ ] 👤 DMARC-Record setzen: p=none (Monitoring-Modus für den Start)
  ```
  TXT _dmarc.gabelundglas.com "v=DMARC1; p=none; rua=mailto:dmarc@gabelundglas.com"
  ```
- [ ] 👤 MXToolbox DMARC Checker: alle drei Records validieren

---

## Phase 8 — End-to-End Test

Alles muss vor dem ersten echten Subscriber durchgespielt werden.

- [ ] 👤 Wien-Flow: selbst anmelden, DOI-Mail erhalten, Link klicken, Welcome-Mail erhalten, Beehiiv-Dashboard prüfen (Tags korrekt?)
- [ ] 👤 Wartelisten-Flow: mit VPN auf München wechseln, Formular testen, Tag waitlist_münchen im Dashboard prüfen
- [ ] 👤 Multi-City: Wien + München anhaken, beide Tags im Dashboard prüfen
- [ ] 👤 Recipe Briefing: abgehakt und nicht abgehakt, Tags prüfen
- [ ] 👤 Soft Wall: ältere Ausgabe öffnen, Wall erscheint, Email eingeben, Wall verschwindet, sessionStorage Flag gesetzt
- [ ] 👤 Mobile: gesamte Landing Page auf iPhone prüfen (Safari + Chrome)
- [ ] 👤 Email-Rendering: DOI + Welcome-Mail in Gmail, Apple Mail, Outlook prüfen
- [ ] 👤 Plausible Dashboard: Seitenaufruf erscheint, kein Cookie-Banner

---

## Phase 9 — Seed-Liste & Launch

- [ ] 👤 Seed-Liste aufbauen: 20-30 Leute aus persönlichem Umfeld direkt anschreiben, manuell in Beehiiv importieren (DOI überspringen für direkte Bekannte — Beehiiv erlaubt das)
- [ ] 👤 Erste Ausgabe fertigschreiben (Lutzi + Claude)
- [ ] 👤 Ausgabe in Beehiiv einbauen, Preview im Browser checken
- [ ] 👤 Test-Send an dich selbst, nochmal in echtem Email-Client anschauen
- [ ] 👤 Rausschicken

---

## Parallel (zeitunabhängig, sobald möglich)

- [ ] 👤 Dub.co: Sticker-Links anlegen (ein Link pro geplantem Lokal, QR-Code generieren)
- [ ] 👤 jobs@gabelundglas.com Email-Adresse einrichten
- [ ] 👤 Lutzi: Bio-Text und Foto-Freigabe einholen
- [ ] 👤 Paul Ostwald: Telefonat vereinbaren (hat es selbst angeboten)
- [ ] 👤 Presseartikel-Strategie überlegen (laut Paul vor Angel-Gesprächen)

---

*Linked to:* [[03-AI/Gabel-und-Glas/Website/website-prd]] · [[03-AI/Gabel-und-Glas/Website/website-copy]] · [[03-AI/Gabel-und-Glas/preserve]]
