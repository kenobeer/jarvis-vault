---
created: '2026-04-10'
status: active
tags:
  - vault
  - claude-setup
type: project
updated: '2026-05-04'
---
# VaultSetup

## Decisions & facts

### Vault-Philosophie
- Vault ist Claude-input fokussiert, nicht manueller Input
- Claude entscheidet immer wo Dinge gespeichert werden — User gibt nie Ordner an
- Vault dient als strukturiertes Referenz- und Wissenssystem, nicht als Session-Replay-Mechanismus
- Claude-Memory übernimmt Cross-Session-Kontinuität — Vault ist das strukturierte Langzeitgedächtnis

### Aktuelle Struktur
- 01-Ideas/: eine Note pro Idee
- 02-Knowledge/: eine Note pro Konzept/Framework
- 03-Projects/: eine Note pro aktivem Projekt (Decisions & facts + optionaler Log)
- 04-VC/: gesperrt bis Juli 2026
- 05-People/: eine Note pro Person
- 06-Uni/: universitäre Projekte und akademische Arbeit
- _Private/: absolut off-limits

### Skill
- obsidian-router Skill installiert und aktiv
- Regelt autonomes Routing — User muss nie Ordner spezifizieren

### MCP-Verbindung
- Obsidian verbunden via @bitbonsai/mcpvault MCP Server
- obsidian-mcp-tools Plugin empfohlen für semantische Suche (noch nicht installiert)

### Token-Effizienz
- Bevorzuge patch_vault_file für Sections, append für Log-Einträge, overwrite nur für vollständige Restructures
- Immer Note lesen bevor geschrieben wird
