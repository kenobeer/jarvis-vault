---
created: 2026-06-25
status: building
tags:
  - friday
  - ops-agent
  - productivity
  - ai-agents
  - automation
type: spec
---
# Friday, Build-Spec

> Persönlicher Ops-/Sparring-Agent. Design-Kontext. Lauffähiges Gerüst im Repo `friday/`. Status: Fundament gebaut, Engine + Usability-Fixes offen.
>
> **Name:** Friday. **API-Stand:** offizielle TickTick-Doku ausgewertet. **Usability-Fixes:** unten als feste Anforderungen, von Anfang an einzubauen.

---

## Zweck

1. **Mechanik:** jeder Task eine konkrete nächste Aktion, energie-getaggt (`#deep`/`#admin`/`#call`), Zustand klar (`#waiting` mit Owner+Datum, `#parked` mit Grund). Alles in TickTick, dort abhakbar.
2. **Urteil:** Disconnects zwischen Strategie (Vault) und Handeln (TickTick) aufdecken. Wo „dringend" real geparkt ist, wo bequeme Solo-Arbeit höhere Hebel verdrängt.

Harte Anforderungen: kommt zu mir (Telegram-Push), verliert nie Kontext, alles in TickTick, **und ich kann ihm unterwegs Infos zuwerfen, die er in Tasks und Notes verwandelt.**

---

## Primäre Oberfläche: TickTick

Friday hält in TickTick aktuell: Energie-Tags, Status-Tags, Fälligkeit, Priorität, und das „Warum" im content-Feld. Menü = native TickTick-Ansicht (Filter Deep/Admin/Call + Ausblend-Filter für waiting/parked). Vault hält Strategie und Wochen-Review.

---

## TickTick-API (geklärt)

Basis `https://api.ticktick.com/open/v1`, OAuth2 `tasks:read tasks:write`.
- `GET /project`, `GET /project/{id}/data`; `POST /task/filter` (Projekt/Tag/Prio/Status/Datum, Datum auf **startDate**); `POST /task/completed`; create/update/complete/delete/move; Habits + Focus.
- Nur `authorization_code`-Grant, kein Refresh, Tokens langlebig, bei Ablauf manuell neu.
- Update sendet volles Objekt und ersetzt Tags → `set_tags()` merged, manuelle Tags bleiben.

---

## Datenfluss (inkl. Capture)

