---
created: '2026-06-03'
status: active
tags:
  - klara
  - build
  - projekt
  - voice-ai
type: project-plan
---
# Klara — Build Plan

Milestone-basierter Umsetzungsplan. Jedes Milestone hat eine klare Fertig-Bedingung die du selbst testen kannst. Nie zum nächsten Milestone bevor das aktuelle wirklich funktioniert.

**Tools:** Claude Code, Cursor, Google Antigravity
**Stack:** Python FastAPI · Twilio Media Streams · GPT-Realtime-2 · Supabase · Celery · Redis · Railway · Next.js · Stripe

→ Hauptkonzept: [[Klara]]

---

## Fortschritt

- [x] M0 — Setup & Accounts ✅ Abgeschlossen 16. Juni 2026
- [ ] M1 — Klara nimmt ab
- [ ] M2 — Klara erinnert sich
- [ ] M3 — Klara ruft an
- [ ] M4 — Familie bekommt WhatsApp
- [ ] M5 — Alles auf Railway
- [ ] M6 — Erster externer User kann sich anmelden
- [ ] M7 — Erster echter externer User
- [ ] M8 — Wetter + ÖBB
- [ ] M9 — Klara ruft für dich an

---

## Generelle Prinzipien

**AI Coding Agents richtig nutzen:** Gib immer vollständigen Kontext -- nicht "schreib mir eine Funktion", sondern "hier ist mein gesamter FastAPI Code, hier ist mein Supabase Schema, hier ist was ich erreichen will." Nach jedem größeren Change lokal testen bevor du weiter baust. Nie mehrere Milestones gleichzeitig angehen.

**System Prompt ist kein einmaliger Schritt.** Klara's Persönlichkeit, Gesprächsstil, und wie sie auf verschiedene Situationen reagiert -- das wird sich durch die ersten 20-30 Testgespräche herausschälen. Halte eine separate Notiz mit dem aktuellen System Prompt und was du geändert hast.

**Supabase ist dein Dashboard.** Bis du ein echtes Admin-Interface hast, ist Supabase's Table Editor dein Monitoring-Tool. Schau regelmäßig rein: Wurden Calls gemacht? Was steht in den Memories? Welche User sind active?

**Railway Logs sind dein bester Freund.** Wenn etwas kaputt ist, ist die Antwort fast immer in den Logs. Railway UI → Service → Logs.

**Lokal vor Railway.** Jedes Feature erst lokal mit ngrok zum Laufen bringen, dann auf Railway deployen.

---

## M0 — Setup & Accounts

**Was entsteht:** Du rufst eine Twilio-Nummer an und ein AI antwortet. Noch nicht Klara, noch keine Persönlichkeit -- aber die gesamte Infrastruktur steht.

**Geschätzter Aufwand:** 3-5 Stunden (meistens Klicken, nicht Coden)

### Was zu tun ist

**Accounts anlegen (in dieser Reihenfolge):**

OpenAI Platform Account erstellen, API Key generieren, €20 Credit aufladen. GPT-Realtime-2 ist noch nicht überall standardmäßig verfügbar -- prüfen ob der Account Zugriff hat, sonst beantragen.

Twilio Account erstellen, €20 aufladen. Eine österreichische Telefonnummer kaufen (+43 Prefix). Unter "Phone Numbers" → "Manage" → "Buy a number" → Austria suchen.

Supabase Account erstellen, neues Projekt anlegen. Region: Frankfurt (EU, wichtig für GDPR). Projekt-URL und API Key notieren.

Railway Account erstellen, GitHub Account verbinden.

Stripe Account erstellen. Im Test Mode bleiben bis zum echten Launch.

**Uber Developer Account beantragen** -- jetzt sofort, nicht später. Die Genehmigung dauert mehrere Wochen. Unter developer.uber.com beantragen.

**Lokale Entwicklungsumgebung:**

Python 3.11 installieren (pyenv empfohlen für Version Management).
ngrok installieren und Account anlegen (kostenloser Account reicht).
VS Code + Claude Code Extension installieren.

**Twilio Repo klonen:**

Das offizielle Repo `twilio-samples/speech-assistant-openai-realtime-api-python` klonen. README genau lesen. `.env.example` zu `.env` kopieren und alle Keys eintragen. Dependencies installieren.

