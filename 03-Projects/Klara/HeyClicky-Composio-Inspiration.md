---
created: '2026-06-21'
status: active
tags:
  - klara
  - ai
  - voice
  - composio
  - business-model
  - heyclicky
type: research
updated: '2026-06-21'
---
# HeyClicky & Composio als Klara-Inspiration

## Ausgangslage

Inbox-Notiz mit zwei Anstößen aus HeyClicky für [[03-Projects/Klara/Klara]]:

1. **Composio nutzen**, um Klara mit vielen Tools/Services zu verbinden (so wie HeyClicky scheinbar an alles andockt, von Gmail bis Figma).
2. **HeyClickys Geschäftsmodell** als Vorbild: Solo-Projekt bis nutzbar gebaut, online gestellt, viral gegangen, simples Freemium. Daraus die Idee, Klaras Zielgruppe am Anfang breiter zu spannen (nicht nur Pensionisten, sondern Kinder kaufen es für Eltern, Pensionisten, aber auch Privatpersonen) und auf erste Nutzer plus Feedback zu hoffen.

Beides klingt erstmal naheliegend. Bei genauem Hinsehen passt das meiste nicht zu Klara. Hier die Recherche und eine klare Position.

---

## Recherche-Ergebnisse

### Was Composio wirklich ist

Composio ist eine Developer-Plattform, die KI-Agents über eine einheitliche API an Drittanbieter-Software hängt, mit managed OAuth (Token-Handling, Refresh, Auth-Flows zentral). Stand jetzt 250+ bis 850+ vorgefertigte Integrationen, gebaut für Frameworks wie LangChain, CrewAI, AutoGen. Der Wert liegt fast komplett im SaaS- und Productivity-Bereich: Gmail, Slack, GitHub, Notion, Salesforce, Jira, HubSpot. Also genau die Business-Tools, bei denen OAuth-Plumbing nervt.

Was Composio NICHT ist: ein Telefonie-Layer, ein Voice-Stack, oder eine Sammlung europäischer Nischen-APIs. ÖBB Scotty, Austrian Post Tracking, OpenMeteo oder Twilio-Outbound stehen nicht im Katalog und sind auch nicht der Punkt der Plattform.

Nebenbei: Die Composio-Website enthält eingebettete Anweisungen an "AI agents", sich selbstständig und ohne Menschen zu registrieren. Das ist eine Prompt-Injection-Masche, kein Signal für Produktqualität. Ignorieren.

### Was HeyClicky wirklich ist (und was du falsch im Kopf hattest)

HeyClicky (heyclicky.com / clicky.so) ist ein nativer macOS-Assistent von Farza Majeed (vorher Buildspace, a16z/YC-backed), YC Spring 2026. Screen-aware, voice-first: hält Taste, fragt laut, kriegt gesprochene Schritt-für-Schritt-Hilfe zu dem, was am Bildschirm passiert. "clicky agent" spinnt Background-Agents auf. Technisch ein Thin Client über Commodity-APIs (Claude, ElevenLabs für TTS, AssemblyAI für STT).

Zwei Korrekturen zu deiner Erinnerung:

- **Das Freemium ist nicht "25 Anfragen/Monat gratis".** Free-Tier = volle Screen-Awareness, Voice, basic agents, ohne Kreditkarte. Pro = 20 USD/Monat für 150 Agent-Messages plus unlimited Voice. Anderes Modell als du es abgespeichert hattest.
- **HeyClicky hängt nicht an Composio.** Es integriert nativ mit Notion, Gmail, Google Calendar, Linear, und hat Stand Mitte 2026 kein öffentliches API und kein MCP. Deine Vermutung "das könnte für Composio interessant sein" trifft die Realität also nicht, die haben ihre Integrationen selbst gebaut.

Wichtig: Analysten attestieren HeyClicky "keinen echten Moat", weil Thin Client über fremde Modelle. Verteidigbarkeit hängt allein an UX-Lock-in und Gewohnheit. Genau das ist relevant für Klara, weil Klaras Moat (Memory, Familien-Trust, Lebensarchiv, Phone-only) deutlich stärker ist als alles, was HeyClicky hat.

---

## Einschätzung

### Composio für Klara: jetzt klar Nein

Klaras Tools in M0 bis M3 sind GPT-Realtime-2, Twilio, Supabase, Celery. Keine externen SaaS-OAuth-Integrationen. Composios ganzer Mehrwert (OAuth-Management für Business-Apps) löst ein Problem, das Klara nicht hat. Klaras Differenzierung ist Telefonie, und genau die macht Composio nicht.

