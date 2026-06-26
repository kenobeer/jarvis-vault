---
name: obsidian-router
description: Use this skill whenever the user wants to save, log, capture, or store anything from a Claude conversation into their Obsidian vault. Triggers on phrases like "save this", "log this", "add this to my vault", "store this", "compress this session", "remember this in Obsidian", or any request to persist information from a chat. Claude autonomously decides where the content goes — the user never specifies a folder or note type.
---

# Obsidian Router

Claude decides where everything goes. The user never specifies a folder.

The vault is a structured reference and knowledge base — not a session replay system. Save things in a way that makes them findable later, not to create a complete log of every conversation.

---

## STEP 0 — Pre-flight (always run first)

Before writing anything:

1. Check the canonical project registry below for an existing match
2. Call `list_directory("03-Projects")` to catch any files not in the registry
3. If obsidian-mcp-tools is connected, call `search_vault_smart` with topic keywords to find related notes
4. If a related note exists, read it before deciding what to write

Only create a new note if nothing relevant exists after all three checks. Patch or append to existing notes whenever possible.

---

## Vault structure

```
01-Ideas/       — one note per idea worth tracking
02-Knowledge/   — durable mental models and conceptual frameworks ONLY (see definition)
03-Projects/    — active projects as files or folders (see registry)
04-VC/          — locked until July 2026
05-People/      — one note per person worth tracking
06-Uni/         — university coursework and academic projects
_Private/       — off-limits, never read or write
```

No Inbox. Nothing gets parked without a home.

---

## Routing decision tree

Work through these in order. Stop at the first match.

**1. Is this about an active project in the registry?**
→ Write to that project's note or subfolder. Project-specific research, strategy docs, advisor feedback, operational briefings, and draft content all go in the project folder — not in 02-Knowledge.

**2. Is this a durable mental model or conceptual framework — worth having in 5 years regardless of current projects?**
→ 02-Knowledge. See strict definition below.

**3. Is this an investment thesis or analysis of a specific security, trade, or market trend?**
→ 03-Projects/Investments/ as its own file. Not in 02-Knowledge.

**4. Is this a new idea — not yet a project, not actively being built?**
→ 01-Ideas. Thin ideas (one paragraph or less, no real development) go into Ideen-Sammlung.md. Developed ideas with real thinking get their own note.

**5. Is this about a specific person worth remembering?**
→ 05-People.

**6. Is this university coursework, an assignment, or an academic project?**
→ 06-Uni. Operational documents (interview briefings, drafts) go inside the course's subfolder.

**7. Does it not fit any of the above?**
→ Do not save it. Not everything deserves a note.

---

## Canonical project registry

Always use exactly these paths. Never create variations.

| Project | Path |
|---|---|
| Gabel & Glas newsletter | `03-Projects/Gabel-und-Glas/Gabel-und-Glas.md` |
| Career planning & VC roadmap | `03-Projects/Career/Career.md` |
| LinkedIn & personal branding | `03-Projects/Career/LinkedIn.md` |
| Blog / writing | `03-Projects/Blog/Blog.md` |
| Investment ideas & theses | `03-Projects/Investments/Investments.md` |
| Macroeconomics & markets | `03-Projects/Investments/Macro.md` |
| Personal finances & runway | `03-Projects/Finanzen.md` |
| Vault & Claude setup | `03-Projects/VaultSetup.md` |
| JustImmo Makler AI Platform | `03-Projects/JustImmo-Makler-AI.md` |

Individual investment analyses live in `03-Projects/Investments/` as their own files (e.g. `NFLX-Short.md`).

**When adding a new project:** add it to this table immediately, and also to Claude.md. Only create a new entry if the topic will clearly recur and has no match above.

---

## When a project gets a folder vs. staying a flat file

A project starts as a single file (`ProjectName.md`). Promote it to a **folder** when it generates a supporting document long enough or specific enough to reference independently — a PRD, a research doc, a strategy brief, a post draft. When promoting: move `ProjectName.md` into the folder as `ProjectName/ProjectName.md`. Add supporting docs as siblings. Never use `index.md`.

Currently as folders: `Gabel-und-Glas/`, `Blog/`, `Career/`, `Investments/`, `06-Uni/Bondi-Consult/`.
Currently as flat files: `JustImmo-Makler-AI.md`, `VaultSetup.md`, `Finanzen.md`.

---

## 02-Knowledge — strict definition

Only notes that pass this test: *would this still be worth having in the vault 5 years from now, even if every current project had ended?* If no, it belongs in the project folder instead.

**Belongs here:** Mental models (Status-Framework, Marketing-Frameworks), macro frameworks (Debasement-Trade), timeless strategic concepts.

