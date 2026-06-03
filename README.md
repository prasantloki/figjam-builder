# FigJam Builder

A Claude skill that builds and modifies FigJam boards through the Figma Plugin API. It teaches Claude how to compose boards using FigJam's type scale, color, and layout patterns so the output reads as designed instead of generated.

## Why this exists

FigJam earns its keep when spatial layout matters: side-by-side comparisons, multi-team status views, boards that accumulate content over time. Building them by hand is slow when you already know what should be on them. Asking an LLM without guidance produces boards that read like generic output (uniform grids, decorative emoji, every section the same volume). This skill closes that gap by giving Claude a set of visual principles to hold in its head and concrete patterns to apply.

## What it produces

Boards (new files, or edits to existing ones) built from FigJam primitives:

- Sections, cards, stickies, connectors, text, and images
- A type scale and palette picked from a defined system, not freestyled
- Section hierarchy that descends from hero to supporting to appendix
- Layouts that match the shape of the container they sit in
- Emphasis markers used sparingly, only where they earn their weight

## Requirements

- Figma MCP tools enabled: `use_figma`, `get_figjam`, `upload_assets`, `create_new_file`, `generate_diagram`
- The `figma-plugin:figma-use` companion skill loaded before any `use_figma` call. It carries the Plugin API rules this skill builds on.
- A target FigJam file URL, or none if you want a new file created

## Installation

### Claude Code

```bash
git clone https://github.com/prasantloki/figjam-builder.git ~/.claude/skills/figjam-builder
```

### Claude.ai (Team / Enterprise)

1. Download the latest zip from the [Releases](../../releases) page (or clone this repo and zip the root folder)
2. Go to **Settings → Customize → Skills → "+" → "+ Create skill"**
3. Upload the zip

### Other clients (Cursor, VS Code, Copilot CLI, etc.)

Add `SKILL.md` to your client's skills or context directory. Refer to your client's documentation for the exact location.

### Claude API

Upload via the `/v1/skills` endpoint. See the [Skills API docs](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview).

## Usage

Ask Claude naturally, with a FigJam URL when you have one:

- *"Build a status board for these projects: figma.com/board/..."*
- *"Add a section to this board comparing the three directions: figma.com/board/..."*
- *"Fix the layout on the bottom half: figma.com/board/..."*
- *"Take these meeting notes and build a workshop board"*

Without a URL, Claude creates a new FigJam file via `create_new_file` and returns the link.

## How it works

1. Reads the brief or the existing board, then picks the layout pattern that fits the content
2. Picks a palette and type scale up front so the board reads cohesively across sections
3. Builds section by section, sizing containers to content in greenfield mode, fitting content to containers in modification mode
4. Verifies between major steps with screenshots and reworks layout when it's off

## Customization

The skill is opinionated about visual judgment. If your house style differs, fork and adjust:

- The palette is a three-tier hue system (accent / vibrant / muted). Replace hues to match your design system.
- The type scale uses Inter at fixed sizes for hero, section, body, and metadata. Swap font or shift sizes; the ratios matter more than the absolute pixels.
- The pattern catalog at the end of `SKILL.md` names the board types this skill recognizes. Add your own labels and recipes.

## License

MIT