ngrok starten auf dem Port den FastAPI nutzt. Die ngrok HTTPS URL in Twilio als Webhook für die Nummer eintragen (unter Phone Numbers → die gekaufte Nummer → Voice Configuration → Webhook URL).

Server lokal starten.

**Fertig-Bedingung:** Du rufst die Twilio-Nummer an und ein AI antwortet auf Englisch. Das Gespräch funktioniert. ngrok zeigt incoming Requests.

### Häufige Probleme

Twilio Webhook braucht zwingend HTTPS → ngrok löst das. Die ngrok URL ändert sich bei jedem Neustart (kostenloser Account) → jedes Mal in Twilio neu eintragen. OpenAI Realtime API Zugang: wenn der erste Anruf einen Authentifizierungsfehler gibt, ist der API Key nicht für Realtime freigeschaltet.

---

### Session-Log: M0 abgeschlossen 16. Juni 2026

**Bugs die aufgetreten sind:**

ngrok mit falschem Port gestartet (`ngrok http` ohne Port → lief auf Port 80 statt 5050). Fix: `ngrok http 5050`.

`.env` hatte `PUBLIC_BASE_URL=PUBLIC_BASE_URL=brewing-smell-outclass.ngrok-free.dev` statt nur die Domain ohne Präfix. Fix: nur die Domain eintragen, kein `https://` und kein doppeltes `PUBLIC_BASE_URL=`.

`APP_ENV` und `DATABASE_URL` fehlten im `.env`. Fix: `APP_ENV=local` und `DATABASE_URL=placeholder` für M0.

**Technisches:**

- Python Version: 3.9.6 (audioop funktioniert, Upgrade auf 3.11 empfohlen vor M2)
- Model string: `gpt-realtime` (verifiziert aus offiziellem Twilio Tutorial)
- Voice: `coral`
- Twilio Nummer (Trial): +1 945 288 4323
- Codex hat den Code mit Strict M0 gebaut: 18 Files, 4 Tests passing

**Qualitätsnoten:**

Audio-Qualität etwas rauschig und Latenz spürbar -- normal für M0 mit ngrok und PCMU 8kHz Codec. Verbessert sich in Production (Railway, kein ngrok).

**Für nächste Session:**

Bei jedem Neustart ngrok URL in Twilio Console aktualisieren (Voice Configuration → Webhook URL). Dann direkt M1 starten: System Prompt auf Deutsch iterieren bis Klara sich natürlich anfühlt.

---

## M1 — Klara nimmt ab

**Was entsteht:** Du rufst an, Klara stellt sich auf Deutsch vor und führt ein natürliches Gespräch. Warm, geduldig, keine roboterhafte Sprache.

**Geschätzter Aufwand:** 5-10 Stunden (meistens Iteration auf dem System Prompt)

### Was zu tun ist

**System Prompt schreiben:** Das ist der kreativste und wichtigste Schritt dieses Milestones. Klara soll sich anfühlen wie eine echte Person am Telefon. Definiere: Name (Klara), Sprache (Deutsch), Tonalität (warm, geduldig, nie gehetzt), wie sie sich beim ersten Call vorstellt, was sie fragt, wie sie reagiert wenn jemand langsam antwortet oder sich wiederholt, was sie nicht kommentiert (Versprecher, Pausen).

**Stimme wählen:** OpenAI bietet verschiedene Voices an. Teste alle auf Deutsch und wähle die wärmste, natürlichste. Frauen-Stimmen klingen für "Klara" naheliegender -- aber testen.

**Gesprächsführung definieren:** Klara soll nicht warten bis der User eine Frage stellt. Sie führt das Gespräch aktiv: fragt wie es geht, interessiert sich, stellt Nachfragen. Das muss im System Prompt verankert sein.

**Iterationsschleife:** Ruf 10-15 Mal an. Notiere nach jedem Call was unnatürlich klingt: zu schnelle Antworten, fehlende Pausen, falsche Reaktion auf Schweigen, unpassende Themen. System Prompt anpassen. Wiederholen.

**Unterbrechungen testen:** GPT-Realtime-2 kann mit Unterbrechungen umgehen -- aber es muss konfiguriert sein. Testen ob ein "Moment, kurze Frage" mitten im Satz funktioniert.

