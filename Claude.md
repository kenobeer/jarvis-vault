# Yona's Vault — Instructions for Claude

## Who this vault belongs to
21-year-old WISO student at WU Wien. Working on Gabel & Glas (food newsletter, most important project). Interested in VC, entrepreneurship, macro/markets. Exchange semester Grenoble Sep 2026.

## Vault purpose
Structured reference and knowledge base — not a session replay system. Claude memory handles cross-session continuity. The vault is for things worth finding again later, organized so they can be found.

---

## Folder structure

```
01-Ideas/       — one note per idea worth tracking
02-Knowledge/   — durable mental models and conceptual frameworks only (see definition below)
03-Projects/    — active projects, each as a file or folder (see registry below)
04-VC/          — locked until July 2026
05-People/      — one note per person worth tracking
06-Uni/         — university coursework and academic projects
_Private/       — NEVER read or write, ever
```

### When a project gets a folder vs. staying a flat file

A project starts as a single file (`ProjectName.md`). It gets promoted to a **folder** (`ProjectName/ProjectName.md`) when it generates at least one supporting document that is either long enough (>~100 lines) or specific enough to be referenced independently — a strategy doc, a PRD, a research brief, a post draft, an operational document. When you promote: move the `.md` into the folder as `ProjectName/ProjectName.md`, then add the supporting doc as a sibling file. Do not use `index.md`.

Projects currently as folders: `Gabel-und-Glas/`, `Blog/`, `Career/`, `Investments/`. Projects currently as flat files: `JustImmo-Makler-AI.md`, `VaultSetup.md`, `Finanzen.md`.

---

## Canonical project registry (03-Projects/)

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

Investment analysis notes (individual theses) live in `03-Projects/Investments/` as their own files — e.g. `NFLX-Short.md`, `EU-Mercosur.md`.

When adding a new project: add it to this table immediately, in both this file and the obsidian-router skill. Only create a new entry if the topic will clearly recur and has no match above.

---

## Pre-flight rule (run before every write)

1. Check the canonical registry above for an existing match
2. Call `list_directory("03-Projects")` to catch any files not in the registry
3. If obsidian-mcp-tools is available, call `search_vault_smart` with the topic keywords to find related notes

If a match is found: patch or append to the existing note — do not create a new file. Only create a new note if nothing relevant exists after all three checks.

---

## Routing decision tree

**Is this content about one of the active projects in the registry?**
→ Yes: write to that project's note or subfolder. Project-specific research, strategy docs, advisor feedback, operational briefings, and draft content all belong in the project's folder — not in 02-Knowledge.

**Is this a durable mental model or conceptual framework — something worth having in 5 years regardless of current projects?**
→ Yes: 02-Knowledge. See definition below.

**Is this an investment thesis or analysis of a specific security / trade?**
→ 03-Projects/Investments/ as its own file. Not in 02-Knowledge.

**Is this a new idea (not yet a project, not actively being built)?**
→ 01-Ideas. Thin ideas (one paragraph or less) go into Ideen-Sammlung.md. Developed ideas with real content get their own note.

**Is this about a specific person worth remembering?**
→ 05-People.

**Is this university coursework, an assignment, or an academic project?**
→ 06-Uni. Operational documents for the project (interview briefings, drafts) go inside the course's subfolder.

**Does it not fit any of the above?**
→ Do not save it. Not everything deserves a note. If genuinely ambiguous, ask.

---

## 02-Knowledge — strict definition

02-Knowledge is **only** for durable conceptual frameworks and mental models that pass this test: *would this still be worth having in the vault 5 years from now, even if every current project had ended?* If no, it belongs in the project folder.

What belongs: mental models (Status-Framework, Marketing-Frameworks), macro frameworks (Debasement-Trade), timeless strategic concepts.

What does NOT belong: project-specific research (goes in the project folder), investment theses (go in Investments/), interview prep (goes in the relevant project or uni folder), advisor feedback (goes in the project folder), session logs, one-off analysis.

Knowledge note format:
```
**What I learned:** [2-4 sentences — the actual insight, not a summary of a session]
**My take:** [opinion or reaction]
**Open questions:** [what's still unclear]
**Linked to:** [[related note]] · [[related note]]
```

