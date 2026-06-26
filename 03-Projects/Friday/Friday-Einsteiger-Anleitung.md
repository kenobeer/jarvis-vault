---
created: 2026-06-25
status: active
tags:
  - friday
  - ops-agent
  - anleitung
  - einsteiger
  - automation
type: guide
---
# Friday, Einsteiger-Anleitung

> Sehr ausführliche Schritt-für-Schritt-Erklärung für jemanden, der keine Coding-Erfahrung hat. Begleitet [[Friday-Umsetzungsplan]] (die Kurzfassung) und [[Friday-Spec]] (das Warum). Wenn etwas unklar ist: den Fehler oder die Frage einfach Claude Code pasten.

---

## Das Wichtigste zuerst, zur Beruhigung

Du musst den Code **nicht** selbst schreiben oder verstehen. Das macht Claude Code für dich, ein Programm, das in deinem Terminal lebt und auf Anweisung Dateien schreibt, Befehle ausführt und Fehler erklärt. Deine Aufgabe ist zweigeteilt:

1. **Klick-Aufgaben:** ein paar Accounts und Schlüssel erstellen (TickTick, Telegram, GitHub, Railway). Das kann dir niemand abnehmen, weil es deine Konten sind. Diese erkläre ich unten Klick für Klick.
2. **Code-Aufgaben:** alles im Terminal. Hier gibst du Claude Code einen Satz, es macht den Rest, und du schaust nur, dass am Ende „funktioniert" dasteht. Die genauen Sätze gebe ich dir.

Plane nicht alles an einem Tag. Teil A und B sind ein ruhiger Admin-Block, Teil C braucht ein, zwei fokussierte Blöcke.

---

## Mini-Glossar (einmal lesen, dann vergessen)

- **Terminal:** das schwarze Textfenster auf dem Mac, in das man Befehle tippt. Öffnen: Cmd+Leertaste, „Terminal" tippen, Enter.
- **Befehl:** eine Zeile Text, die du ins Terminal tippst und mit Enter ausführst.
- **Ordner / Pfad:** wo Dateien liegen, z.B. `~/dev/friday`. Das `~` heißt „dein Benutzerordner".
- **Python:** die Programmiersprache, in der Friday geschrieben ist. Muss auf dem Mac sein, damit Friday lokal läuft.
- **Token / Secret / Key:** ein langes Passwort, mit dem sich Programme bei Diensten anmelden. Geheim halten.
- **.env-Datei:** eine Textdatei im Projekt, in der diese geheimen Schlüssel stehen. Wird nie ins Internet hochgeladen.
- **API:** die Schnittstelle, über die Friday mit TickTick und Telegram „spricht".
- **Webhook:** eine Adresse, an die Telegram deine Nachrichten schickt, damit Friday sofort reagiert.
- **GitHub / Repo:** ein Online-Speicher für Code (und für deinen Vault). Friday wird dort liegen.
- **Railway:** der Cloud-Dienst, auf dem Friday dauerhaft läuft, auch wenn dein Laptop aus ist.

---

## Teil A — Einmal-Vorbereitung (Admin, ~20 Min)

