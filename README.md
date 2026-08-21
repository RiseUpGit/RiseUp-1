# RiseUp-1

**Public reusable reference for BrownMediaGroup:** skills, templates, build/deploy helpers, and shared utilities.

## Contents

| Folder | Description |
|---|---|
| `skills/` | Reusable Hermes skills for build, deploy, and ops workflows |
| `scripts/` | Utility scripts (placeholder for future additions) |
| `plans/` | Release and deployment plans |

## Included Skills

- **base-docs-mcp-setup** — Connect Hermes to Base chain docs via MCP
- **github-release-workflow** — Automated GitHub release creation from Hermes
- **sam-crypto-forecaster** — Crypto market snapshot tool for Sam oracle

## Usage

These skills are installed into Hermes profiles and referenced by name in cron jobs and agent workflows. They are not standalone applications — they extend Hermes capabilities.

## Visibility

This repo is **public**. Skills published here are safe for community reuse. Sam-specific and experimental skills remain in research hold until ready.

## License

MIT — feel free to fork and adapt.

---

Part of the [BrownMediaGroup LLC](https://github.com/RiseUpGit/BrownMediaGroup-LLC) project family.
