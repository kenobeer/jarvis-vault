# Produkt & Operations — Bitschnau EdTech

→ Hauptprojekt: [[Bitschnau-EdTech]] · Strategie: [[Strategie]]

---

## Bestehende Produkte (Status aktuell ungeklärt, ob aktiv oder pausiert)

### Diplomlehrgang
"Klassische Homöopathie – Schwangerschaft, Geburt, Wochenbett und Stillphase"
- 7 Wochenend-Module, berufsbegleitend
- Zielgruppe: Ärztinnen und Hebammen, kein Vorwissen nötig
- Didaktik: Praxisfälle (Kasuistiken), interaktiv, Lerncoaching, Supervision
- Abschluss: Diplom des Ausbildungszentrums, Privatklinik Döbling
- Laut Micha (Treffen Juni 2026): die Kurse waren früher eigentlich immer ausgebucht

### Aufbaulehrgang
"Klassische Homöopathie advanced"
- 10 eintägige Workshops
- Voraussetzung: Grundkenntnisse (Nachweis oder Aufnahmegespräch)
- Schwerpunkt: Case Management, Arzneimittellehre, Hands-on unter Supervision

### Akkreditierung (Stand: In-Person-Format)
- **AT Hebammengremium:** 75 Fortbildungspunkte (§ 37(6) HebG), Kategorie 3 (3 Pkt/h)
- **DFP Ärztekammer AT:** noch nicht akkreditiert, politisch zunehmend heikel
- **DE, CH:** nicht akkreditiert

**KRITISCH:** Akkreditierung gilt für In-Person-Format. Online-Format = neuer Antrag.
Step Zero: Anruf AT Hebammengremium. Klären: Welches Live-/Async-Verhältnis ist für 75 Punkte nötig?

---

## Produktarchitektur Digital

**Empfohlenes Format: Hybrides Kohorten-Modell**

Aufbau pro Modul:
- Aufgezeichnete Video-Einheiten (Asset, einmal produziert, unbegrenzt nutzbar)
- 1 Live-Q&A mit Micha pro Modul, ~60-90 Minuten (ihr aktiver Aufwand)
- Dokumentierte Anwesenheit für Akkreditierungsnachweise
- Automatisches Completion-Tracking und Zertifikat bei Abschluss

Kohortengröße: 18-24 Teilnehmerinnen. Begründung: groß genug für wirtschaftlich sinnvolle Gruppe, klein genug damit Live-Termine persönlich bleiben.

Michaz Gesamtzeitaufwand im Laufbetrieb:
- 7 Module × 1,5h Live-Q&A = ~10,5h pro Kohorte
- Bei 2-3 Kohorten/Jahr und versetztem Timing: wenige Stunden pro Monat aktiv

Yonas Aufwand im Laufbetrieb: ~3-5h/Woche (Remote-kompatibel)

**Warum Kohorten und kein Self-paced:**
- Completion-Rate Self-paced: 10-15%; Kohorten: 60-80%
- Kaufentscheidungs-Druck durch Startdatum
- Akkreditierungsstellen verlangen dokumentierte Anwesenheit
- Bei festem Starttermin schließen deutlich mehr Teilnehmerinnen den Kurs auch tatsächlich ab

---

## Pricing-Strategie (korrigiert, Stand 21.06.2026)

**Hebammenkurs (Diplomlehrgang digital): €900-1.600/Person**

Ursprüngliche Annahme (€1.700-1.800) beruhte primär auf dem BfHD/Stadelmann-Vergleichskurs. Bei genauerer Prüfung erwies sich dieser Vergleich als unsicher:
- Die beiden ursprünglichen Research-Reports schrieben den Preis ~€1.750 teils dem falschen Produkt zu (Vermischung von Online-Jahreskurs und einem separaten, viel günstigeren Präsenz-Zertifikat)
- Der Vergleichskurs ist von einer Hebamme für Hebammen, nicht von einer Fachärztin, und aus Deutschland statt Österreich
- Digitale Formate sind in der Regel nicht teurer als Präsenzformate, oft eher günstiger (kein Live-Erlebnis, keine Hands-on-Supervision)