### A1. Claude Code installieren
1. Terminal öffnen (Cmd+Leertaste, „Terminal", Enter).
2. Diesen Befehl reinkopieren und Enter:
   ```
   curl -fsSL https://claude.ai/install.sh | bash
   ```
   Das ist der offizielle Installer von Anthropic, er braucht nichts weiter.
3. Terminal einmal **schließen und neu öffnen** (damit der Mac den neuen Befehl kennt).
4. Tippen: `claude --version`. Wenn eine Versionsnummer erscheint, hat es geklappt.

Hinweis: Claude Code braucht ein bezahltes Claude-Abo (Pro oder höher). Der kostenlose Plan reicht nicht.

### A2. Das Friday-Repo ablegen
1. Die Datei `friday.zip` (aus unserem Chat) liegt vermutlich in deinem Downloads-Ordner. Doppelklick darauf, dann entsteht ein Ordner `friday`.
2. Diesen Ordner an einen festen Platz legen. Vorschlag im Terminal:
   ```
   mkdir -p ~/dev
   mv ~/Downloads/friday ~/dev/friday
   ```
3. In den Ordner wechseln:
   ```
   cd ~/dev/friday
   ```

### A3. Claude Code starten und übergeben
1. Tippen: `claude` (du bist ja im Ordner `~/dev/friday`).
2. Beim ersten Start öffnet sich der Browser zum Einloggen. Einloggen, fertig.
3. Ersten Satz an Claude Code schreiben:
   > Lies die README.md in diesem Ordner. Ich bin Anfänger ohne Coding-Erfahrung. Wir bauen Friday nach der Anleitung Schritt für Schritt. Fang mit Phase 0 an (Python, Dependencies, Health-Check) und erklär mir jeden Schritt in einfachen Worten. Mach kleine Schritte und warte nach jedem auf mein OK.

Ab hier macht Claude Code die Terminal-Arbeit. Wenn es dir einen Befehl vorschlägt, lässt du ihn laufen. Wenn ein Fehler kommt, kopierst du den ganzen Fehlertext zurück an Claude Code und sagst „dieser Fehler kam, was tun?".

---

## Teil B — Die vier Klick-Aufgaben (Accounts und Schlüssel)

Diese machst du selbst im Browser. Schreib dir jeden Schlüssel in eine sichere Notiz, du brauchst sie gleich für die `.env`. Claude Code kann dir beim Eintragen helfen, sehen darf es die Schlüssel, aber poste sie nie in einen öffentlichen Ort.

### B1. TickTick-App (damit Friday deine Tasks lesen und schreiben darf)
1. Auf **developer.ticktick.com** mit deinem normalen TickTick-Login anmelden.
2. Den Bereich „Manage Apps" öffnen und eine neue App anlegen (Name frei, z.B. „Friday").
3. Bei der App eine **Redirect URI** eintragen, exakt: `http://127.0.0.1:8765/callback`
4. Du bekommst eine **Client ID** und ein **Client Secret**. Beide kopieren und sicher notieren.
5. Das eigentliche Zugriffs-Token holst du später mit einem fertigen Hilfsskript, das im Projekt liegt. Claude Code führt dich da durch (Phase 1). Du öffnest dabei einmal einen Link im Browser, klickst „erlauben", und kopierst einen Code zurück ins Terminal.

Merke dir: Client ID, Client Secret. (Das Access-Token kommt in Phase 1.)

