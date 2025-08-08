# VS Code MCP Starter Pack – Setup Guide (Windows)

Want to get started fast? This opinionated starter pack helps you install and configure a chat mode, add instruction files that optimize MCP tools, and adopt a streamlined development workflow. It also includes a sample chat mode inspired by Burke Holland’s [Beast Mode prompt](https://gist.github.com/burkeholland/88af0249c4b6aff3820bf37898c8bacf).

## 1) Locate your VS Code User folder

Use the path that matches your installation:

- VS Code

```text
%APPDATA%\Code\User
```

- VS Code Insiders

```text
%APPDATA%\Code - Insiders\User
```

Notes:

- %APPDATA% resolves to `C:\Users\<you>\AppData\Roaming` (Roaming is already included).
- In VS Code: Command Palette → “Developer: Reveal User Data Folder” → open the “User” subfolder.

## 2) Unzip the Starter Pack

- Extract `StarterPack.zip` somewhere convenient (e.g., your Desktop or Downloads).
- Inside the zip you’ll find:
  - `prompts/` (folder)
  - `mcp.json` (file)

## 3) Merge the prompts folder

- If you do not have a `prompts/` folder under your VS Code User folder, copy the `prompts/` from the zip directly into the User folder.
- If you already have a `prompts/` folder, merge the contents:
  - Copy all files and subfolders from the zip’s `prompts/` into your existing `%APPDATA%\Code\User\prompts` (or Insiders equivalent).
  - If asked to overwrite, choose the action that makes sense for you; if unsure, back up first (see below).

Backup tip: Create a copy of your existing prompts before merging, e.g., `prompts_backup_YYYYMMDD`.

## 4) Merge or create mcp.json

- If you already have `%APPDATA%\Code\User\mcp.json`:
  - Open both files: your existing `mcp.json` and the one from the zip.
  - Append any new servers/inputs from the zip’s `mcp.json` into your file, taking care to keep valid JSON/JSONC (commas, brackets). Avoid duplicate keys.
- If you do not have an `mcp.json` yet:
  - Place the zip’s `mcp.json` at `%APPDATA%\Code\User\mcp.json` (or the Insiders path).

Included MCP server configurations:

- `microsoft-docs`
- `memory`
- `desktop-commander` (Opinionated personal pick; remove it if you don't need it.)
- `sequential-thinking`
- `playwright`

## 5) Update settings.json

Open `%APPDATA%\Code\User\settings.json` (or Insiders equivalent) and ensure these keys exist with the values below (add if missing, update if present):

```json
"chat.tools.autoApprove": true,
"chat.agent.maxRequests": 100
```

Tip: Keep the surrounding JSON valid — include commas where needed.

## 6) Reload VS Code and verify

- Reload or restart VS Code to pick up changes.
- Open Settings (JSON) to confirm your edits were saved.
- If you use MCP, confirm servers are listed as expected.

## Troubleshooting

- JSON errors: Use a JSON linter/formatter or VS Code’s Problems panel to fix commas/brackets.
- Path confusion: Remember %APPDATA% already includes \Roaming.
- Merge conflicts: Back up your old `prompts/` and `mcp.json` before merging so you can restore if needed.

