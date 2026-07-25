---
name: github-release-workflow
description: >
  Cut a GitHub release from Hermes using the GitHub MCP server, end-to-end:
  release notes → tag → GitHub release. Use when the user asks to release,
  tag, cut a version, or publish a release for a BrownMediaGroup project.
---

# GitHub Release Workflow Skill

## When to use
- User says: release, tag, cut version, publish release, deploy version
- Project has a `plans/releases/YYYY-MM-DD-vX.Y.Z.md` release notes file

## Inputs to confirm with user (if not already given)
- Repo owner/name
- Version string (`vX.Y.Z`) and date (`YYYY-MM-DD`)
- Asset installer path to attach (optional)

## Step-by-step

1. Verify GitHub MCP is connected:
   `python -m hermes_cli.main mcp test github`

2. Read current release notes from project repo:
   `G:/BrownMediaGroup/dev/<project>/plans/releases/YYYY-MM-DD-vX.Y.Z.md`

3. Push release notes file with `mcp__github__push_files` to branch `main`

4. Get the latest commit SHA from `mcp__github__list_commits --sha main`

5. Create git tag reference:
   `mcp__github__create_or_update_file` with path `.git/refs/tags/vX.Y.Z` equivalent
   Preferred via direct API or GitHub MCP create reference behavior available.

6. Create GitHub release:
   `mcp__github__create_or_update_file` is NOT for releases.
   Use `mcp__github__create_release` if exposed by MCP server, otherwise:
   - Pause workflow
   - Tell user: “Release-tag creation requires additional MCP tooling; 
     GitHub MCP on this Hermes instance may need `github create_release` capability.”

## Fallback if MCP lacks release tooling
- Provide exact `git` + `gh` CLI commands for the user/system:
  ```
  git -C G:/BrownMediaGroup/dev/<project> tag -a vX.Y.Z -m "vX.Y.Z YYYY-MM-DD"
  git -C G:/BrownMediaGroup/dev/<project> push origin vX.Y.Z
  gh release create vX.Y.Z --repo RiseUpGit/<project> \
    --title "vX.Y.Z (YYYY-MM-DD)" \
    --notes-file G:/BrownMediaGroup/dev/<project>/plans/releases/YYYY-MM-DD-vX.Y.Z.md
  ```

## Output format
After success, report:
- Repo + release URL
- Tag SHA
- Attached assets
- Next suggested action (e.g., “send notify to Telegram gateway”)