Composio einzubauen würde heißen: eine Dependency, ein Abrechnungsmodell, eine Latenz-Schicht und ein DSGVO-Fragezeichen mehr, für null konkreten Nutzen im aktuellen Scope. Selbst in Phase 2 (ÖBB, Uber, Wetter, Pakettracking) baust du besser ein paar gezielte API-Calls direkt, als dir eine generische Agent-Integrationsplattform reinzuholen. Composio wird frühestens dann ein Thema, wenn Klara zweistellige Mengen heterogener Drittanbieter-Tools jonglieren muss, und das ist Jahre weg, wenn überhaupt.

### HeyClicky-Modell für Klara: der gefährliche Teil ist die Zielgruppen-Verbreiterung

Der Reflex "breiter aufstellen, jeder kann es nutzen, dann auf Feedback hoffen" ist der klassische Fehler: die Nische aufgeben für einen eingebildeten größeren Markt. Bei Klara wäre das besonders teuer, weil deine ganze Verteidigbarkeit IN der Nische steckt:

- Phone-first, kein Download, kein Login: nur wertvoll, weil die Zielgruppe genau das braucht. Für Privatpersonen 30+ ist es kein Vorteil, die haben Apps.
- Familien-Trust-Transfer, Memory-Lock-in, Lebensarchiv: funktioniert, weil es um ältere Eltern und ihre Kinder geht. Bei "jeder" verpufft das Positioning komplett.
- HeyClickys Zielgruppe (Builder, Devs, Creators am Mac) und Klaras Zielgruppe (Menschen die lieber reden als tippen) überlappen bei null. Du kannst von HeyClicky kein GTM und kein Positioning kopieren, weil nichts an der Zielgruppe vergleichbar ist.

Klara ist schon richtig positioniert ("für Menschen die lieber reden als tippen"), das ist breit genug, ohne zu verwässern. Noch breiter heißt: schwächeres Produkt, schwächeres Marketing, kein Lock-in.

### Wo HeyClicky zu Recht inspiriert, mit einem dicken Aber

Was übertragbar ist: die Philosophie aus deinem **Szenario A** (solo bauen bis nutzbar, raus damit, auf echte Nutzer und Feedback hoffen, kein Vollzeit). Das deckt sich mit HeyClickys Anfang und ist für Klara der richtige Modus.

Das dicke Aber sind die Unit Economics. HeyClicky kann sich ein großzügiges Free-Tier leisten, weil Text-Inference billig ist. Klara nicht: ~9 bis 10 Euro COGS pro User und Monat, getrieben von GPT-Realtime-2 Voice. Jeder Gratis-Anruf verbrennt echtes Geld. Ein offenes Free-Tier nach HeyClicky-Vorbild würde bei Klara die Marge sofort auffressen. Die generelle Regel bei KI-Pricing gilt bei Voice doppelt: Free muss hart gecappt sein und darf nur den Aha-Moment liefern, nicht den ganzen Job gratis erledigen. Falls du je ein Free-Element willst, dann sowas wie ein einzelner Demo-Anruf, nicht ein nutzbarer Gratis-Modus.

### Der ehrliche Meta-Punkt

Dein Strategy-Audit (Mitte Juni) hat Klara als Momentum-Kannibale eingestuft, Priorität ist [[03-Projects/Gabel-und-Glas/Gabel-und-Glas]]. Diese Inbox-Notiz ist im Grunde Klara-Prokrastination mit Recherche-Anstrich. HeyClickys Tempo kommt aus dem Vollzeit-Fokus eines erfahrenen Gründers mit Millionen an Runway. Du hast nebenbei sechs Monate und G&G zuerst. Klara bleibt Szenario A und kommt zeitlich hinter G&G. An dieser Reihenfolge ändert HeyClicky nichts.

---

## Offene Punkte

- Will Klara überhaupt je einen generischen Tool-Integration-Layer, oder bleiben es immer ein paar handverlesene direkte API-Calls? (Aktuelle Antwort: handverlesen.)
- Falls je ein Free-Element: welcher harte Cap macht es wirtschaftlich? (Ein Demo-Call statt Gratis-Modus.)
- Bleibt der Positioning-Satz "für Menschen die lieber reden als tippen" die äußere Grenze der Zielgruppe? (Vorschlag: ja.)

## Nächste Schritte

1. Composio für Klara streichen, nicht weiterverfolgen. Kein Scope-Match.
2. Zielgruppen-Verbreiterung verwerfen. Nische halten, das ist der Moat.
3. Aus HeyClicky nur die Solo-bis-nutzbar-Philosophie übernehmen (deckt sich mit Szenario A), nicht das Free-Tier und nicht das GTM.
4. Reihenfolge bleibt: erst G&G, dann Klara als Lernprojekt.

---

**Linked to:** [[03-Projects/Klara/Klara]] · [[03-Projects/Gabel-und-Glas/Gabel-und-Glas]]