**Robustere Herleitung der neuen Spanne:**
- Preis pro Unterrichtseinheit über mehrere unabhängige Anbieter: 15-24 EUR/UE (DÄGfA, AIM, UKE Hamburg, DMG-Curriculum)
- Diplomlehrgang hat ~85-110 UE → rechnerisch ca. €1.275-2.600, aber das ist der Wert für etablierte Anbieter mit Marktposition, nicht für einen neuen Online-Umstieg
- Österreichischer Vergleichswert (ÖGHM Diplomausbildungen): €840-1.200 — näher an Michaz tatsächlichem Markt als der deutsche Vergleich
- Resultierende Spanne: konservativ €900, optimistisch €1.600

**Die eigentlich entscheidende fehlende Zahl:** Was hat Michaz Präsenzkurs früher gekostet? Das ist die wichtigste einzelne offene Frage im Projekt — wichtiger als die Frage zum Status des Ausbildungszentrums, weil sie die gesamte Preislogik entweder bestätigt oder kippt. Bis diese Antwort da ist, ist die Preisspanne explizit als vorläufig zu kommunizieren.

**Perimenopause-Kurs (Ärztinnen): €1.000-1.400/Person**
20-25 Teilnehmerinnen, Frequenz pro Jahr noch offen (daher Kalkulation "pro Durchgang", nicht "pro Jahr"). Bewusst unter €1.500 — siehe Erweiterungs-Abschnitt in [[Strategie]] für die Begründung (Marktanalyse Ärzte-CME zeigt: privater Einzelanbieter ohne Uni-/Diplom-Anbindung kommt selten über €1.500).

**Kombinierte Umsatzrechnung (Umsatz vor Kosten):**

| | Teilnehmer | Preis/Person | Durchgänge/Jahr | Umsatz |
|---|---|---|---|---|
| Hebammenkurs | 18-24 | €900-1.600 | 2-3 | €32.000-115.000 |
| Perimenopause-Kurs | 20-25 | €1.000-1.400 | 1 (offen) | €20.000-35.000 |
| **Gesamt, wenn beide laufen** | | | | **€52.000-150.000** |

---

## Plattform

**Entscheidung: Ablefy (ehemals elopage)**

Warum Ablefy, nicht Kajabi:
- Deutsches Unternehmen, Server in DE: DSGVO-nativ, kein Schrems-II-Risiko
- B2B-tauglich: automatisierte EU-Rechnungen mit Umsatzsteuer (wichtig wenn Kliniken die Kursgebühren erstatten)
- Reseller-Modell: korrekte internationale VAT-Abfuhr automatisch
- All-in-one: Kurse + Checkout + Email-Marketing + Affiliates
- ~€29-49/Monat

Kajabi explizit ausgeschlossen: US-Server, Schrems-II-Risiko, angreifbarer AVV, blockiert B2B-Erstattung durch Arbeitgeber und erzeugt DSGVO-Haftungsrisiko.

**Eigene kleine Website fürs Ausbildungszentrum:** wahrscheinlich sinnvoll, weil sich das auf der bestehenden Vienna Female Medcare Website nicht gut abbilden lässt. Im Stundensatz enthalten, kein separater Kostenpunkt.

**Für spätere internationale Skalierung:** EACCME-Akkreditierung nur bei echter europaweiter Vermarktung sinnvoll. Korrektur: Gebühren liegen bei €600 Flat-Fee + €600/Stunde für E-Learning (nicht €500/Modul wie ursprünglich angenommen), zusätzlich verlangt EACCME eine Mitentwicklung mit einer ärztlichen Organisation. Kein naher Schritt, siehe [[Strategie]].

---

## HWG-Compliance — Nicht optional

BGH-Urteil I ZR 96/10: Werbung für homöopathische Mittel mit Indikationsangaben ist auch gegenüber Fachkreisen verboten. Gilt für Landingpages, Ads, Newsletter, LinkedIn.

**Regel für alle Werbetexte:**
- Erlaubt: "Lernen Sie die Methode der klassischen Homöopathie in der Geburtshilfe"
- Verboten: "Erfahren Sie welches Mittel bei postpartalen Blutungen wirkt"

**Vor Launch: Medizinrechtler beauftragen.** Gesamten Funnel prüfen lassen. Einmalige Kosten ~€500-1.500. Verglichen mit einer Abmahnklage: trivial.

---

## Yonas Rolle