**Fertig-Bedingung:** Du rufst 10x an. Jedes Mal fühlt sich das Gespräch natürlich an. Kein Anruf unter 3 Minuten weil das Gespräch zu holprig ist. Eine andere Person (nicht du) hört eine Aufnahme und findet sie nicht roboterhaft.

### Häufige Probleme

Latenz bei komplexen Antworten: System Prompt kürzer und direktiver halten hilft. Österreichischer Akzent wird manchmal schlechter verstanden: in mehreren Tests mit verschiedenen Sprechern verifizieren.

---

## M2 — Klara erinnert sich

**Was entsteht:** Klara weiß beim zweiten Anruf wie du heißt, fragt von sich aus nach etwas das beim ersten Call erwähnt wurde, und fühlt sich nicht wie ein Reset an.

**Geschätzter Aufwand:** 8-12 Stunden

### Was zu tun ist

**Supabase Schema erstellen:**

Tabelle `users`: id, phone_number, name, call_time (Zeit des Morgenanrufs), language, preferences (JSONB -- Allergien, Lieblingsorte, Familie, etc.), active (boolean), created_at, last_called_at, missed_calls_count.

Tabelle `memories`: id, user_id (FK), call_date, full_transcript (Text), summary (Text), key_facts (JSONB -- strukturierte Infos aus dem Call), emotional_state (Text: "gut", "nachdenklich", "müde" etc.), topics (Text Array), created_at.

pgvector Extension in Supabase aktivieren (Settings → Database → Extensions). Noch nicht nutzen -- aber Schema jetzt vorbereiten damit später keine Migration nötig ist. Eine `embedding` Spalte (vector(1536)) in memories anlegen.

**Caller ID Lookup beim Inbound Call:**

Wenn ein Anruf reinkommt, liest Twilio die Absender-Nummer aus dem Webhook. Diese Nummer gegen die users Tabelle prüfen. Wenn gefunden: User-Profil und letzte 5 Memory-Einträge laden. Alles als Kontext in den System Prompt injizieren bevor Klara antwortet.

Wenn nicht gefunden: kurzer "Unbekannte Nummer" Flow -- Klara fragt höflich wer da ist und erklärt dass sie noch keinen Account für diese Nummer findet.

**Call-Ende Webhook:**

Wenn ein Call endet, sendet Twilio einen Status-Callback. In diesem Handler: das vollständige Transkript von OpenAI abrufen (OpenAI speichert Realtime Session Transcripts). GPT-4o-mini mit dem Transkript aufrufen und ein strukturiertes Memory-Objekt erstellen: Zusammenfassung in 3-5 Sätzen, key_facts als JSON (Namen, Orte, Termine, Gesundheitsstatus, Stimmung), emotional_state. Ergebnis in memories Tabelle speichern.

**Memory Injection:**

Beim nächsten Call: letzte 5 Einträge aus memories für diesen User laden. Als "Kontext aus vergangenen Gesprächen" in den System Prompt einbauen. Klara liest das nicht vor -- sie nutzt es um natürliche Anknüpfungen zu machen.

**Fertig-Bedingung:** Du rufst zweimal an. Beim zweiten Call erwähnt Klara von sich aus deinen Namen oder etwas aus dem ersten Gespräch ("Letzte Woche hast du von deinem Knie erzählt -- wie geht's dir damit?"), ohne dass du es zuerst angesprochen hast.

### Häufige Probleme

OpenAI Realtime API Transkripte: das Format wie OpenAI die Transkripte zurückgibt kann sich ändern -- direkt aus den offiziellen Docs checken. Call-Ende Timing: manchmal kommt der Status-Callback bevor das Transkript fertig ist -- kurze Verzögerung oder Retry-Logik einbauen.

---

## M3 — Klara ruft an

**Das schwierigste Milestone. Mehr Zeit einplanen als geschätzt.**

**Was entsteht:** Um eine konfigurierte Zeit klingelt dein Telefon und Klara ist dran. Mit Gedächtnis aus vorherigen Calls.

**Geschätzter Aufwand:** 12-18 Stunden, davon ein erheblicher Teil Debugging

### Was zu tun ist

**Redis als Service:**

Lokal: Redis via Docker starten (`docker run -d -p 6379:6379 redis`). Später auf Railway als separater Service.

**Celery einrichten:**

