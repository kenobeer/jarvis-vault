---
created: 2026-06-25
status: active
tags:
  - friday
  - ops-agent
  - runbook
  - automation
type: runbook
---
# Friday, Umsetzungsplan

> Schritt-für-Schritt-Runbook mit den Usability-Fixes direkt in den Phasen verankert. Architektur in [[Friday-Spec]], Gerüst im Repo `friday/`.

---

## Entscheidung: weiterbauen, nicht neu anfangen

Vorhandenes Repo als Basis in Claude Code öffnen (`~/dev/friday`), Engine + Fixes füllen, gegen echte Daten testen. Start bei ~40%.

## Wie du in Claude Code startest

```
mkdir -p ~/dev/friday && cd ~/dev/friday
# friday.zip entpacken, README.md im aktuellen Ordner
git init
claude
```
Erster Prompt:
> Lies README.md und Friday-Spec. Arbeite nach Friday-Umsetzungsplan. Bring mich durch Phase 0 und 1, erklär jeden Schritt kurz. Die 10 Usability-Fixes aus der Spec bauen wir in den markierten Phasen mit ein.

## Sicherheits-Grundregel

Secrets nur in `.env` (gitignored) und Railway-Env. Nie in Commits, nie in den Chat pasten.

---

## Phasen

### Phase 0 — Lokales Setup · [Admin, ~20 Min]
**Ziel:** Friday bootet lokal.
1. Entpacken, `cd ~/dev/friday`. 2. `python -m venv .venv && source .venv/bin/activate`. 3. `pip install -r requirements.txt`. 4. `cp .env.example .env`. 5. `uvicorn app.main:app --reload`.
**Verifikation:** `curl localhost:8000/` → `friday up`.

### Phase 1 — TickTick · [Admin + Deep, ~30-45 Min]
**Ziel:** echte Tasks lesen/schreiben, Merge-Gotcha verifiziert.
1. developer.ticktick.com → Manage Apps → App, Redirect `http://127.0.0.1:8765/callback`, `client_id`+`secret` in `.env`.
2. `python scripts/get_ticktick_token.py` → `access_token` in `.env`.
3. Projekte listen (Read-Test).
4. **Merge-Experiment:** Wegwerf-Task mit manuellem Tag → `set_tags(task, energy="deep")` → neu lesen.
5. **Fix 7 gleich mitbauen:** Helper „Task frisch lesen vor Schreiben, 404 abfangen".
**Verifikation:** echte Projektnamen erscheinen; nach Schritt 4 hat der Task `#deep` UND den manuellen Tag; ein Schreibversuch auf eine gelöschte Task-ID failt nicht, sondern wird sauber übersprungen.

### Phase 2 — Telegram · [Admin, ~15 Min]
**Ziel:** Friday kann senden und Sprachnachrichten empfangen.
1. @BotFather → `/newbot` → Token. 2. Chat-ID via getUpdates oder @userinfobot. 3. `.env`: Token, Chat-ID, selbst gewähltes Secret.
4. **Vorbereitung Fix 1:** prüfen, dass `voice`-Nachrichten im Webhook ankommen (file_id), STT-Weg wählen (z.B. Whisper-API).
**Verifikation:** `send_message("Friday lebt")` kommt an; eine Sprachnachricht an den Bot liefert eine `voice.file_id` im Update.

### Phase 3 — Engine-Logik + Kern-Fixes · [Deep, 2-3 Blöcke]
**Ziel:** echter Tagespass + die Fixes, die über Vertrauen entscheiden.
1. Daily-Flow: Tasks → Modell → Vorschläge → Telegram.
2. **Fix 6 „Heute":** Definition von heute exakt gegen echte Daten festnageln (dueDate + All-Day + überfällig), nicht nur startDate.
3. **Fix 4 Heartbeat:** täglich immer eine Nachricht, auch wenn leer.
4. **Fix 5 Anti-Müdigkeit:** Energie-Tags auto nach einmaliger Freigabe; Bestätigungen bündeln; Fragen-Cap 3/Tag.
5. **Fix 9 Audit/Undo:** jede Änderung in `runs_log`; `/undo` für die letzte Aktion.
6. `handle_reply` (Antwort auf Frage) bauen.
**Verifikation:** `POST /run/daily` liefert eine sinnvolle Nachricht; an einem leeren Tag kommt trotzdem der Heartbeat; sechs Tag-Änderungen kommen als ein Bündel; `/undo` macht die letzte rückgängig.

### Phase 4 — Capture (Inbound unterwegs) · [Deep, 1-2 Blöcke]
**Ziel:** dein wichtigster Realfall. Du wirfst Friday unterwegs Infos zu, er macht Tasks und Notes daraus.
1. Webhook: zwischen „Antwort auf offene Frage" und „neue Capture" unterscheiden.
2. **Sprachnachricht → STT** (Fix 1), dann parsen.
3. **Fix 8 Entity-Resolution:** Personen→Projekt-Map in `Ops/config.md`; „Designerin" auflösen, bei Mehrdeutigkeit nachfragen.
4. Vorschlag zurück, nach Tap: Task nach TickTick, Notiz nach `Ops/capture-log.md`. **Nie in Projekt-Notes.**
**Verifikation:** Sprachnachricht „Designerin verschiebt auf nächste Woche, Lutz schickt Vertrag Freitag" erzeugt einen sauberen Vorschlag; nach Bestätigung existieren die Tasks in TickTick und der Log-Eintrag in `Ops/capture-log.md`.