**Aufbauphase (Juni-August 2026, ca. 100-130h):**
- Plattform-Setup (Ablefy) + kleine eigene Website fürs Ausbildungszentrum
- Landing Page + Sales Copy (nach HWG-Prüfung)
- Branding Ausbildungszentrum (Name, visuelle Identität)
- Akkreditierungsanfrage beim Hebammengremium koordinieren
- Aufnahme-Sessions koordinieren (Technik, Scheduling, Nachbearbeitung)
- Alumni-Outreach: erste Waitlist aufbauen
- Medizinrechtler beauftragen und Prozess begleiten
- Vertrag ausarbeiten und abschließen
- Stunden von Anfang an tracken (Transparenz, Erfahrung aus Morningcrunch-Zeit)

**Laufbetrieb (Remote ab September 2026, ca. 3-5h/Woche):**
- Enrollment-Management (weitgehend automatisiert via Ablefy)
- Teilnehmerinnen-Kommunikation
- Live-Q&A-Scheduling für Micha
- Zertifikatsausgabe
- Newsletter und LinkedIn-Posts erstellen, plus Organisation kleinerer öffentlicher Auftritte (z.B. Podcast-Gastauftritte), immer in Absprache mit Micha
- Monitoring (Completion-Rates, Support, Conversion)

**Übergabefähigkeit:** Das gesamte Setup wird von Anfang an so gebaut, dass eine Übergabe an eine andere Person später reibungslos möglich ist, falls Yona das Projekt irgendwann nicht mehr selbst betreiben kann. Konkrete Vergütungsfolgen einer Übergabe gehören in den Vertrag (Exit-Klausel), nicht ins Konzeptpapier.

**Michaz Rolle:**
- Live-Q&As (wenige Stunden pro Monat pro aktiver Kohorte)
- Content-Aufnahmen (einmalige Investition, dann Asset)
- Inhaltliche Qualitätskontrolle
- Institutionelle Netzwerke aktivieren (PIKÖ, ÖGHM, Hebammengremium, NATUM-Kontakt)

---

## Vertrag — Was schriftlich sein muss vor Tag 1

- Stundensatz Yona (Aufbauphase) + Gewinnbeteiligung ab Laufbetrieb, Rechnungsstellung regelmäßig
- Definition Gewinnbeteiligung: Basis (Nettogewinn nach Plattform-, Rechts- und Marketingkosten), Auszahlungsrhythmus
- IP-Ownership: Plattform, Materials, Email-Liste gehören wem?
- Exit-Klausel: Was passiert wenn Yona das Projekt übergibt, inkl. wie sich Stundensatz/Gewinnbeteiligung dann verändern
- Haftung: Wer haftet für HWG-Verstöße wenn Copy von Yona erstellt wurde?
- Laufzeit und Kündigungsfristen

---

## Timeline

**Juni 2026 (sofort)**
- Zweittreffen mit Lutz und Micha (Konzeptpapier liegt vor)
- Michaz früheren Kurspreis erfragen — Priorität vor allen anderen Fragen
- Status Ausbildungszentrum klären (aktiv/pausiert, Material, Alumni-Liste)
- Anruf AT Hebammengremium: Online-Akkreditierungsvoraussetzungen klären
- Schriftlichen Vertrag abschließen
- Medizinrechtler kontaktieren

**Juli 2026**
- Ablefy-Account + kleine Website aufsetzen
- Landing Page Entwurf (nach HWG-Briefing)
- Erste 2-3 Module aufnehmen
- NATUM-Kontakt prüfen

**August 2026**
- Landing Page live (nach Rechtsprüfung)
- Alumni-Outreach + Waitlist aufbauen
- Pilotkohorte ankündigen und füllen (Ziel: 18-24 Personen, Start Oktober/November)
- Restliche Module vorbereiten

**September 2026+ (Remote, Grenoble)**
- Pilotkohorte läuft
- Micha liefert Live-Q&As, Yona koordiniert Remote
- Restliche Module werden laufend aufgenommen
- Nach Pilotabschluss: Feedback auswerten, Preis für Kohorte 2 final festlegen
- DE-Akkreditierung (NRW HebBO) beantragen für Kohorte 2/3

**Jahr 2 (2027)**
- 2-3 volle Kohorten Diplomlehrgang
- Aufbaulehrgang für erste Alumni
- LinkedIn-Präsenz etabliert
- Perimenopause-Kurs ggf. parallel gestartet, falls Go von Micha
- NATUM-Kooperation geprüft und ggf. aktiviert
