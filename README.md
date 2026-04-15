# Excalidraw Diagram Skill

Generate `.excalidraw` JSON files that argue visually — not just boxes-and-arrows.

Based on [coleam00/excalidraw-diagram-skill](https://github.com/coleam00/excalidraw-diagram-skill), adapted for dual-environment use (Claude Code + Claude Chat) with a refined color palette and expanded semantic coverage.

## Installation

### Claude Code
```bash
cp -r excalidraw-skill .claude/skills/excalidraw
```

### Claude Chat (claude.ai)
Upload or install to `/mnt/skills/user/excalidraw/`.

## Setup (Claude Code only)

The render pipeline lets Claude visually validate diagrams before delivering them.

```bash
cd .claude/skills/excalidraw/references
uv sync
uv run playwright install chromium
```

## Usage

Ask Claude to create a diagram:

> "Create an Excalidraw diagram showing an OAuth 2.0 authorization code flow"

> "Diagram a microservices architecture with a message queue between services"

> "Visualize a CI/CD pipeline from commit to production deploy"

The skill handles concept mapping, layout, JSON generation, and (in Claude Code) visual validation.

## Customize Colors

Edit `references/color-palette.md` to match your brand. Everything else is universal design methodology.

## File Structure

```
excalidraw/
  SKILL.md                          # Design methodology + workflow
  README.md                         # This file
  references/
    color-palette.md                # Brand colors (edit to customize)
    element-templates.md            # JSON templates per element type
    json-schema.md                  # Excalidraw JSON format reference
    render_excalidraw.py            # Render .excalidraw to PNG (Claude Code)
    render_template.html            # Browser template for rendering
    pyproject.toml                  # Python dependencies (playwright)
```
