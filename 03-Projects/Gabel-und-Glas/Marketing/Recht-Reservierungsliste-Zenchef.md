---
type: research
status: active
tags:
  - gabel-und-glas
  - marketing
  - recht
  - dsgvo
  - tkg
  - reservierungsliste
created: 2026-06-28
---
# Recht: Reservierungsliste (Zenchef) für G&G-Newsletter

Frage: Dürfen die ~1.000–3.000 E-Mail-Adressen aus Lutz' Reservierungstool (Zenchef) direkt für den Gabel & Glas Newsletter genutzt werden?

**Kein Rechtsrat. Recherche-Stand, vor dem ersten Send einem Datenschutzjuristen vorlegen.**

---

## Verdikt

**Im Default-Fall: nein.** Die bestehende Liste ist nicht ohne Weiteres für den G&G-Newsletter nutzbar. Sie ist als gesperrt zu behandeln, bis ein Jurist einen sauberen Weg bestätigt.

---

## Rechtliche Grundlage

Relevanter Paragraph: **§174 TKG 2021** (vormals §107 TKG 2003, bestehende Rechtsprechung gilt fort).

Grundregel: E-Mail-Werbung (Direktwerbung) braucht die **vorherige, ausdrückliche Einwilligung** des Empfängers (Opt-in). "Direktwerbung" wird vom OGH sehr weit ausgelegt — jede auf Absatz gerichtete Äußerung. Bereits ein einziges Werbe-Mail ohne gültige Grundlage erfüllt einen Verwaltungsstraftatbestand. Die 50-Empfänger-Grenze ist seit 2018 weg, gilt also auch für kleine Sends.

## Warum die Bestandskunden-Ausnahme (§174 Abs 4) NICHT greift

Die Ausnahme erlaubt Werbung ohne Einwilligung nur, wenn **alle** Bedingungen kumulativ erfüllt sind. Hier scheitert sie mehrfach:

1. **"Eigene ähnliche Produkte"** — Die Ausnahme gilt nur für eigene, ähnliche Produkte/Dienstleistungen des Absenders. G&G ist eine **eigene Marke, getrennt vom Restaurant** (das Doppler). Newsletter-Versand an die Restaurant-Liste ist damit **Dritt-Werbung**, nicht "eigenes Produkt".
2. **Kein "ähnliches Produkt"** — Ein Food-Newsletter ist keine ähnliche Dienstleistung zu einer Tischreservierung/Gastronomie. Eng auszulegen.
3. **Zweckbindung (DSGVO Art. 5)** — Die Daten wurden zur Tischbuchung erhoben, nicht fürs Marketing. Umwidmung ohne eigene Grundlage unzulässig.
4. **Controller-Mismatch** — Datenverantwortlicher ist das Restaurant (Zenchef ist nur Auftragsverarbeiter, "full data ownership" liegt beim Restaurant). G&G ist ein anderer Verantwortlicher. Weitergabe an G&G ist Transfer an einen Dritten.

**Präzedenz OLG Wien:** Selbst breite Einwilligungsklauseln, die Dritt-Angebote oder nicht-ähnliche Produkte umfassen ("zusätzliche Angebote rund ums Reisen"), wurden als rechtswidrig eingestuft.

---

## Was sauber funktioniert (Wege nach vorn)

| Weg | Mechanik | Effekt |
|---|---|---|
| **Going-forward Opt-in in Zenchef** | Separate, **nicht vorangekreuzte** Checkbox im Reservierungsflow, spezifisch für den G&G-Newsletter, mit klarer Widerspruchsmöglichkeit | Baut die Liste sauber auf — schaltet aber die bestehenden 1–3k **nicht** frei |
| **In-Restaurant aktives Opt-in** | QR auf Tischkarte / Bon / Aufsteller bei das Doppler → Gast meldet sich **selbst** an (DOI) | Saubere, einwandfreie Subscriber |
| **Bestehende Liste anmailen, um Einwilligung zu holen** | ❌ Nicht zulässig — diese Anfrage-Mail wäre selbst schon unzulässige Werbung | Geht nicht |

---

## Offen zu klären (mit Jurist / mit Lutz)

- Hat Zenchef beim Buchen überhaupt ein Marketing-Opt-in erfasst — und wenn ja, auf wen lautend? (Mit hoher Wahrscheinlichkeit nur auf das Restaurant, nicht auf G&G.)
- Gibt es einen sauberen Pfad, bei dem das Restaurant selbst als Absender auftritt? (Riskant, eher nicht empfehlenswert.)
- DOI-Setup (DMARC/SPF/DKIM) muss ohnehin vor dem ersten Send stehen.

---

## Strategische Konsequenz

Annahme 1 aus dem Phase-0-Framing (Reservierungsliste als Launch-Motor) **löst sich auf zu: nein**. Die Marketing-Strategie baut auf In-Restaurant-Opt-in, Going-forward-Erfassung über Zenchef und organischem Wachstum. Die alte Liste fließt nur ein, falls ein Jurist nachträglich einen sauberen Weg findet — als Upside, nicht als Basis.

---

*Linked to:* [[03-Projects/Gabel-und-Glas/Marketing/Marketing-Strategie-Phase0]] · [[03-Projects/Gabel-und-Glas/Gabel-und-Glas]] · [[03-Projects/Gabel-und-Glas/analyse-schwachstellen]]