Als Python Dependency hinzufügen. Celery App Konfiguration: Broker ist Redis, Backend ist Redis. Zwei Komponenten: Celery Worker (führt Tasks aus) und Celery Beat (Scheduler der Tasks triggert).

**Scheduled Task "check_and_call":**

Diese Task läuft jede Minute. Sie fragt Supabase: welche User haben `active=true`, haben eine `call_time` die auf die aktuelle Minute fällt (Zeitzone: Europe/Vienna), und haben `last_called_at` nicht heute? Diese User werden angerufen. Nach dem Anruf: `last_called_at` auf heute setzen.

**Outbound Call via Twilio:**

Twilio kann einen Call initiieren ohne dass jemand anruft. Die Twilio REST API wird aufgerufen mit: From (Klara's Twilio-Nummer), To (User's Telefonnummer), und einer URL die den TwiML zurückgibt der die Realtime-Session startet. Das ist derselbe TwiML wie beim Inbound Call -- nur dass Twilio jetzt der Initiator ist.

Beim Outbound Call: User-Profil und Memories laden, in den System Prompt für diesen Call injizieren. Klara begrüßt aktiv: "Guten Morgen [Name], ich bin's, Klara."

**Missed Call Handling:**

Twilio gibt im Status-Callback zurück ob jemand abgehoben hat oder nicht. Wenn nicht: `missed_calls_count` in users um 1 erhöhen. Wenn ≥ 3: Flag setzen für WhatsApp-Alert (kommt im nächsten Milestone).

**Lokaler Test vor Railway:**

Celery Worker lokal starten. Celery Beat lokal starten. Eine Test-Anrufzeit auf "jetzt + 2 Minuten" setzen und warten ob das Telefon klingelt.

**Fertig-Bedingung:** Du konfigurierst 09:00 als Anrufzeit in der Datenbank. Am nächsten Morgen um 09:00 klingelt dein Handy. Klara ist dran und fragt nach dem Knie.

### Häufige Probleme

Das häufigste: Celery Worker startet aber findet Redis nicht (falsche Connection String). Oder: Celery Beat läuft, aber der Worker läuft nicht (müssen beide laufen). Oder: die Zeitzone ist falsch konfiguriert und der Anruf kommt 2 Stunden zu spät. Railway-spezifisch: zwei separate Services die sich eine Redis URL teilen müssen -- Environment Variables in beiden Services korrekt setzen.

---

## M4 — Familie bekommt WhatsApp

**Was entsteht:** Nach jedem Klara-Anruf bekommt eine konfigurierte Nummer eine kurze WhatsApp mit dem Gesprächshighlight. Bei 3 verpassten Anrufen kommt sofort ein Alert.

**Geschätzter Aufwand:** 5-8 Stunden

### Was zu tun ist

**WhatsApp Business API aktivieren:**

In Twilio Console → WhatsApp → Sandbox für erste Tests aktivieren (keine Genehmigung nötig, aber eingeschränkt). Für Production: WhatsApp Business API beantragen. Meta muss den Account genehmigen -- 1-5 Werktage.

`family_whatsapp` Feld in users Tabelle: Array von Telefonnummern im internationalen Format.

**Zusammenfassung generieren:**

Im Call-Ende Webhook: nachdem das Memory gespeichert wurde, GPT-4o-mini aufrufen mit dem Auftrag eine WhatsApp-taugliche Zusammenfassung zu schreiben. 2-3 Sätze, warm formuliert, kein medizinisches oder beunruhigendes Detail ohne dass es wirklich beunruhigend war. Beispielformat: "Klara hat heute mit [Name] gesprochen. Es geht ihr gut, sie hat von ihrem Enkel erzählt und freut sich auf das Wochenende."

