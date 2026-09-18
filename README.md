# FigJam Builder

A skill for Claude Code, Codex, and other compatible agents that builds and modifies FigJam boards through the Figma Plugin API. It brings opinionated layout, typography, color, and native editing guidance to boards that should feel expressive and be easy to scan.

## Why this exists

FigJam earns its keep when spatial layout matters: side-by-side comparisons, multi-team status views, boards that accumulate content over time. Building them by hand is slow when you already know what should be on them. This skill gives an agent visual principles and construction patterns for making those boards clear, playful, and useful in a working session.

## What it produces

Boards (new files, or edits to existing ones) built from FigJam primitives:

- Native sections, shapes with editable text, stickies, tables, connectors, and images
- Paired typography and a coherent palette that preserve hierarchy at overview zoom
- Section hierarchy that descends from hero to supporting to appendix
- Layouts that match the shape of the container they sit in
- Expressive color, artwork, and intentional looseness without obscuring facts or arrows
- Usable contribution space, with representative note-entry and connector move/resize checks
- Charts with honest scales, clear labels, and accessible series identities

## Requirements

- A connected Figma MCP integration with `use_figma` and board inspection/screenshot tools. Media upload, file creation, and generated diagrams additionally require their corresponding tools.
- The `figma-use` and `figma-use-figjam` companion skills loaded before any `use_figma` call, using the names exposed by your client. They carry the Plugin API rules this skill builds on. Load the file-creation or diagram companion skill before using those tools.
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

### Codex

```bash
git clone https://github.com/prasantloki/figjam-builder.git ~/.codex/skills/figjam-builder
```

### Other clients (Cursor, VS Code, Copilot CLI, etc.)

Install the whole skill folder, including `SKILL.md` and `references/`, in your client's skills or context directory. The reference guides are required for the relevant workflows. Refer to your client's documentation for the exact location.

### Claude API

Upload via the `/v1/skills` endpoint. See the [Skills API docs](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview).

## Usage

Ask your agent naturally, with a FigJam URL when you have one:

- *"Build a status board for these projects: figma.com/board/..."*
- *"Add a section to this board comparing the three directions: figma.com/board/..."*
- *"Fix the layout on the bottom half: figma.com/board/..."*
- *"Take these meeting notes and build a workshop board"*

Without a URL, the agent can create a new FigJam file via `create_new_file` when that tool is available and return the link.

## How it works

1. Reads the brief or the existing board, then picks the layout pattern that fits the content
2. Picks a palette and type scale up front so the board reads cohesively across sections
3. Builds section by section, sizing containers to content in greenfield mode, fitting content to containers in modification mode
4. Verifies between major steps with screenshots and reworks layout when it's off
5. Checks native text fit, contribution space, and representative connector behavior before delivery

## Reference guides

- [Composition](references/composition-grammar.md): glance order, compact grouping, and traceable connections
- [Typography](references/typography-grammar.md): paired roles, wrapping, and overview readability
- [Primitives](references/primitive-grammar.md): choosing native forms and handling runtime limitations
- [Expression](references/expressive-grammar.md): playful atmosphere without sacrificing clarity
- [Data visualization](references/data-viz-grammar.md): chart selection, scales, and accessible labels

Runtime-specific observations in the guides are not a guarantee of support in every Figma client. Follow the connected tool's current API and verify the rendered result.

## Customization

The skill is opinionated about visual judgment. If your house style differs, fork and adjust:

- The palette is a three-tier hue system (accent / vibrant / muted). Replace hues to match your design system.
- The type scale uses Inter at fixed sizes for hero, section, body, and metadata. Swap font or shift sizes; the ratios matter more than the absolute pixels.
- The pattern catalog at the end of `SKILL.md` names the board types this skill recognizes. Add your own labels and recipes.

## License

MIT
