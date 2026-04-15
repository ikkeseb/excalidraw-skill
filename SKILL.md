---
name: excalidraw
description: Create Excalidraw diagram JSON files that make visual arguments — not just boxes-and-arrows. Use this skill whenever the user asks to create, generate, draw, or visualize a diagram, flowchart, architecture diagram, process flow, system overview, network topology, integration map, workflow, or any visual that explains relationships, causality, or structure. Also trigger on "excalidraw", ".excalidraw", "lag et diagram", "tegn", "visualiser", "prosessflyt", "arkitekturdiagram", or when the user says things like "can you show me how X connects to Y" or "map out the flow of Z". Use this skill even for quick conceptual sketches — it handles both simple and comprehensive diagrams. Do NOT use for charts/graphs with numerical data (use a charting tool), UI mockups (use frontend-design), or pure text documentation.
---

# Excalidraw Diagram Creator

Generate `.excalidraw` JSON files that **argue visually**, not just display information.

## Environment Detection

This skill works in both Claude Code and Claude Chat.

**Claude Code** (`/.claude/skills/`): Full render-validate loop available via Playwright. After generating JSON, render to PNG, view it, and fix issues iteratively. See `references/render_excalidraw.py`.

**Claude Chat** (`/mnt/skills/user/`): Create the `.excalidraw` JSON file and save to `/mnt/user-data/outputs/`. The user opens it in [excalidraw.com](https://excalidraw.com) or the Excalidraw desktop app. No render loop — get it right by following the methodology carefully.

**First-time setup (Claude Code only):**
```bash
cd <skill-path>/references && uv sync && uv run playwright install chromium
```

## Customization

All colors live in `references/color-palette.md`. Edit that file to rebrand — everything else is universal methodology.

---

## Core Philosophy

**Diagrams should ARGUE, not DISPLAY.**

A diagram is a visual argument showing relationships, causality, and flow that words alone can't express. The shape should BE the meaning.

**The Isomorphism Test**: Remove all text. Does the structure alone communicate the concept? If not, redesign.

**The Education Test**: Could someone learn something concrete, or does it just label boxes? Good diagrams teach.

---

## Depth Assessment (Do This First)

### Simple / Conceptual
Use abstract shapes when explaining mental models, the audience doesn't need technical specifics, or the concept IS the abstraction.

### Comprehensive / Technical
Use concrete examples when diagramming a real system, the diagram will teach or explain, the audience needs to understand what things actually look like, or you're showing how technologies integrate.

**For technical diagrams, include evidence artifacts** — real code snippets, actual JSON payloads, real event/method names from specs. See the Evidence Artifacts section.

---

## Design Process

### Step 0: Assess Depth
Simple/conceptual or comprehensive/technical? If comprehensive — research actual specs, formats, and terminology first.

### Step 1: Understand Deeply
For each concept, ask: What does it DO? What relationships exist? What's the core transformation? What would someone need to SEE?

### Step 2: Map Concepts to Patterns

| If the concept...              | Use this pattern                                    |
|-------------------------------|-----------------------------------------------------|
| Spawns multiple outputs        | **Fan-out** — radial arrows from center              |
| Combines inputs into one       | **Convergence** — funnel, arrows merging             |
| Has hierarchy/nesting          | **Tree** — lines + free-floating text                |
| Is a sequence of steps         | **Timeline** — line + dots + labels                  |
| Loops or improves continuously | **Spiral/Cycle** — arrow returning to start          |
| Is an abstract state/context   | **Cloud** — overlapping ellipses                     |
| Transforms input to output     | **Assembly line** — before → process → after         |
| Compares two things            | **Side-by-side** — parallel with contrast            |
| Separates into phases          | **Gap/Break** — visual separation between sections   |

### Step 3: Ensure Variety
Each major concept uses a different visual pattern. No uniform cards or grids.

### Step 4: Sketch the Flow
Mentally trace how the eye moves. There should be a clear visual story.

### Step 5: Generate JSON
Only now create the Excalidraw elements. For large diagrams, build section-by-section (see below).

### Step 6: Render & Validate (Claude Code only — MANDATORY)
Run the render-view-fix loop. See Render & Validate section.

---

## Evidence Artifacts

Concrete examples that prove accuracy and help viewers learn. Include in technical diagrams.

| Artifact Type        | When to Use                       | How to Render                                       |
|---------------------|-----------------------------------|-----------------------------------------------------|
| Code snippets        | APIs, integrations, implementation | Dark rect + syntax-colored text                     |
| Data/JSON examples   | Schemas, payloads, formats         | Dark rect + green text                              |
| Event sequences      | Protocols, workflows, lifecycles   | Timeline (line + dots + labels)                     |
| UI mockups           | Showing actual output              | Nested rectangles mimicking real UI                 |
| API/method names     | Real function calls, endpoints     | Use actual names from docs, not placeholders         |

**Key principle**: Show what things actually look like, not just what they're called.

---

## Multi-Zoom Architecture

Comprehensive diagrams operate at multiple zoom levels:

**Level 1 — Summary Flow**: Simplified overview of the full pipeline. Often at top or bottom.

**Level 2 — Section Boundaries**: Labeled regions grouping related components. Visual "rooms".

**Level 3 — Detail Inside Sections**: Evidence artifacts, code snippets, concrete examples. Where educational value lives.

For comprehensive diagrams, include all three levels.

---

## Container vs. Free-Floating Text

Default to free-floating text. Add containers only when they serve a purpose.

| Use a Container When...                    | Use Free-Floating Text When...            |
|-------------------------------------------|-------------------------------------------|
| It's the focal point of a section          | It's a label or description               |
| It needs visual grouping with other elements | It's supporting detail or metadata        |
| Arrows need to connect to it               | It describes something nearby             |
| The shape itself carries meaning            | Typography alone creates sufficient hierarchy |
| It represents a distinct "thing"            | It's a section title or annotation        |

**The container test**: For each boxed element, ask "Would this work as free-floating text?" If yes, remove the container. Aim for <30% of text elements inside containers.

---

## Large Diagram Strategy

**Build JSON one section at a time.** Do not attempt the entire file in a single pass — it leads to worse quality and may exceed output limits.

### Section-by-Section Workflow

**Phase 1 — Build each section:**
1. Create the base file with JSON wrapper and first section of elements
2. Add one section per edit — take your time with layout and spacing
3. Use descriptive string IDs (e.g., `"trigger_rect"`, `"arrow_fan_left"`)
4. Namespace seeds by section (section 1: 100xxx, section 2: 200xxx)
5. Update cross-section bindings as you go

**Phase 2 — Review the whole:**
Check cross-section arrow bindings, overall spacing balance, and that all IDs reference existing elements.

**Phase 3 — Render & validate** (Claude Code) or final review (Claude Chat).

---

## Shape Meaning

| Concept Type                  | Shape                          | Why                        |
|------------------------------|--------------------------------|----------------------------|
| Labels, descriptions, details | **none** (free-floating text)  | Typography creates hierarchy|
| Section titles, annotations   | **none** (free-floating text)  | Font size/weight is enough  |
| Markers on a timeline          | small `ellipse` (10-20px)      | Visual anchor, not container|
| Start, trigger, input          | `ellipse`                      | Soft, origin-like           |
| End, output, result            | `ellipse`                      | Completion, destination     |
| Decision, condition            | `diamond`                      | Classic decision symbol     |
| Process, action, step          | `rectangle`                    | Contained action            |
| Abstract state, context        | overlapping `ellipse`          | Fuzzy, cloud-like           |
| Hierarchy node                 | lines + text (no boxes)        | Structure through lines     |

---

## Color, Aesthetics & Layout

**Colors**: Read `references/color-palette.md` before generating any diagram. Every color choice comes from there.

**Roughness**: `0` for clean/modern (default), `1` for hand-drawn/informal.

**Stroke width**: `1` thin/elegant, `2` standard (default), `3` bold emphasis (sparingly).

**Opacity**: Always `100`. Use color, size, and stroke width for hierarchy instead.

**Hierarchy through scale**: Hero 300×150, Primary 180×90, Secondary 120×60, Small 60×40.

**Whitespace = importance**: Most important element gets 200px+ of empty space around it.

**Flow direction**: Left→right or top→bottom for sequences, radial for hub-and-spoke.

**Connections**: Position alone doesn't show relationships. If A relates to B, there must be an arrow.

---

## Text Rules

The JSON `text` property contains ONLY readable words. No formatting codes.

```json
{ "id": "myElement1", "text": "Start", "originalText": "Start" }
```

Settings: `fontSize: 16`, `fontFamily: 3`, `textAlign: "center"`, `verticalAlign: "middle"`

---

## JSON Structure

```json
{
  "type": "excalidraw",
  "version": 2,
  "source": "https://excalidraw.com",
  "elements": [...],
  "appState": { "viewBackgroundColor": "#ffffff", "gridSize": 20 },
  "files": {}
}
```

See `references/element-templates.md` for copy-paste JSON templates per element type.
See `references/json-schema.md` for the full schema reference.

---

## Render & Validate (Claude Code — MANDATORY)

After generating JSON, render to PNG, view it, and fix — in a loop.

```bash
cd <skill-path>/references && uv run python render_excalidraw.py <path-to-file.excalidraw>
```

### The Loop

1. **Render & View** — Run script, read the PNG
2. **Audit against vision** — Does the visual structure match the plan? Correct hierarchy? Evidence artifacts readable?
3. **Check for defects** — Text clipped/overflowing, overlapping elements, arrows through elements, ambiguous labels, uneven spacing, text too small, lopsided composition
4. **Fix** — Edit JSON. Widen containers, adjust coordinates, add arrow waypoints, reposition labels
5. **Re-render & re-view**
6. **Repeat** — Typically 2-4 iterations. Stop when it passes both vision and defect checks.

---

## Quality Checklist

### Depth & Evidence (Technical Diagrams)
- [ ] Research done — actual specs, formats, event names
- [ ] Evidence artifacts — code snippets, JSON examples, real data
- [ ] Multi-zoom — summary + sections + detail
- [ ] Concrete over abstract — real content, not labeled boxes

### Conceptual
- [ ] Isomorphism — visual structure mirrors concept behavior
- [ ] Argument — shows something text alone couldn't
- [ ] Variety — each concept uses a different visual pattern
- [ ] No uniform containers — no card grids or equal boxes

### Container Discipline
- [ ] Minimal containers — <30% of text in boxes
- [ ] Lines as structure — timelines/trees use lines + text, not boxes
- [ ] Typography hierarchy — font size and color create hierarchy

### Structural
- [ ] Connections — every relationship has an arrow/line
- [ ] Flow — clear visual path for the eye
- [ ] Hierarchy — important elements larger/more isolated

### Technical
- [ ] `fontFamily: 3`, `roughness: 0`, `opacity: 100`
- [ ] `text` contains only readable words
- [ ] Rendered and visually validated (Claude Code)