Twilio WhatsApp API aufrufen: From (Klara's WhatsApp Nummer), To (jede family_whatsapp Nummer), Body (Zusammenfassung).

**Missed Call Alert:**

Separate Funktion die aufgerufen wird wenn missed_calls_count auf 3 springt. Alert-Text: "[Name] hat heute den dritten Tag in Folge nicht abgehoben. Bitte melde dich bei ihr."

**Fertig-Bedingung:** Du machst einen Testanruf. Innerhalb von 3 Minuten danach kommt auf einer zweiten Nummer eine WhatsApp mit einer sinnvollen Zusammenfassung des Gesprächs.

### Häufige Probleme

WhatsApp Template Messages: für ausgehende Nachrichten an Nummern die noch nicht mit dem Service interagiert haben braucht man genehmigte Templates. Für Tests: die Sandbox-Nummern zuerst "opt-in" lassen (sie schicken eine Nachricht an die Sandbox-Nummer). Für Production: Templates vor Launch einreichen.

---

## M5 — Alles auf Railway

**Was entsteht:** Das System läuft komplett in der Cloud. ngrok ist aus. Du kannst den Laptop zumachen und Klara funktioniert trotzdem.

**Geschätzter Aufwand:** 6-10 Stunden

### Was zu tun ist

**Railway Projekt anlegen:**

Neues Railway Projekt. Drei Services werden erstellt: FastAPI Server, Celery Worker, Redis.

**FastAPI Service:**

GitHub Repo verbinden. Railway erkennt Python automatisch. Startkommando konfigurieren: `uvicorn main:app --host 0.0.0.0 --port $PORT`. Alle Environment Variables eintragen: OpenAI Key, Twilio Account SID, Twilio Auth Token, Twilio Phone Number, Supabase URL, Supabase Key, Redis URL.

Railway gibt automatisch eine Public URL mit HTTPS. Diese URL in Twilio als Webhook eintragen (ersetzt ngrok URL).

**Celery Worker Service:**

Gleiche Codebase, anderes Startkommando: `celery -A tasks worker --loglevel=info`. Gleiche Environment Variables wie FastAPI Service. Zusätzlich: Celery Beat Startkommando als separater Prozess oder als zweiter Service -- `celery -A tasks beat --loglevel=info`.

**Redis Service:**

Railway hat ein Redis Plugin. Einmal klicken, Railway gibt eine interne Redis URL. Diese URL als REDIS_URL Environment Variable in FastAPI und Celery Worker eintragen.

**Supabase Production:**

Supabase Connection String auf den Production-Modus umstellen (Connection Pooling aktivieren für bessere Performance unter Last).

**Fertig-Bedingung:** ngrok ist auf deinem Computer aus. Du rufst die Twilio-Nummer an. Klara antwortet. Am nächsten Morgen kommt der Morgenanruf. WhatsApp funktioniert. Alles läuft ohne dein Zutun.

### Häufige Probleme

Environment Variables vergessen: die häufigste Fehlerquelle. Checke Railway Logs auf "KeyError" oder "None" Fehler direkt nach dem Deploy. Celery Worker findet Tasks nicht: sicherstellen dass der Worker dasselbe `tasks` Modul importiert wie Beat. Railway Cold Starts: kostenloser Railway Plan hat "Sleep" -- Upgrade auf Hobby Plan ($5/Monat) für always-on.

---

## M6 — Erster externer User kann sich anmelden

**Was entsteht:** Eine komplett unbekannte Person kann über eine Website Klara für ihre Mutter aktivieren und dafür bezahlen. Ohne dass du manuell eingreifst.

**Geschätzter Aufwand:** 10-15 Stunden

### Was zu tun ist

**Next.js Projekt erstellen:**

`npx create-next-app klara-onboarding`. Deployment auf Vercel (kostenlos, automatisch von GitHub).

**Onboarding Formular:**

Seite 1 -- Wen richtest du Klara ein für?: Name der Person, Telefonnummer (+43/+49/+41 Prefix), bevorzugte Anrufzeit (Dropdown: 08:00, 09:00, 10:00...), Sprache (Deutsch).

Seite 2 -- Wer soll informiert werden?: Bis zu 3 WhatsApp-Nummern von Familienmitgliedern mit Namen.

Seite 3 -- Über Klara: kurze Erklärung was passiert (Klara ruft täglich an, Family bekommt Zusammenfassung, erste 14 Tage gratis).

**Stripe Integration:**

Stripe Checkout Session erstellen wenn User auf "Weiter zu Zahlung" klickt. Preis: €39/Monat (oder €24 für Companion Tier). Nach erfolgreichem Payment: Stripe redirected zurück zur Website mit einer Session ID.

Stripe Webhook: wenn `customer.subscription.created` Event eintrifft, User-Datensatz in Supabase anlegen mit allen Formulardaten. `active=true` setzen. `call_time` aus Formular übernehmen.

**Confirmation:**

Nach erfolgreichem Signup: WhatsApp an die Familienmitglieder "Klara wurde eingerichtet. Der erste Anruf bei [Name] kommt morgen um [Uhrzeit]."

**Fertig-Bedingung:** Du gehst als Test-User durch den gesamten Flow. Trägst eine Test-Kreditkarte ein (Stripe Test Mode: 4242 4242 4242 4242). Findest dich danach in Supabase als active User. Bekommst am nächsten Morgen einen Anruf.

### Häufige Probleme

Stripe Webhook Signature: muss korrekt verifiziert werden (Security). Stripe's Docs dazu sind gut -- AI Coding Agent kann das korrekt generieren wenn du auf die Security-Anforderung hinweist. Telefonnummer-Validierung: internationale Formate sind uneinheitlich -- libphonenumber Library nutzen.

---

## M7 — Erster echter externer User

**Kein Code. Wichtigstes Milestone.**

**Was entsteht:** Eine Person die du kennst nutzt Klara 10+ Tage ohne dass du eingreifen musst.

### Was zu tun ist

**Person finden:**

Idealer erster User: Elternteil eines engen Freundes, älterer Verwandter, oder eine Person aus deinem Netzwerk die du persönlich fragen kannst. Nicht kalt anfragen -- das funktioniert für so ein persönliches Produkt nicht.

**Persönlich erklären:**

Kein Tech-Jargon. Einfach: "Klara ruft täglich an, fragt wie es geht, erinnert sich an alles, und deine Kinder kriegen eine kurze Nachricht danach. Willst du das 2 Wochen gratis testen?"

**Manuell onboarden:**

Sitz mit der Person oder mit der Familie zusammen und füll das Formular gemeinsam aus. Erkläre was beim ersten Anruf passiert.

**Beobachten:**

Täglich in Supabase schauen: Haben sie abgehoben? Wie lang war das Gespräch? Was steht in den Memories? Gab es Fehler?

Nach einer Woche: kurzes Feedback-Gespräch mit der Person oder der Familie. Was funktioniert gut? Was fehlt?

**Fertig-Bedingung:** Person nutzt Klara 10+ Tage. Du musstest nicht einmal manuell eingreifen. Du hast konkrete Rückmeldungen was gut und was schlecht ist.

---

## M8 — Wetter + ÖBB

**Was entsteht:** Klara kann Wetterfragen beantworten und Zugverbindungen nachschlagen. Erste echte Tool-Calls.

**Geschätzter Aufwand:** 6-8 Stunden

### Was zu tun ist

**OpenAI Tool Use konfigurieren:**

GPT-Realtime-2 unterstützt Function Calling während des Gesprächs. Zwei Tools definieren: `get_weather` (Parameter: city, date) und `get_train_connection` (Parameter: from_city, to_city, datetime).

**OpenMeteo API:**

Kostenlos, kein API Key nötig. REST Call mit Stadtname und Datum gibt Temperatur, Wetterbeschreibung, Niederschlagswahrscheinlichkeit zurück. Klara fügt Kleidungsempfehlung als Layer hinzu ("Bei 12 Grad und Regen würde ich eine Jacke empfehlen").

**ÖBB Scotty API:**

Öffentliche REST API, kostenlos, kein Auth nötig. Call mit Abfahrtsort, Ziel, und gewünschter Zeit gibt Verbindungen mit Zeiten und Umstiegen zurück. Klara liest die erste sinnvolle Verbindung vor.

**Intent Detection:**

GPT-Realtime-2 entscheidet selbst wann es ein Tool aufruft -- das ist der Vorteil von Function Calling. Klara hört "Wie wird das Wetter morgen?" und ruft automatisch `get_weather` auf. Keine separate Intent-Klassifikation nötig.

**Fertig-Bedingung:** Du rufst an und fragst nach dem Wetter morgen in Wien und nach dem nächsten Zug nach Salzburg. Beides wird korrekt beantwortet mit echten Daten.

---

## M9 — Klara ruft für dich an

**Was entsteht:** Klara macht im Auftrag des Users einen Outbound Call zu einem Restaurant oder einer Arztpraxis und meldet das Ergebnis zurück.

**Geschätzter Aufwand:** 8-15 Stunden

### Was zu tun ist

**Confirmation Flow:**

Bevor Klara irgendwo anruft, muss der User explizit bestätigen. Flow: User sagt was er will → Klara wiederholt den Plan ("Also soll ich beim Restaurant Mochi anrufen und einen Tisch für 2 Personen am Freitag um 20 Uhr reservieren?") → User bestätigt → Klara sagt "Ich ruf kurz an und melde mich dann bei dir per WhatsApp" → Call endet.

**Outbound Call initiieren:**

Nach Ende des User-Calls: Celery Task erstellt den Outbound Call zu der Zielnummer. Wichtig: dieser Outbound Call braucht einen komplett anderen System Prompt als der User-Call. Klara im Restaurant-Modus stellt sich vor als "Klara, ich rufe im Auftrag von [Username] an", erklärt den Wunsch, und handelt den Termin ab.

**Ergebnis zurückmelden:**

Nach Ende des Restaurant-Calls: Zusammenfassung per WhatsApp an User ("Tisch für 2 am Freitag um 20 Uhr beim Mochi reserviert. Auf deinen Namen.") und an Family-Nummern wenn gewünscht.

**Fehlerfall:**

Wenn das Restaurant besetzt ist, nicht abnimmt, oder keinen Tisch hat: WhatsApp mit Statusmeldung und Alternativvorschlag wenn möglich.

**Fertig-Bedingung:** Du bittest Klara einen Tisch zu reservieren. Sie bestätigt den Plan, der Call endet, 5 Minuten später bekommst du eine WhatsApp mit dem Ergebnis. Du rufst das Restaurant an und bestätigst dass die Reservierung wirklich gemacht wurde.

### Häufige Probleme

Zwei verschiedene Persönlichkeiten (User-Modus vs. Restaurant-Modus) brauchen zwei verschiedene System Prompts. Das Management welcher Prompt für welchen Call gilt muss sauber gelöst sein. Busy Signal / kein Abnehmen: Retry-Logik -- wie oft versucht Klara es nochmal?

---

## Phase 2+ Features (nach M7)

Reihenfolge nach echtem Nutzerbedarf -- nicht nach eigener Präferenz. Was fragen erste User wirklich? Das hat Priorität.

**Einfach (je 3-6 Stunden):**
- Caller ID Fraudcheck (Tellows API, kostenlos)
- Spaziergang Check-in Timer (Celery Delayed Task)
- Gesundheitsmetrik-Tagebuch (nur Supabase, kein API)
- Pakettracking (DHL/Austrian Post API)
- Ausgaben-Tracking per Sprache (nur Supabase)
- Tägliches Rätsel / Wissens-Gespräch (nur GPT)
- Project Gutenberg Bücher vorlesen

**Mittel (je 6-12 Stunden):**
- Uber API Booking (nach Developer Approval)
- WhatsApp Proxy -- eingehende Nachrichten vorlesen
- Voice-only Kalender (Supabase + Celery Reminder)
- Google Places für lokale Services und Apothekennotdienst

**Komplex (je 12-20+ Stunden):**
- Familien-Dashboard (größeres Next.js Projekt mit Auth)
- pgvector Semantic Search für Langzeit-Memory
- Lebensgeschichte-Archiv (strukturiertes Langzeit-Gedächtnis)
- B2B Onboarding Flow für Seniorenresidenzen

---

## Frühe APIs die jetzt beantragt werden müssen

Einige APIs brauchen manuelle Genehmigung die Wochen dauern kann.

| API | Wofür | Beantragen wann |
|---|---|---|
| Uber Developer Access | Taxi bestellen | Sofort, jetzt |
| WhatsApp Business API | Ausgehende Nachrichten | Milestone M4 Beginn |
| OpenAI Realtime API | Voice | Prüfen ob Account Zugang hat |

---

## Kosten im laufenden Betrieb

Während Development (vor ersten zahlenden Usern):

| Service | Kosten/Monat |
|---|---|
| Twilio (Tests) | ~€10-20 |
| OpenAI API | ~€20-40 |
| Railway (Hobby Plan) | $5 |
| Supabase | Kostenlos bis 500MB |
| Stripe | Kostenlos im Test Mode |
| **Gesamt** | **~€35-65** |

---

## Related
- [[Klara]]
- [[02-Knowledge/Pre-Seed-Fundraising]]