---

## Project note format

```
---
type: project
status: active
tags: []
created: YYYY-MM-DD
updated: YYYY-MM-DD
---

# [Project Name]

## Decisions & facts
[Current-state bullets. Update in place — this section always reflects now, not history.]

---

## Log

### YYYY-MM-DD — [short label]
**Was passierte:** [1-2 sentences]
**Decisions:** [bullets]
**Open threads:** [bullets]
```

**When to update Decisions & facts:** Any time a permanent decision was made or a key fact changed. Edit existing bullets, don't append history.

**When to add a Log entry:** Only when something genuinely notable happened — multiple decisions in one session, an external event (advisor feedback received, application sent, product launched, strategic direction changed). Routine single-question sessions: update Decisions & facts only, no log entry.

---

## Investment thesis note format (03-Projects/Investments/)

```
---
type: investment-idea
status: [watching / active / closed]
tags: []
created: YYYY-MM-DD
---

# [Ticker / Trade Name]

## Kernthese
[One paragraph — bull or bear case]

## Argumente
[Bull and bear points]

## Katalysatoren
[What would trigger action]

## Status & Entscheidung
[Current status. "Beobachten. Kein Trade ohne Katalysator." is a valid status.]
```

---

## People note format (05-People/)

Filename: `Vorname-Nachname.md`. Always include the H1 heading `# Vorname Nachname` as first content line.

```
# Vorname Nachname

**Kontext:** [when/where met — one sentence, specific]
**Kontakt:** [channels — LinkedIn · Instagram · WhatsApp · Phone · Email, mark each ✓/✗]
**Beruf:** [current role / company]
**Projekt:** [[linked project or idea if relevant]]

---

## Notizen
[free text, compact]

## Potenzielle Relevanz
[why relevant, brief]

---

**Linked to:** [[related notes]]
```

Frontmatter: `type: person`, `status: active`, tags include `people`.

---

## Ideas (01-Ideas/)

- `status: active` — own ideas being explored
- `status: watching` — external projects (someone else's), observing but not running
- `status: archived` — evaluated and set aside (keep the note, the reasoning is useful)
- `status: completed` — finished (e.g. a uni project that became a uni note)

Thin ideas with no real development go into `Ideen-Sammlung.md`, not their own note. An idea earns its own note when it has a clear thesis, at least a paragraph of real thinking, and a realistic chance of being revisited.

---

## Linking rules

- Always use `[[double brackets]]` for any note that exists
- Use full vault paths in links — `[[03-Projects/Career/Career]]`, not `[[Career]]` — to prevent ambiguity and broken links if files move
- Only link to notes that already exist. Never invent links to paths that don't exist
- Knowledge notes and project notes on the same topic link to each other
- People notes link to relevant Ideas and Project notes
- Investment analysis notes link to the main `[[03-Projects/Investments/Investments]]` note

---

## Frontmatter rule

Always use the `frontmatter` parameter in `obsidian:write_note` — never embed YAML manually in the content body. Valid types: `project`, `knowledge`, `person`, `idea`, `investment-idea`, `research`, `memory`.

---

## Token-efficient write patterns

- **Updating a section:** use `obsidian-mcp-tools:patch_vault_file` (targets a heading, cheapest)
- **Adding a log entry:** use `obsidian-mcp-tools:append_to_vault_file`
- **Full overwrite:** only when restructuring an entire note or creating new
- **Always read before writing** — check what's already there first

---

## 04-VC lockout

04-VC is locked until July 2026. Until then: VC-related insights go in `03-Projects/Career/Career.md` under a VC section, or in 02-Knowledge if genuinely conceptual. Tag with `vc` for later retrieval. Do not create files in 04-VC.

---

## Uni project lifecycle

06-Uni holds active coursework and academic projects. When a course or project ends, update `status: completed` in frontmatter — do not delete. The learning stays useful. Operational documents (interview briefings, assignment drafts) live inside the project's subfolder, not in 02-Knowledge.

---

## Privacy

`_Private/` is completely off-limits. Never read, write, reference, or acknowledge content from it.
