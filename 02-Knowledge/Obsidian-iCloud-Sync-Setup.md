---
type: knowledge
status: active
tags:
  - obsidian
  - setup
  - icloud
created: '2026-05-25'
---
# Obsidian iCloud Sync Setup

**What I learned:**
Obsidian Sync ($4/month) ist nicht notwendig wenn iCloud bereits aktiv ist. Der korrekte Weg: Vault direkt über die Obsidian iOS App erstellen (nicht manuell den Ordner anlegen) — das legt den Vault automatisch unter `~/Library/Mobile Documents/iCloud~md~obsidian/Documents/[VaultName]` ab, was der von Obsidian erkannte iCloud-Pfad ist. Bestehenden Vault-Inhalt danach per `cp -R` rüberkopieren. Die MCP-Config in Claude Desktop muss auf den neuen Pfad zeigen.

**My take:**
Der alte Vault lag bereits in iCloud (`com~apple~CloudDocs`) — war also nicht falsch, nur nicht im Obsidian-spezifischen iCloud-Container. Der Obsidian-eigene Container (`iCloud~md~obsidian`) ist stabiler weil Obsidian iOS ihn automatisch erkennt ohne manuelles Verknüpfen.

**Open questions:**
- obsidian-mcp-tools Pfad in der Config noch zu korrigieren (fehlende führende `/` + alter Vault-Pfad)
- Plugins im neuen Vault reaktivieren (sind kopiert aber deaktiviert)

## Technische Details

**Obsidian iCloud Pfad (korrekt):**
`~/Library/Mobile Documents/iCloud~md~obsidian/Documents/[VaultName]`

**Migration Terminal-Befehl:**
```bash
cp -R ~/Library/Mobile\ Documents/com~apple~CloudDocs/Obsidian\ Vault/. ~/Library/Mobile\ Documents/iCloud~md~obsidian/Documents/Jarvis/
```

**Claude Desktop Config (MCP):**
```json
{
  "mcpServers": {
    "obsidian": {
      "command": "npx",
      "args": [
        "@bitbonsai/mcpvault@latest",
        "/Users/yona/Library/Mobile Documents/iCloud~md~obsidian/Documents/Jarvis"
      ]
    }
  }
}
```

**Linked to:** [[03-Projects/VaultSetup]]