### B2. Telegram-Bot (damit Friday dir schreiben kann)
1. In Telegram den Kontakt **@BotFather** suchen und öffnen.
2. Schreib ihm: `/newbot`
3. Er fragt nach einem Namen (frei, z.B. „Friday") und einem Benutzernamen (muss auf `bot` enden, z.B. `yona_friday_bot`).
4. Er antwortet mit einem **Bot-Token** (lange Zeichenkette mit Doppelpunkt). Kopieren und sicher notieren.
5. Deine **Chat-ID** holen: such in Telegram den Bot **@userinfobot**, starte ihn, er zeigt dir deine numerische ID. Notieren.
6. Denk dir noch ein eigenes **Webhook-Secret** aus (irgendein Zufallswort, z.B. `friday-7x9k`). Notieren.

Merke dir: Bot-Token, Chat-ID, Webhook-Secret.

### B3. GitHub-Repos (Speicher für Friday und für deinen Vault)
Du brauchst zwei private Repos. „Privat" heißt, nur du siehst sie.
1. Auf **github.com** einloggen (oder kostenlos registrieren).
2. Repo eins anlegen, Name z.B. `friday`, sichtbar auf **Private** stellen. Das wird Fridays Code-Speicher.
3. Repo zwei anlegen, Name z.B. `jarvis-vault`, ebenfalls **Private**. Da wird dein Obsidian-Vault gespiegelt.
4. Ein **Zugriffs-Token** erstellen, damit Friday in der Cloud den Vault lesen darf: in den GitHub-Einstellungen unter „Developer settings" ein „Personal access token" anlegen, mit Repo-Zugriff. Kopieren und notieren.
5. **Obsidian mit dem Vault-Repo verbinden:** in Obsidian die Community-Plugins öffnen, das Plugin **Obsidian Git** installieren und aktivieren, in dessen Einstellungen das Repo `jarvis-vault` als Ziel setzen und „Auto push" auf z.B. alle 10 Minuten stellen. Einmal manuell „commit and push" auslösen, damit dein Vault zum ersten Mal hochgeht.

Merke dir: die beiden Repo-Adressen, das GitHub-Token. (Claude Code hilft beim Verbinden, wenn du nicht weiterkommst.)

### B4. Railway (damit Friday dauerhaft in der Cloud läuft)
Das machst du erst in Phase 6, wenn lokal alles funktioniert. Hier nur, damit du es kennst:
1. Auf **railway.app** mit GitHub einloggen.
2. Ein neues Projekt aus deinem `friday`-Repo erstellen.
3. Eine **Postgres**-Datenbank hinzufügen (ein Klick, Railway setzt die Verbindung automatisch).
4. Alle Schlüssel aus deiner `.env` als „Variables" in Railway eintragen.
Claude Code begleitet dich Schritt für Schritt, wenn es so weit ist.

---

## Teil C — Den Bau von Claude Code durchführen lassen

Ab jetzt arbeitet ihr zu zweit: du gibst pro Phase einen Satz, Claude Code baut und testet. Nach jeder Phase prüfst du die „Verifikation" aus dem [[Friday-Umsetzungsplan]]. Hier die Sätze, die du Claude Code gibst:

**Phase 0 und 1 (TickTick anbinden):**
> Wir machen Phase 0 und 1. Richte mir die lokale Umgebung ein, prüf den Health-Check, dann verbinde meine TickTick-App. Ich habe Client ID und Secret bereit. Führ mich durch das Token-Skript und teste danach, ob du meine echten Projekte lesen kannst. Zum Schluss das Tag-Merge-Experiment aus der Anleitung. Erklär jeden Schritt einfach.

**Phase 2 (Telegram):**
> Phase 2: Telegram. Ich habe Bot-Token, Chat-ID und Webhook-Secret. Trag sie in die .env ein und schick mir eine Testnachricht, damit ich sehe, dass es ankommt. Prüf auch, dass Sprachnachrichten erkannt werden.

**Phase 3 (die tägliche Logik):**
> Phase 3: Bau die tägliche Engine fertig, so wie in der Spec beschrieben, inklusive Heartbeat, dem Fragen-Limit und der exakten Definition von „heute" gegen meine echten Daten. Lass mich einen Testlauf machen, der mir eine echte Telegram-Nachricht schickt.

**Phase 4 (Capture unterwegs):**
> Phase 4: Bau die Capture-Funktion, damit ich dem Bot unterwegs per Text oder Sprachnachricht Infos schicken kann und er daraus Vorschläge für Tasks macht. Teste es mit einem Beispiel.

**Phase 5 (Vault verbinden):**
> Phase 5: Verbinde den Vault über mein GitHub-Repo, lege den Ops-Ordner an und bau die Funktion, dass Friday nach meiner Bestätigung das Status-Frontmatter einer Projekt-Note aktualisiert.

**Phase 6 (in die Cloud):**
> Phase 6: Hilf mir, Friday auf Railway zu deployen. Führ mich durch das Anlegen des Projekts, die Datenbank und das Eintragen der Variablen, und richte am Ende den Telegram-Webhook ein.

**Phase 7 (scharf schalten):**
> Phase 7: Lass uns Friday scharf schalten. Hilf mir, die drei TickTick-Filter anzulegen, und mach den ersten Bestands-Durchlauf in kleinen Häppchen, nicht alles auf einmal.

---

## Wenn etwas schiefgeht (gilt immer)

- **Fehler im Terminal?** Den ganzen roten Text markieren, kopieren, an Claude Code schicken mit „dieser Fehler kam, bitte erklär und behebe ihn". Das ist der Normalfall, kein Drama.
- **Claude Code selbst zickt?** Im Terminal `claude doctor` ausführen, das prüft die Installation und schlägt Fixes vor.
- **Du verstehst einen Schritt nicht?** Claude Code fragen: „erklär mir das nochmal, als wäre ich kompletter Anfänger".
- **Schlüssel verloren?** Den jeweiligen wieder im Dienst neu erzeugen (BotFather, TickTick, GitHub) und in der .env ersetzen.

## Fertig heißt

Friday läuft auf Railway. Morgens kommt ein kurzes Menü plus Heartbeat per Telegram, sonntags ein Review. Du arbeitest in TickTick aus den Filtern Deep, Admin, Call. Unterwegs kannst du dem Bot per Sprachnachricht Infos zuwerfen, und er macht Tasks daraus. Nichts wird ohne dein Antippen geändert.

---

## Related
- [[Friday-Umsetzungsplan]]
- [[Friday-Spec]]
- Repo: `friday/`
