---
name: base-docs-mcp-setup
description: >
  Set up the Base Docs MCP server in Hermes. The Base MCP endpoint
  is hosted remotely — no local install required. Connects Hermes to
  Base chain/agent docs for live search and querying via
  `https://docs.base.org/mcp`.
platforms: [windows, linux, macos]
---

# Base Docs MCP Setup

## One-line setup
```sh
python -c "
import yaml, pathlib
p = pathlib.Path('C:/Users/<user>/AppData/Local/hermes/profiles/<profile>/config.yaml')
d = yaml.safe_load(p.read_text())
d['mcp_servers']['base-docs'] = {'url': 'https://docs.base.org/mcp', 'transport': 'http'}
p.write_text(yaml.dump(d, default_flow_style=False))
"
```

## Config entry
```yaml
mcp_servers:
  base-docs:
    url: https://docs.base.org/mcp
    transport: http
```

## Verify
```powershell
python -m hermes_cli.main mcp test base-docs
python -m hermes_cli.main mcp list
```

## Expected result
- Connected in ~1s
- 3 tools discovered:
  - `search_base_documentation`
  - `query_docs_filesystem_base_documentation`
  - `submit_feedback`

## Requirements
- No auth key required
- Restart Hermes after editing `mcp_servers:` — config is not hot-reloaded
- Does NOT require Hermes Node prefix install — this is a remote HTTP MCP

## Pitfalls
- If `mcp test` shows "Server not found", Hermes was not restarted after the config change.
- The Base endpoint is HTTPS only — no local SSE/stdio needed.