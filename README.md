# Excalidraw Diagram Skill

Generate `.excalidraw` JSON files that argue visually — not just boxes-and-arrows.

Based on [coleam00/excalidraw-diagram-skill](https://github.com/coleam00/excalidraw-diagram-skill), adapted for dual-environment use (Claude Code + Claude Chat) with a refined color palette and expanded semantic coverage.

## Install

Pick one option. **Symlink is recommended** — `git pull` updates the installed skill in place.

### Claude Code — global (all projects)

Run from inside the cloned repo.

| Mode    | macOS / Linux                            | Windows                                                              |
|---------|------------------------------------------|----------------------------------------------------------------------|
| Symlink | `ln -s "$PWD" ~/.claude/skills/excalidraw` | `mklink /D "%USERPROFILE%\.claude\skills\excalidraw" "%CD%"`         |
| Copy    | `cp -r . ~/.claude/skills/excalidraw`     | `xcopy /E /I . "%USERPROFILE%\.claude\skills\excalidraw"`            |

The Windows symlink command needs Developer Mode enabled or an elevated terminal.

### Claude Code — single project

Same commands, but point at `<project>/.claude/skills/excalidraw` instead of the user-global path. Project-level skills override global ones with the same name.

### Claude Chat (claude.ai)

Upload the skill via the Skills UI on claude.ai, or distribute it as a plugin. You don't manage the runtime path manually — Claude exposes it at `/mnt/skills/user/excalidraw/` inside the sandbox.

## Render pipeline (Claude Code only)

Optional. The render-validate loop lets Claude check diagrams visually before delivering them. Requires Python ≥3.11, [uv](https://docs.astral.sh/uv/), and a Chromium binary for Playwright.

**Install uv** (once per machine):

| Platform       | Command                                                                          |
|----------------|----------------------------------------------------------------------------------|
| macOS / Linux  | `curl -LsSf https://astral.sh/uv/install.sh \| sh` (or `brew install uv` on Mac) |
| Windows        | `powershell -c "irm https://astral.sh/uv/install.ps1 \| iex"`                    |

**Then sync dependencies and install Chromium:**

```bash
cd ~/.claude/skills/excalidraw/references
uv sync
uv run playwright install chromium
```

(On Windows, replace the path with `%USERPROFILE%\.claude\skills\excalidraw\references`.)

If you skip this step the skill still works — Claude generates `.excalidraw` files but won't render and self-correct them.

## Verify

Start a new Claude Code session and ask:

> "Use the excalidraw skill to diagram an OAuth 2.0 authorization code flow"

Claude should auto-discover the skill. If not, confirm that `SKILL.md` is reachable at `~/.claude/skills/excalidraw/SKILL.md` (symlink not broken).

## Usage

> "Create an Excalidraw diagram showing an OAuth 2.0 authorization code flow"

> "Diagram a microservices architecture with a message queue between services"

> "Visualize a CI/CD pipeline from commit to production deploy"

The skill handles concept mapping, layout, JSON generation, and (in Claude Code) visual validation.

## Customize colors

Edit `references/color-palette.md` to match your brand. Everything else is universal design methodology.

## File structure

```
excalidraw-skill/
  SKILL.md                  # Design methodology + workflow
  README.md                 # This file
  references/
    color-palette.md        # Brand colors (edit to customize)
    element-templates.md    # JSON templates per element type
    json-schema.md          # Excalidraw JSON format reference
    render_excalidraw.py    # Render .excalidraw to PNG (Claude Code)
    render_template.html    # Browser template for rendering
    pyproject.toml          # Python dependencies (playwright)
```