### Phase 5 — Vault-Bridge + Sync-Fixes · [Admin + Deep, ~45-60 Min]
**Ziel:** Strategie lesen, sauber zurückschreiben, Schleife geschlossen.
1. Privates GitHub-Repo für den Vault, Obsidian-Git am Mac, Auto-Push ~10 Min, initialer Push.
2. Im Vault `Ops/` + `Ops/config.md` anlegen (Projekt-Mapping + **Lese-Scope-Ordner**, Fix 3).
3. **Fix 2 Frontmatter-Sync:** Friday schreibt auf Bestätigung das `status`-Frontmatter EINER Projekt-Note (kontrollierte Ausnahme, pull-rebase).
4. `.env`/Railway: `VAULT_REPO_URL` + GitHub-Token.
**Verifikation:** `vault.pull()` liest Notes aus den konfigurierten Ordnern; bestätigtes „Klara aktiv" setzt das Frontmatter auf `status: active` und Friday flaggt den Disconnect danach nicht mehr.

### Phase 6 — Railway-Deploy · [Admin, ~30 Min]
1. `friday`-Repo zu eigenem GitHub-Repo (getrennt vom Vault-Repo). 2. Railway-Projekt + Postgres. 3. Env-Variablen setzen. 4. Deploy, Health prüfen. 5. Webhook setzen:
`curl "https://api.telegram.org/bot<TOKEN>/setWebhook?url=https://<app>/telegram/webhook&secret_token=<SECRET>"`
**Verifikation:** Health antwortet; eine Telegram-Nachricht (Text und Voice) löst den Webhook aus (Railway-Logs).

### Phase 7 — Go-Live + Bootstrap · [Deep, 1 Block + laufend]
1. TickTick-Filter Deep/Admin/Call + Ausblend-Filter für waiting/parked.
2. **Fix 8 Bootstrap in Häppchen:** Bestand projektweise oder in Fünfer-Batches taggen/schärfen, nicht alles auf einmal.
3. Eine Woche beobachten, Prompts justieren.
**Verifikation:** morgens Menü, Tags bleiben aktuell, Sonntag Review, Bootstrap kam verdaulich in Batches.

---

## Reihenfolge der Fixes (wenn Zeit knapp)

Zuerst Capture inkl. Voice (Phase 4), größte fehlende Fähigkeit und dein wichtigster Realfall. Dann Frontmatter-Sync (Phase 5, Fix 2), sonst frisst Scheindisconnect Vertrauen. Dann Heartbeat + gebündelte Bestätigungen (Phase 3, Fix 4+5), die entscheiden, ob du das System nach zwei Wochen noch ernst nimmst. Der Rest ist Härtung im Live-Betrieb.

## Definition of Done

Friday läuft auf Railway, täglich Menü + Heartbeat, Sonntag Review, hält Tags in TickTick aktuell, nimmt unterwegs Captures per Text und Voice an, schließt die Frontmatter-Schleife, bündelt Bestätigungen, und ändert nichts ohne Tap.

---

## Related
- [[Friday-Spec]]
- Repo: `friday/`

---

## Stand 2026-06-26

**Session mit Claude Code (26.6.):**

- ✅ Phase 0 abgeschlossen: lokales Setup, `.venv`, `requirements.txt`, FastAPI bootet
- ✅ Phase 1 Schritte 1–3: TickTick OAuth-App registriert, `client_id` + `secret` + `access_token` in `.env`, Live-Read erfolgreich (5 Projekte: Work, Personal, Exercise, Uni, Shopping)
- ✅ Phase 2 Schritte 1–3: BotFather-Bot erstellt, `TELEGRAM_BOT_TOKEN` + `TELEGRAM_CHAT_ID` + `TELEGRAM_SECRET` in `.env`, `send_message` kommt an
- ✅ GitHub-Vault-Sync eingerichtet (Obsidian-Git)

---

## Stand 2026-06-30

**Phasenstatus:**
- ✅ Phase 0 — Lokales Setup: abgeschlossen
- ✅ Phase 1 — TickTick OAuth: abgeschlossen, Tags funktionieren
- ✅ Phase 2 — Telegram: abgeschlossen, Nachrichten kommen an
- ✅ Phase 3 — Engine-Logik: tägliche Nachrichten laufen seit Deployment, Fragen + Tap-Bestätigung funktionieren
- ✅ Phase 4 — Capture: deployed. `CAPTURE_ENTITY_MAP` gesetzt (`Micha:Bitschnau,Luca:Bitschnau,Lutz:G&G`). Noch nicht getestet.
- ✅ Phase 5 — Vault-Bridge: Code fertig, `VAULT_REPO_URL` mit Token in Railway, `nixpacks.toml` deployed (git-Fix). Weekly-Run läuft erst nächsten Sonntag — noch nicht verifiziert.
- ✅ Phase 6 — Railway-Deploy: läuft stabil, tägliche Nachrichten kommen jeden Morgen an
- ⬜ Phase 7 — Go-Live + Bootstrap: TickTick-Filter anlegen, Bestand-Tasks taggen

**TickTick-Projekte (exakte Namen):** `Uni`, `Klara`, `Bitschnau`, `G&G`, `Work`

**Nächste Session:** Fokus auf Usability — Telegram-Nachrichtenformat überarbeiten (Struktur, Länge, Lesbarkeit der täglichen Nachrichten und Capture-Bestätigungen). Dafür: `app/engine/protocols.py` (Prompts) und `app/engine/runner.py` (Formatting-Logik).