- **Geplant täglich (07:30):** Tasks lesen, Disconnect-Scan, Tags aktuell halten, fällige waiting/parked wiedervorlegen, **Heartbeat-Nachricht immer** (auch wenn nichts ansteht). Menü lese ich in TickTick.
- **Geplant wöchentlich (So 18:00):** Projekt-Notes gegen TickTick abgleichen, Reconciliation, Wochen-Narrativ nach `Ops/Weekly/`.
- **Inbound sofort (Webhook), zwei Typen:**
  - **Antwort auf eine offene Frage** → gegen die Frage interpretieren, nach Bestätigung schreiben.
  - **Unaufgeforderte Capture** (Text ODER Sprachnachricht) → transkribieren, parsen, Vorschlag zurück („Waiting-For Designerin, Nachfass nächste Woche; neuer Waiting-Eintrag Vertrag-Lutz, Nachfass Fr. Übernehmen?"). Nach Tap: **Task nach TickTick**, Notiz nach `Ops/capture-log.md`. Niemals in Projekt-Notes (Disjunkt-Regel).

---

## Schreibwege und Konfliktregel

- **Friday → TickTick:** Tasks, Tags, content, Fälligkeit, Priorität. Vor jedem Schreiben Task frisch lesen, 404 sauber abfangen (out-of-band erledigt/gelöscht).
- **Friday → Vault:** nur `Ops/` (capture-log, Weekly, state, README). **Eine kontrollierte Ausnahme:** auf explizite Bestätigung darf Friday das `status`-Frontmatter EINER Projekt-Note umschreiben (Frontmatter-Sync, siehe Fix 2), per pull-rebase, selten, ein Feld.
- **Claude Desktop → Vault:** Projekt-Notes (`03-Projects`), Inbox, Ideas. Disjunkt von `Ops/`, also kein Git-Konflikt mit Friday.

---

## Was in `Ops/` landet

Agent schreibt alles in `Ops/` außer `config.md` (nur ich). `README.md`, `config.md` (Mapping Vault↔TickTick-Projekte **+ Personen→Projekt-Map** für Capture-Resolution), `state.md` (Spiegel aus DB), `capture-log.md` (Inbound-Captures), `Weekly/<jahr-woche>.md`.

---

## State (Postgres)

`tasks_cache`, `waiting_for`, `parked`, `questions`, `runs_log` (auch als Undo-/Audit-Trail jeder Friday-Änderung). Spiegel in `Ops/state.md`.

---

## Usability-Fixes (feste Anforderungen, von Anfang an)

1. **Capture-Funktion (größte Lücke).** Webhook nimmt unaufgeforderte Nachrichten als Capture an, inkl. **Telegram-Sprachnachricht + Speech-to-Text** (ich diktiere unterwegs). Parst zu Task/Waiting/Parked, schlägt vor, schreibt nach Bestätigung in TickTick + `Ops/capture-log.md`.
2. **Frontmatter-Sync-Schleife.** Disconnect-Erkennung hängt am `status`-Frontmatter. Claude-Desktop-Edits ziehen es oft nicht mit → Friday nörgelt über Scheindisconnects. Fix: bei bestätigtem Status-Wechsel schreibt Friday das Frontmatter selbst (kontrollierte Ausnahme oben). Schließt die Schleife, eine Wahrheit.
3. **Lese-Scope vs Speicher-Ort.** obsidian-router speichert oft nach `00-Inbox`/`01-Ideas`, Friday liest nur `03-Projects`. Entweder Friday liest auch Inbox/Ideas für Projektbezug, oder Projekt-Erkenntnisse müssen nach `03-Projects`. In `config.md` festlegen, welche Ordner Friday scannt.
4. **Heartbeat.** Täglich immer eine Nachricht, auch „nichts Dringendes, 12 offene Tasks". Damit heißt Stille immer Fehler (Token abgelaufen, Railway tot), nie Entwarnung.
5. **Keine Bestätigungs-Müdigkeit.** Energie-Tags nach einmaliger Freigabe komplett auto. Bestätigungen gebündelt („diese 6 Änderungen übernehmen?"). Fragen hart auf 3/Tag gedeckelt. Friday erfindet nie Engagement.
6. **„Heute" exakt definieren.** Filter ist auf startDate, Fälligkeit auf dueDate, plus All-Day und Überfälliges. In Phase 3 gegen echte Daten festnageln, bevor dem Menü vertraut wird. Falsches Menü ist schlimmer als keins.
7. **Out-of-band-Erledigung.** Vor jedem Schreiben Task frisch lesen, 404/erledigt abfangen, dann still überspringen statt failen.
8. **Bootstrap in Häppchen.** Erster Wochenlauf nicht als 40-Vorschläge-Block, sondern projektweise oder in Fünfer-Batches.
9. **Undo/Audit.** Jede Friday-Änderung in `runs_log`, mindestens nachvollziehbar, idealerweise „letzte Aktion rückgängig" per Befehl.
10. **Privacy-Bewusstsein.** Sensibles (Bitschnau, Finanzen) fließt über Telegram (nicht E2E) und die API. Kein Drama, aber gescoptes Senden im Wochenpass und Bewusstsein.

---

## Repo-Stand (`friday/`)

Gebaut: Gerüst, Config, Modelle, FastAPI-Skeleton, **vollständiger TickTick-Client**, Protokolle, Vault-Bridge-Skeleton, Scheduler, OAuth-Helper. Offen: Engine-Logik, die 10 Fixes oben, Vault-Bridge live, Deploy.

---

## Related
- [[Friday-Umsetzungsplan]]
- [[03-Projects/Klara/Klara]]
- Repo: `friday/`
