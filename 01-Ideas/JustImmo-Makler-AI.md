---
type: project
status: archived
tags:
  - side-project
  - proptech
  - austria
  - ai
  - justimmo
  - real-estate
created: 2026-05-05
updated: 2026-05-05
---
# JustImmo Makler AI Platform

## Decisions & facts

**Kern-Konzept:** B2B-Service für unabhängige österreichische Immobilienmakler die JustImmo nutzen. Einstieg via AI Exposé Generator (Pull aus JustImmo API, Text generieren, Write-Back via Business API), langfristig Custom Website + AI Add-ons.

**Zielkunde (ICP):** Unabhängige Maklerbüros (3-15 MA) in Wien, JustImmo-Nutzer, kein Franchise (kein RE/MAX, s REAL, Raiffeisen). Schätzzahl: 200-500 relevante Firmen in Wien.

**Differenzierungsmerkmal:** Einziger Anbieter mit JustImmo Write-Back — generierter Exposé-Text wird direkt via PATCH /realties/{id} ins CRM zurückgeschrieben und propagiert automatisch auf alle 80+ Portale. Kein deutscher Konkurrent (Immowriter, bloxl, Textosé) macht das für JustImmo.

**Preisarchitektur:**
- Exposé Generator allein: €39/month
- Generator + Custom Website: €149/month
- Beta-Preis für erste Kunden: €29/month (dauerhaft)

**Technischer Stack:**
- Normale API (REST v1): XML, HTTP Basic Auth, Read-only — reicht für Website-Display
- Business API (v1): JSON, OAuth2 (Client Credentials), 61 Endpoints — nötig für Write-Back, Webhooks, Kontakte, Termine
- Auth für Business API: OAuth2 mit client_id/secret von JustImmo + X-Justimmo-TenantId Header pro Makler
- Webhooks: WebhookRealty (neues Objekt) triggert automatische Exposé-Generierung
- Azure OpenAI Frankfurt (Zero-Data-Retention) für DSGVO-Compliance — nicht OpenAI direkt
- Frontend: Next.js auf Vercel, JustImmo PHP-SDK oder REST direkt

**Wichtigste Risiken:**
1. JustImmo könnte Business API-Zugang verweigern oder einschränken (existenzielles Risiko)
2. JustImmo baut AI Exposé Generator nativ in 12-24 Monaten (Commoditisierungsrisiko)
3. Österreichische Makler zahlen nicht für €39/month wenn ChatGPT Plus €23 kostet (Demand-Risiko)

**Validierte Konkurrenz:**
- itweb.at (Linz): JustImmo Custom Websites seit 2011, direkter Wettbewerber im Website-Segment
- Propup.at (Wien): AI Workflows für JustImmo als Consulting, kein Produkt — kein direkter Konkurrent
- Immowriter.de: €39-69/month Exposé Generator, onOffice-Integration, keine JustImmo-Anbindung
- immoprofessional.com: SaaS Website + AI ab €69.95/month, kein JustImmo Write-Back

**Marktgröße Wien (konservativ):**
- ~460-500 JustImmo-Firmen in Wien
- 5% Penetration = 25 Kunden = ~€3.700 MRR = €44.700 ARR
- 10% Penetration = 50 Kunden = ~€7.450 MRR = €89.400 ARR
- Lifestyle/Bootstrap-Business, kein Venture-Scale ohne DACH-Expansion

**Distribution (in Priorität):**
1. ÖVI (Österreichischer Verband der Immobilienwirtschaft) — ~440 Mitgliedsfirmen, JustImmo sponsert dort bereits
2. Immobilienring Österreich — ~400 unabhängige Maklerbüros, idealer ICP
3. Direktansprache via WKO-Liste + JustImmo-Template-Erkennung auf Websites
4. OIZ / immomedien.at Presseartikel

---

## Log

### 2026-05-05 — Konzeptentwicklung und API-Analyse

**Was passierte:** Ausgehend von der Frage "wie Geld verdienen in 2 freien Monaten" wurde nach mehreren Iterationen das JustImmo Makler AI Konzept entwickelt. Deep Research (zwei parallele Reports) hat das Konzept validiert. JustImmo Business API analysiert: 61 Endpoints, OAuth2, Webhooks — Write-Back technisch möglich.

**Decisions:**
- AI Exposé Generator (nicht Website) ist das MVP und der schärfste Differenzierungspunkt
- Business API braucht OAuth2-Zugang von JustImmo — Email an B&G Consulting & Commerce ist Schritt 1
- Azure OpenAI Frankfurt von Anfang an, nicht OpenAI direkt (DSGVO)
- Website ist Trojanisches Pferd, nicht das Produkt

**Open threads:**
- JustImmo kontaktieren: erlauben sie Third-Party-Business-API-Nutzung? Gibt es Partner-Programm?
- JustImmo Business API: wie kommt man an client_id/secret als Drittentwickler?
- Propup.at genauer analysieren bevor erster Pitch
- Discovery Calls mit 5-8 Wiener Maklern führen (Woche 1-2 des Plans)
- Existiert ein Exposé-Generator der Write-Back in irgendein CRM macht? (Immowriter/onOffice nativ — für JustImmo nicht)