**Does NOT belong here:** Project-specific research → project folder. Investment theses → Investments/. Interview prep → uni or project folder. Advisor feedback → project folder. Session logs or one-off analysis → project folder.

Knowledge note format:
```
**What I learned:** [2-4 sentences — the actual insight, not a session summary]
**My take:** [opinion or reaction]
**Open questions:** [what's still unclear]
**Linked to:** [[full/path/to/note]] · [[full/path/to/note]]
```

---

## Project note format

```markdown
---
type: project
status: active
tags: []
created: YYYY-MM-DD
updated: YYYY-MM-DD
---

# [Project Name]

## Decisions & facts
[Current-state bullets. Update in place — always reflects now, not history.]

---

## Log

### YYYY-MM-DD — [short label]
**Was passierte:** [1-2 sentences]
**Decisions:** [bullets]
**Open threads:** [bullets]
```

**When to update Decisions & facts:** Any permanent decision or key fact change. Edit existing bullets, don't append history.

**When to add a Log entry:** Only when something genuinely notable happened — multiple decisions in one session, an external event (advisor feedback received, application sent, product launched, strategic direction changed). Routine single-question sessions: update Decisions & facts only, no log entry.

---

## Investment thesis note format (03-Projects/Investments/)

```markdown
---
type: investment-idea
status: watching
tags: []
created: YYYY-MM-DD
---

# [Ticker / Trade Name]

## Kernthese
[Bull or bear case, one paragraph]

## Argumente
[Bull and bear points]

## Katalysatoren
[What would trigger action]

## Status & Entscheidung
[e.g. "Beobachten. Kein Trade ohne Katalysator."]
```

---

## People note format (05-People/)

Filename: `Vorname-Nachname.md`. Always include `# Vorname Nachname` as the first content line after frontmatter.

```markdown
# Vorname Nachname

**Kontext:** [when/where met — one sentence, specific]
**Kontakt:** [LinkedIn · Instagram · WhatsApp · Phone · Email — mark each ✓/✗]
**Beruf:** [current role / company]
**Projekt:** [[full/path/to/project]]

---

## Notizen
[free text, compact]

## Potenzielle Relevanz
[why relevant, brief]

---

**Linked to:** [[full/path/to/related/note]]
```

Frontmatter: `type: person`, `status: active`, tags include `people`.

---

## Ideas (01-Ideas/)

- `status: active` — own ideas being explored
- `status: watching` — external projects (someone else's), observing but not running
- `status: archived` — evaluated and set aside (keep the note, reasoning is useful)
- `status: completed` — finished

Thin ideas with no real development go into `Ideen-Sammlung.md`. An idea earns its own note when it has a clear thesis, at least a paragraph of real thinking, and a realistic chance of being revisited.

---

## Frontmatter for every new note

```yaml
---
type: [project / knowledge / person / idea / investment-idea / research]
status: [active / watching / archived / completed]
tags: []
created: YYYY-MM-DD
---
```

Always use the `frontmatter` parameter in `obsidian:write_note` — never embed YAML manually in the content body.

---

## Linking rules

- Always use `[[double brackets]]` for any note
- Use **full vault paths** — `[[03-Projects/Career/Career]]`, not `[[Career]]` — to prevent broken links if files move
- Only link to notes that already exist. Never invent links
- Knowledge notes and project notes on the same topic link to each other
- People notes link to relevant Ideas and Project notes
- Investment analysis notes link to `[[03-Projects/Investments/Investments]]`

---

## Token-efficient write patterns

- **Updating a section:** `obsidian-mcp-tools:patch_vault_file` (targets a heading, cheapest)
- **Adding a log entry:** `obsidian-mcp-tools:append_to_vault_file`
- **Full overwrite:** only when restructuring an entire note or creating new
- **Always read before writing**

If obsidian-mcp-tools is connected, prefer its tools over obsidian: tools for all writes.

---

## Privacy and locked folders

- `_Private/` — permanently off-limits. Never read, write, reference, or acknowledge content from it.
- `04-VC/` — locked until July 2026. Route VC-related content to `03-Projects/Career/Career.md` (under a VC section) or to 02-Knowledge if conceptual. Tag with `vc`.

---

## Edge cases

**Content spans multiple projects** (e.g. Career + G&G)
→ Write to the primary project. Add cross-reference links in both notes.

**Thin or one-off conversation, nothing worth saving**
→ Don't save it. Ask if unsure.

**Something is both a reusable concept AND a project decision**
→ Save the decision in the project note. If the concept generalizes beyond this project, also create a 02-Knowledge note. Link both directions.

**New recurring topic with no project note yet**
→ Create the project note only if clearly going to recur. Add to the registry in this skill and in Claude.md immediately.
