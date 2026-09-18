---
name: figjam-builder
description: Build or modify FigJam boards via the Figma Plugin API. Use for creating, editing, extending, restructuring, or fixing the layout of a FigJam board. Triggers on "make a FigJam", "build a board", "update this FigJam", "fix the layout", "/figjam-builder", or a FigJam URL paired with a visual or content change.
---

# FigJam Builder

Build or modify FigJam boards via the Figma Plugin API. Use whenever the work involves visual or content changes to a FigJam — initial creation, edits, extensions, layout fixes, or restructuring.

**Prerequisites:** Load the `figma-use` AND `figma-use-figjam` skills, using the names available in the current harness, before calling `use_figma`. `figma-use` covers Plugin API rules, `figma-use-figjam` covers FigJam-native node types (sticky, shape-with-text, connector, section, table, label).

**When to load this skill.** Build, edit, extend, restructure, or add content to a FigJam board — not just initial creation. If the user shares a FigJam URL (`figma.com/board/...`) and asks for any visual or content change, load this. Even a one-node copy or color edit can change wrapping, hierarchy, or balance; use the audit pass after the edit.

---

## When to Use FigJam

FigJam earns its keep when spatial layout matters: comparing things side-by-side, showing multi-team status at a glance, or building a board that accumulates content over time. If you're driving a single decision through a linear argument, use Slides or Docs.

---

## Part 1: Design Principles

Eleven things to hold in your head. The rest of this skill is how they show up in code.

**Direction of fit follows ownership.** Greenfield mode — the wrapper is yours — content drives container size. Hug containers to the finished composition. Modification mode — fixed container, eval slot, parent section you don't own — read both width and height, then choose a layout that fits. Keep cards content-fit rather than stretching them to consume empty space. Reserve real contribution space before placing workshop examples. In comparisons, keep labeled alternatives together in one compact wrapper with modest gaps; do not scatter them across the canvas.

**Expression is part of the work, not finishing garnish.** Make a workshop feel inviting before anyone reads the details. Commit to a visual idea in the composition: a bold illustrated opening, a playful relationship map, or a tactile working area. Scale, weight, color, and position still carry important decisions; decoration need not encode data to earn its place.

**The first frame teaches the eye, but volume descends — it doesn't crash.** Whatever opens a board or section sets the volume; the rest of the board sits a tier quieter, not silenced. The opening is the loudest moment, supporting sections carry their own claims at reduced weight, appendix content quieter still. A giant hero with bland everything-else reads as a single firework, not a piece.

**Hierarchy reads through ratio, weight, and color together.** Size is one of three levers, not the whole game. A subtitle at 75% of its heading still feels weak if it's also Regular and gray. Push two levers when you want hierarchy to register, all three when you don't want it missed.

**Enumeration form should match meaning.** Numbers say "sequence" or "of N" — use them when order or count is the point. Bullets say "set" — use them when items are peers and the list could be reordered without losing meaning. Number a decision flow; bullet a feature list.

**Form serves function — uniformity kills curatorial signal.** Equal grids work when items are peers and the comparison is the job. Varied sizes work when judgment is the artifact — a moodboard's exact 3×3 grid flattens the curator's voice. Pick the structure that carries the meaning.

**Geometry is a communication primitive.** Circles read identity. Squares read evidence. Skinny portrait cards read as content blocks; near-square cards read as image-dominant. Pick shape to differentiate role, not as default. A metaphorical title is a promise: give it a literal visual move rather than styling the words and stopping there.

**Primitive choice follows the relationship.** Layout and type establish the reading order. Native forms make a specific relationship faster to see: a table exposes repeated fields, a connector exposes dependency, a sticky invites participation, and a code block preserves literal syntax. Use the least elaborate form that makes the relationship unmistakable. More available primitives should create more precise boards, not busier ones.

**Visual mass should match argument weight.** A small chart under a big headline reads as "not the point." If the chart is the point, size it like it. Every visual element claims a level of importance through its size — be honest about what's primary. Boldness is welcome when it clarifies the argument: use one dominant expressive move, then let the supporting system stay quieter.

**Expressive by default; precise where it counts.** For workshops and creative PM boards, make personality visible at overview zoom, not only in a tiny corner motif. Carry the visual idea through the opening, relationship map, and contribution area at different volumes. Use generous shapes, strong color fields, conversational prompts, illustration, or intentional asymmetry. Stickers, stamps, and doodles may provide delight without a tactical justification when supported by the API; never imply invented votes, approvals, or authorship. Keep factual labels and connector endpoints orderly, text uncovered, and working space usable. Serious reviews may call for quieter expression; do not force a whimsical template onto them.

**Spatial relationships are part of the argument.** Overlap, crossings, and tight routing degrade the read. Generative diagram tools optimize for compactness, not breathing room — when the flow matters, hand-build it or accept the limitation.

---

### Typography

Use **Inter** exclusively. Treat typography as a set of paired roles, not independent size ranges. A heading, its subtitle, and the gap between them form one editorial unit; the larger gap after that unit begins the next thought.

Read [references/typography-grammar.md](references/typography-grammar.md) for every greenfield board, major text reflow, leadership or editorial board, fixed-container layout, metric treatment, or section with more than two text roles. It defines the default type pairs, line heights, measures, stack spacing, optical alignment, and typography-specific audit.

For a small copy edit, preserve the existing local type system unless it is visibly broken. Always wrap on phrase boundaries, keep one-word orphans out of primary copy, and reflow downstream objects after any text change.

### Color semantics

Every hue exists in three tiers (full palette block below in Part 2). The hue carries the meaning; the tier carries the role.

| Hue | Meaning | Most common tier |
|-----|---------|------------------|
| Yellow / gold | Attention — "look here." Neutral urgency, not negative. | Accent for marks; vibrant rarely (only for explicit attention callouts) |
| Orange | Problem / trending wrong | Accent + muted; vibrant for warning sections when scoped |
| Red | Critical / broken | Accent + muted; vibrant for one critical zone per board |
| Green | Healthy / shipped | Accent for status dots; muted for positive-state zones; vibrant only for an explicit success callout |
| Slate-blue | Structural fact / serious but not urgent | Accent + muted; use for constraints and foundational realities that should not read as alarms |
| Blue | Informational / in-progress | Most common zoning hue — muted blue is the default neutral section bg |
| Pink | Decision needed | Muted for aligned-outcome decisions (the common case); vibrant only for action-blocking moments |
| Purple | Exploration / ideation | Muted for zoning; vibrant for an active brainstorm column |
| Teal | Decision captured | Mostly vibrant (sticky); muted for "we agreed" zones |

**Choose a palette, not a white-card template.** White cards in muted containers are one option, not the default for every board. Workshops can use confident color fields, colored native shapes, and bright contribution notes. Distinguish information colors from atmosphere: the semantic palette is a starting convention, not a ban on mint backgrounds or purple illustrations. Label states explicitly, keep their color meanings stable, and do not make decorative color look like status evidence.

**Vibrant tier is signal, used sparingly.** Keep one clear first-glance focus. Several vibrant regions can work in a playful workshop when they support coherent grouping rather than compete. Serious incident reviews usually need quieter warmth. Judge the rendered hierarchy, not a quota of colors or marks.

**Key rules:**
- Gold = "look here." Red = "this is bad." Don't use red for attention.
- Slate-blue = structural fact. Use it for serious constraints that need acknowledgment without the alarm signal of orange or red.
- Vibrant tier ≠ wash. Use strong color deliberately; multiple colorful activity zones are welcome when the reading order remains clear.
- **Color difference alone isn't signal.** A hue change with no scale or weight change reads as noise, not emphasis. When you switch a section's tier or hue to signal something, also push type weight or size — color carried by undersized type doesn't register.
- **Coral/salmon (pinkVibrant, redVibrant) is the loudest move you can make. Don't default to it — even for decisions.** Reserve it strictly for *action-blocking* moments: the team is BLOCKED, the decision is alarm-bell urgent, the room has to do something before they leave. Most decisions are aligned outcomes. Give them a muted treatment that reuses the board's existing recommendation or decision hue; use `pinkMuted` only when color is otherwise unassigned. Recommendation text at section-heading scale with strong weight carries emphasis. Add a border or stripe only when it encodes a real boundary or state. Coral is for the alarm bell, not the conclusion.

### Proportion and alignment

- Size sections to fit content (greenfield); fit content to the section (modification) — see Direction of Fit principle
- Center elements in rows on the same y-axis
- Center content in portrait/vertical cards
- Position badges relative to text, not section edges
- Use the role-pair spacing in the typography grammar; title-to-subtitle spacing must be tighter than subtitle-to-content spacing

### Entry point and claims

Board title at top-left, clearly visible at overview zoom — see the type scale above for size. For templates, add a meta/instructions section. For meetings, a colored agenda sticky. Three rough volumes per board (entry / supporting / appendix); see the "first frame teaches the eye" principle.

**Headlines work best as claims, not topics.** "Day 0 expectations double session length" lands harder than "Day 0 expectations." The headline should tell the reader what to believe; the column or section below is the proof. This applies at the board level and within sections.

### Match the move to the job

Use the dedicated tool when the shape fits. `generate_diagram` for Mermaid-grammar diagrams (flowchart, sequence, ERD, state, gantt). Tables for cross-cutting comparison — when you have N items × M dimensions, the table is almost always clearer than a card grid. Pull-quote cards for captured voice. The hand-built version loses when the shape matches the tool.

Tinted section backgrounds work when 3+ zones need distinct visual energy; a single-zone board doesn't need a tint. When a section reads as muted, reach for type weight and color before padding — muted boards are usually under-typed, not under-spaced.

---

## Part 2: Construction Rules

### Board structure

**Always wrap the entire board in one top-level white section.** This makes the board a single movable unit.

```js
const board = figma.createSection();
board.name = '';
board.resizeWithoutConstraints(estimatedW, estimatedH);
board.fills = [{ type: 'SOLID', color: {r:1, g:1, b:1} }];
// All content goes inside: board.appendChild(...)
```

Sizing rules follow the **Direction of fit** principle. Greenfield: choose card width from the text inside (body 400-1000px depending on density), derive section width from cards, derive board width from sections. Modification: divide the container — that's the input you were handed. Whichever mode, the greenfield instinct "3 columns inside the container" plus a wide container produces narrow columns with wasted space; let the actual shape pick the layout.

**Participatory zones size to expected activity, not current content.** Workshop sections, feedback areas, brainstorm columns exist for other people to fill. Pre-seed with a few example stickies to signal the pattern.

**Clear all section names** unless the section has no title text inside it.

### Reading direction and grouping

Left-to-right, top-to-bottom. Context on the left, evidence in the middle, proposal/asks on the right. Supporting detail and appendix below.

**Tight clustering** (60-92px) = same thought. **Loose spacing** (200-400px) = different topics. **Zone breaks** (1000px+) = different part of the board.

### Spacing grid

All spacing in multiples of 4px.

```js
const spacing = {
  sectionPadding: { top: 68, right: 60, bottom: 100, left: 80 },
  elementGapH: 60,    // between cards/columns
  elementGapV: 64,    // between stacked elements
  siblingGapH: 92,    // between sibling sections
  siblingGapV: 120,   // between section rows
  contentPadding: 32, // inside cards; 24 only for compact labels and chips
};
```

**Lay out inside the inset, not from one edge.** Compute usable area first (container size minus padding on all sides), then fit items within it.

### Color palette

Every FigJam hue has three tiers. They're not interchangeable — each tier does a specific job, and picking the wrong tier is what makes boards feel either washed-out or like a circus.

| Tier | Luminance | Job |
|------|-----------|-----|
| **Accent** | ~30% | Type, strokes, indicator dots, small fills. The "name" of the color. |
| **Vibrant** | ~75% | FigJam's native sticky palette. Signal sections, primary callouts, decision zones — places where the section IS the signal. |
| **Muted** | ~95% | Zone backgrounds that hold white cards. Organizational rhythm without competing with the content. |

**Rule of tier:** Pick by job. Vibrant for signal, muted for zones, accent for marks. White cards inside muted sections are a useful default, not a required recipe. Multiple vibrant zones can make a workshop inviting; judge their grouping and reading order rather than enforcing a one-section limit.

```js
// Neutrals
const black     = {r:0.07,  g:0.07,  b:0.07};   // #121212  accent text
const gray      = {r:0.35,  g:0.35,  b:0.35};   // #595959  secondary text
const grayMid   = {r:0.847, g:0.847, b:0.847};  // #D8D8D8  vibrant gray (signal divider, sticky)
const lightGray = {r:0.976, g:0.976, b:0.976};  // #F9F9F9  muted bg
const white     = {r:1,     g:1,     b:1};      // #FFFFFF  cards

// Yellow / gold — neutral attention ("look here")
const yellow       = {r:0.85, g:0.65, b:0.10};  // #D9A61A  accent (marks, eyebrows when warranted)
const yellowVibrant= {r:1,    g:0.886,b:0.388};  // #FFE163  vibrant (sticky, signal section)
const yellowMuted  = {r:1,    g:0.984,b:0.941};  // #FFFBF0  muted (zone bg)

// Pink — decision needed
const pink         = {r:0.70, g:0.20, b:0.45};  // #B3336E  accent
const pinkVibrant  = {r:1,    g:0.643,b:0.643};  // #FFA4A4  vibrant (decision zone, "act here")
const pinkMuted    = {r:1,    g:0.941,b:0.980};  // #FFF0FA  muted

// Green — healthy / shipped
const green        = {r:0.12, g:0.50, b:0.30};  // #1F804D  accent (status indicator)
const greenVibrant = {r:0.557,g:0.886,b:0.671};  // #8EE2AB  vibrant (positive sticky, success zone)
const greenMuted   = {r:0.922,g:1,    b:0.933};  // #EBFFEE  muted

// Blue — info / in-progress
const blue         = {r:0.22, g:0.40, b:0.75};  // #3866BF  accent
const blueVibrant  = {r:0.580,g:0.745,b:1};      // #94BEFF  vibrant (discussion sticky, neutral signal)
const blueMuted    = {r:0.961,g:0.984,b:1};      // #F5FBFF  muted

// Slate-blue — structural fact / serious but not urgent
const slate        = {r:0.28, g:0.34, b:0.48};  // accent
const slateChip    = {r:0.92, g:0.94, b:0.97};  // compact fill
const slateMuted   = {r:0.95, g:0.96, b:0.99};  // zone bg

// Violet — exploration / ideation
const purple       = {r:0.45, g:0.30, b:0.65};  // #734DA6  accent
const purpleVibrant= {r:0.780,g:0.710,b:1};      // #C7B5FF  vibrant (ideation sticky, exploration zone)
const purpleMuted  = {r:0.973,g:0.961,b:1};      // #F8F5FF  muted

// Teal — decision captured
const tealVibrant  = {r:0.557,g:0.918,b:0.886};  // #8EEAE2  vibrant (decision-captured sticky)
const tealMuted    = {r:0.945,g:0.996,b:0.992};  // #F1FEFD  muted

// Orange — regression / trending wrong
const orange       = {r:0.72, g:0.38, b:0.08};  // #B86114  accent
const orangeVibrant= {r:1,    g:0.722,b:0.475};  // #FFB879  vibrant (warning sticky, "trending wrong" callout)
const orangeMuted  = {r:1,    g:0.969,b:0.941};  // #FFF7F0  muted

// Red — critical / blocked
const red          = {r:0.75, g:0.18, b:0.18};  // #BF2D2D  accent (broken status indicator)
const redVibrant   = {r:1,    g:0.545,b:0.502};  // #FF8B80  vibrant (blocker sticky, critical zone)
const redMuted     = {r:1,    g:0.961,b:0.961};  // #FFF5F5  muted
```

**Reading the palette:** Each row is a hue with its three tiers. Accent stays consistent for text/marks across all sections. Vibrant matches FigJam's native sticky palette — using it for a section background reads as "this section has sticky energy" (active, immediate, signaling). Muted is the quiet zoning layer. White cards always sit inside muted (or inside white wrapper sections). For specific role-to-hue picks, see the Color semantics table in Part 1.

### Primitive selection pass

For greenfield boards, structural rewrites, diagrams, data views, or any request that could benefit from more than sections and text, read [references/primitive-grammar.md](references/primitive-grammar.md). It contains the supported API surface, the semantic shape vocabulary, and the escalation rules for tables, diagrams, code, media, and custom marks. For a small copy or position edit where the information form is staying intact, this extra read is unnecessary.

For greenfield boards, major reflows, fixed-container layouts, dense content, or connected models, also read [references/composition-grammar.md](references/composition-grammar.md). It turns spacing and hierarchy principles into collision-safe construction, attention ordering, balance, and overview-zoom checks.

For greenfield boards, major text reflows, leadership or editorial boards, fixed containers, metrics, or any section with more than two text roles, read [references/typography-grammar.md](references/typography-grammar.md). Use its paired defaults rather than inventing a new scale per section.

For expressive, data-rich, dashboard, chart, portfolio, visual-metaphor, or bold presentation requests, read [references/expressive-grammar.md](references/expressive-grammar.md). It defines how to allocate visual weight, use color and shape semantically, choose honest chart forms, vary motifs, and audit whether the visual move adds meaning.

For any chart, dashboard, metric comparison, funnel, time series, distribution, or set of percentages, read [references/data-viz-grammar.md](references/data-viz-grammar.md). It separates the analytical reading task from the expressive layer, defines honest encodings and scale rules, and prevents a bold treatment from outranking a clearer chart.

Before building, name the job of each section in plain language, then choose one dominant grammar for it. A section can be editorial cards, a comparison table, a connected model, a participatory field, a media composition, or a metric view. Supporting labels and marks are welcome; competing primary grammars usually mean the section has not made a choice.

**Modification mode starts inside the existing composition.** Use pills, labels, marks, connectors, type hierarchy, and local restructuring to strengthen sections already on the board. Add a section only when the user asked for new content or the information genuinely needs a new reading unit.

### Sections nest

A board is a section that contains zone sections, which contain card sections. Cards are just nested sections — same `createSection()` call, white fill, name cleared. Sections don't support auto-layout, so position children manually. Use frames only for badges, pills, or other small containers that need auto-layout to center text.

**Section children use section-local offsets, not page-absolute coordinates.** When you set `child.x = N` on a node parented to a SECTION, Figma renders the child at `section.x + N` — `N` is the offset from the section's top-left, not a position on the page. So:

```js
const child = figma.createText();
child.x = 40;   // 40px from section's left edge — NOT page x=40
child.y = 40;   // 40px from section's top — NOT page y=40
section.appendChild(child);
```

The visual position is `(section.x + child.x, section.y + child.y)`. To place a child 40px inside a section that lives at page `(800, 1200)`, set `child.x = 40` and `child.y = 40`. The child will visually appear at page `(840, 1240)`.

**Order of set vs appendChild does not matter** — the API stores the values you write, and rendering always treats them as section-local. The most common eval failure mode is writing `child.x = section.x + 40`, which doubles the offset and pushes content outside the section bounds.

Frames behave the same way for their children (frame-relative coordinates), but they also support auto-layout. Use `figma.createAutoLayout()` for reflow-sensitive clusters inside a card or section: title + body, a badge row, a compact legend, or a stack whose spacing should survive copy edits. Sections still own the board-level hierarchy and zoom behavior; auto-layout frames own local relationships.

### Text

Load Inter Bold / Regular / Semi Bold / Medium before any `createText` call. For body, set `resize(440-520, 10)` then `textAutoResize = 'HEIGHT'`. Rich text via `setRangeFontName` and `setRangeHyperlink`.

### Data visualization

Start with the question the reader must answer, then choose the most precise visual encoding for that reading task. Build the analytical substrate first. Add expressive hierarchy only after the chart can stand alone without the headline or callout.

Use a line for ordered change over time, aligned bars or dots on a shared scale for category comparison, and separate aligned scales for measures with different units or denominators. A progress ring is appropriate only for one bounded part-to-whole proportion. Multiple independent percentages, including non-exclusive feature adoption, require a shared comparison scale rather than several rings, a pie, a donut, or a 100%-stacked chart.

Read [references/data-viz-grammar.md](references/data-viz-grammar.md) before constructing any data view. Not every metric needs a chart; a large number with a concise label wins when there is no relationship to compare.

### Images

An "image" in FigJam is anything visual that wasn't drawn natively on the canvas — a photo, a Figma file or chart screenshot, a device mock, a wireframe, a logo. Each carries its own meta-text: the chart has axis labels, the screenshot shows its own UI, the device mock includes brand framing. Don't re-label the image; bridge it to the argument only if a bridge is needed.

The card is the argument step. Headlines stay dominant; images support them. Inside a card the image flexes — sometimes the full surface, sometimes a small inline glyph, sometimes a mosaic, sometimes nothing. Size each image to what it needs to communicate, not to a card template.

**Shape carries role.** A circle reads as identity (avatar, portrait); a square reads as evidence (chart, screenshot, photo); a tall rectangle reads as content. Image-dominant cards (hero shots, screenshots, mocks) want near-square aspect — skinny portrait cards make the image fight the frame. Match the card aspect to the image's job: square-ish for evidence, portrait only when the card is text-led with a supporting visual.

**No source? Default to skipping.** Don't fabricate placeholders or filler. Generate a placeholder only when the user's ask explicitly calls for one — template skeletons, wireframes, mock layouts where the placeholder *is* the artifact. When you're tempted to invent, ask for a source.

**Upload flow.** Call `upload_assets({ fileKey, count: N })` for N single-use `submitUrl`s (max 5 per call, 10-min expiry). POST each image to its `submitUrl` as multipart/form-data. The response includes `placedOnNodeId` you can reference in a follow-up `use_figma` call to resize, position, or parent.

### Emphasis markers

Use sparingly. One or two per section max. They work by breaking the visual pattern at overview zoom.

**Card-level:**
- Gold border + warm tint = "pay attention" (neutral)
- Red border + red tint = "off-track" (negative status only)
- Warning triangle (`createPolygon({ pointCount: 3 })`) pinned to top-right corner
- Notification dot (`createEllipse`) with count inside

**Section-level:**
- Starburst (`createStar({ pointCount: 8, innerRadius: 0.65 })`) with gold fill and text ("NEW", "UPDATED")
- Bullseye (concentric rings with decreasing opacity)

**Centering text over shapes:** Always use a frame container. Never position text with manual x/y math.
```js
const container = figma.createFrame();
container.resize(56, 56); container.fills = []; container.clipsContent = false;
const shape = figma.createStar(); shape.resize(56, 56);
container.appendChild(shape); shape.x = 0; shape.y = 0;
const text = figma.createText(); text.characters = 'NEW';
container.appendChild(text);
text.x = (56 - text.width) / 2; text.y = (56 - text.height) / 2;
```

**Flowchart emphasis:** Green Yes / Red No pills. Octagon for hard blockers. Diamond (rotated rect) for decisions.

### Tables

Style header rows with Bold weight and a high-contrast neutral or semantic fill. Size table width to match section width minus padding. Don't leave tables floating in whitespace.

**Express the comparison, don't just lay it out.** A correctly-built but visually flat table reads as scaffolding. Use a high-contrast header; choose a vibrant hue only when it encodes a real role. Alternate neutral row values to anchor the eye, then tint only cells that carry a supplied winner, blocker, or status. Pick the dimension that carries the verdict rather than coloring every cell. A table that visually answers the question is the finished artifact.

### Pull quotes

For editorial use (research synthesis, customer interviews, sourced statements), build a dedicated quote card — `createSection` with an opening glyph or color stripe, the quote at body-text size, attribution at metadata size, and an optional one-line context underneath. Stickies are for *live* discussion; pull quotes are for *captured* voice.

### Stickies

For discussion, not editorial content. Color semantics: blue=discussion, yellow=question, green=positive, pink=concern, red=blocker, teal=decision, violet=ideation.

**Arrange stickies for the activity.** Use aligned grids for direct comparison and loose clusters for ideation or affinity work. Small offsets can feel inviting; keep required text uncovered and groups legible. Inspect actual native sticky bounds rather than assuming a fixed size. Leave accessible room for new notes near their prompt, not just a thin empty footer.

### Diagrams and connectors

For flowcharts, sequence diagrams, ERDs, state machines, and gantts, use `generate_diagram` — it handles layout and routing. Load the `figma-generate-diagram` skill first.

For a carefully read dependency model, plan the exact source-to-target pairs before styling. Hand-build with native text-containing shapes and bound connectors when needed. Place shared prerequisites near consumers and choose node positions that give each edge a clear route; use the smallest gaps that preserve tracing and readable labels, not universal large spacing. Prefer straight routes when clear. One-way arrows use no start cap and a clearly visible end arrowhead. Trace every required direction in the screenshot; neither a connector count nor a collision-free bounding box proves clarity.

In a node-link view, any line or bar touching two nodes reads as an edge. Do not add a structural spine, divider, or decorative route that implies a relationship the source did not supply.

**Finish without over-dressing.** Give the model a clear title and coherent typography, contrast, and grouping. Neutral nodes can be beautiful and complete; saturated fills are optional. Add expressive character after routing is clear, and keep it out of connector corridors.

**Native editing is part of delivery.** Keep connected labels in `shape.text` where possible. Fix clipping by measuring, resizing, or reflowing, not by replacing native labels with unrelated text siblings. Inspect every required card body at reading zoom after final sizing, not only its heading. A custom grouped-label fallback must be tested.

**Prefer edge magnets over custom ports.** Start with node-bound `AUTO` or side magnets (`RIGHT` to `LEFT` for a left-to-right flow). Reposition nodes or change connector sides before introducing custom attachment positions. Distinct pixel-perfect ports are not worth arrows that detach on resize. Consult the loaded connector API reference before using custom positions; do not assume the coordinate units or derive permanent ports from the node's current width and height. Use custom ports only after a representative move-and-resize check proves they still meet the boundary. See [the routing check](references/composition-grammar.md#route-connected-models-for-tracing) for acceptance and restoration.

---

## Part 3: Workflow

### 0. Understand the ask
Purpose, audience, one-off vs recurring. Match to a common shape if it fits. Check ownership: greenfield or modification — that determines whether content drives container size or the other way around.

### 1. Plan the narrative
Outline beats/sections in plain text before writing code.

### 2. Build incrementally
**Greenfield first call:** Create the white wrapper section at a rough estimated size — you'll resize it in the reflow pass.
**Modification first call:** Read the container's existing width × height; let those drive your layout pick. Skip creating a wrapper.

Then for each sub-section:
1. Create cards and content first — size cards to fit their text in greenfield, to fit the container shape in modification
2. Create the container section sized to wrap those cards (greenfield only; modification reuses the parent)
3. Validate with `get_screenshot`. Fix before moving on.

### 3. Reflow pass
- `textAutoResize = 'HEIGHT'` on all text
- Resize cards to fit content
- Equalize card heights within rows only when comparison benefits; never shrink below the tallest required body
- Resize each section to hug its children (greenfield only — measure rightmost/bottommost content edge + padding)
- Resize the board wrapper to hug all sections (greenfield only)

### 4. Audit pass
- No overflow (child exceeds parent bounds)
- No overlap (consecutive text nodes collide)
- Section names cleared
- Consistent grouping and intentional looseness; compact comparison placement
- Type scale compliance — subtitles wrap on phrase boundaries, not mid-clause
- Color consistency — stable information semantics and a clear first-glance focus
- Attachment hygiene — visible arrowheads, unambiguous edge ownership, labels move with their nodes
- Volume descent — does the board have an opening louder than the rest? Are supporting sections quieter but not silenced?
- Primitive finish — is the dominant form large enough to carry the argument, with redundant marks removed and no accidental empty field?
- Render integrity — screenshot-driven check for clipped, missing, or duplicated content; literal code must appear exactly once and remain readable
- Participation capacity — insert a realistic multi-line native note in each distinct activity layout, inspect its actual bounds and screenshot, then remove it and verify cleanup; labeled space alone is not a pass
- Edge ownership — trace every required edge source-to-target in the screenshot; resolve ambiguous paths through placement or connector sides before custom ports, and verify boundary attachment after moving and resizing a representative node
- Edit integrity — preserve protected nodes and remove obsolete content, including hidden duplicates; use type-specific getters during inspection

---

## Part 3b: Reading Large Boards

When updating or extending an existing board, you need to read it first. `get_figjam` works for small-to-medium boards, but large ones (100+ nodes, sprawling retros, multi-team planning) produce screenshots where text is illegible. Use this escalation ladder:

### Step 1: Try `get_figjam` first

Call `get_figjam` with `includeImagesOfNodes: true`. If it returns readable text content, you're done.

### Step 2: If text is illegible or it times out, use node discovery

Break the board into individual nodes and screenshot them separately. This is the reliable path for large boards.

1. **Get child IDs (minimal payload).** Switch to the correct page and read only the child IDs of the target node. No names, no positions. This keeps the response small and fast:
   ```js
   await figma.setCurrentPageAsync(figma.root.children.find(p => p.id === 'PAGE_ID'));
   const node = figma.getNodeById('TARGET_NODE_ID');
   return JSON.stringify({ childIds: node.children.map(c => c.id) });
   ```

2. **Get metadata in batches of ~30.** For each batch, read id, type, name (truncated to 50 chars), and child count. Run batches in parallel where possible. Adding positions/sizes reduces safe batch size to ~15.

3. **Screenshot individual nodes.** Prioritize by type:
   - `SHAPE_WITH_TEXT` — often section headers or key messages
   - `STICKY` — substantive discussion content
   - `SECTION` — screenshot small ones directly; recurse into large ones (20+ children)
   - `TEXT` — standalone labels
   - Skip `CONNECTOR`, `STAMP`, and decorative nodes unless specifically needed

   `get_screenshot` works cross-page (no page switch needed). Run 4-6 screenshots in parallel.

4. **Synthesize.** Combine name previews from the metadata batches with the visual reads from screenshots.

### Timeout rules

- **Never combine page switching with heavy computation.** A `setCurrentPageAsync` call should do minimal additional work.
- **Page switch state does NOT persist between `use_figma` calls.** Every call that needs a non-default page must include `setCurrentPageAsync` at the top.
- **If a call times out, halve the batch size and retry.**
- **`get_screenshot` is the most reliable tool.** It never times out on individual nodes and returns highly readable images. When in doubt, screenshot it.

### For very large boards (200+ nodes)

Ask the user which sections or topics matter most rather than reading the entire board. Faster and more useful.

---

## Part 4: Common shapes

The board types you'll see most. Each is a combination of the rules above — not a recipe to copy, a label to recognize.

- **Decision board** (Context | Options | Decision) — three zones side-by-side. Most decisions are aligned outcomes; muted treatment on the decision zone unless action-blocking.
- **Exec review** — linear left-to-right story, 5-8 sections, alternating warm/cool muted tints, appendix below.
- **Status grid** — uniform team panels in a grid, each with the same sub-structure (KRs, projects, references).
- **Workshop / brainstorm** — pre-filled analysis + live sticky zone. Size sticky zone to expected activity, pre-seed with examples.
- **Vision board / moodboard** — narrative sections with mixed media. For moodboards, vary tile sizes — the variance is the artifact.
- **Competitive research** — screenshot-dominant spatial map with evidence tables.
- **Vertical metric cards** — portrait cards with centered content and one viz per card (ring, sparkline, star row).

---

## Anti-patterns

A final-pass checklist. The principles teach how to think; this is what reliably goes wrong. Scan before you ship.

### Emphasis failures
- **Expression lost to compliance.** At overview zoom, check whether the opening, relationship map, and contribution area feel intentionally designed and inviting—not just neat. If a no-skill comparison feels more alive, identify the useful compositional move and improve the skill; do not weaken the baseline or add token stickers to claim a win. Recheck factual visibility and connector traceability after the expressive pass.
- **Eyebrows carrying weight.** "OPTION A," "CRITICAL," "DECISION" labels above an important card make the card feel smaller. If it matters, signal with scale, weight, color, or position — not a small all-caps label.
- **Color difference without scale difference.** Switching a section's tier or hue but keeping the type the same reads as noise, not emphasis. When you push color, push type too.
- **Volume crash.** A giant hero with bland everything-else reads as a single firework. Supporting sections carry their own claim at reduced weight, not silenced.
- **Over-emphasis.** One or two markers per section, max. Five "important" things means none are.

### Color failures
- **Coral defaulting.** `pinkVibrant`/`redVibrant` for any "decision" or "important" moment is the loudest move on the board — reserve it for *action-blocking* moments. Most decisions are aligned outcomes; reuse the existing decision hue at muted intensity, or use `pinkMuted` only when color is otherwise unassigned.
- **Generic accent bars.** A left stripe on unrelated titles, cards, and metrics is a template habit, not hierarchy. Remove every bar whose semantic job cannot be named in one short phrase.
- **Arbitrary status-like palette cycling.** Avoid random hues that suggest unsupported differences between peers. Expressive activity zones and ornamental accents may use a coherent multi-color palette without pretending each hue is data.
- **Competing vibrant regions.** Reduce saturation when regions compete with the primary takeaway; playful grouping may use several colors.
- **Red for attention.** Red = "this is bad," not "look here." Use yellow/gold for neutral attention.
- **Color mistaken for evidence.** A green atmosphere is allowed; a green approval badge without actual approval is not. Wording must distinguish readiness, recommendation, and recorded decision.

### Layout & spacing failures
- **Accidental empty fields.** Read both container axes; rebalance compact groups without stretching cards or consuming reserved contribution space.
- **Stretching a card to fill its parent.** Card width should serve readability, not container math. Hug to content in greenfield; fit shape to container in modification mode.
- **Ambiguous flowchart routes.** Change topology or attachment points before increasing every gap. Trace actual rendered edges.
- **Unreadable sticky clusters.** Looseness is welcome; covered text, crowded edges, and inaccessible working areas are not.
- **Skipping the wrapper section.** Every board is one movable unit; skipping it makes future rearrangement painful.
- **Skipping the entry point.** Every board needs a visible title.
- **Sizing participatory zones to their pre-filled content.** Size for expected activity, not what's already there.

### Type failures
- **Independent type ranges.** Do not pick every role from a broad range. Start with one paired preset from the typography grammar and change it as a system.
- **Subtitle too small or too close to the heading.** The subtitle needs enough scale and weight to frame the section, plus a tighter gap to its heading than to the content below.
- **Em dashes in board text.** Periods, commas, or restructure.
- **Awkward wraps.** A subtitle that breaks mid-clause reads worse than the same subtitle one tier smaller. Shorten, drop a tier, widen the container, or break manually with `\n`.
- **Bullets where order matters.** Number the steps when sequence is the point. Bullet only when items are peers and could be reordered without losing meaning.

### Image & data failures
- **Placeholder rectangles by default.** No source? Skip the image entirely. Generate a placeholder only when the ask is "show the shape of this brief I'll fill in tomorrow."
- **Equal-grid moodboards.** A perfect 3×2 of equal squares flattens the curator's voice. Vary tile sizes; one hero tile claims the eye.
- **Skinny portrait card with a landscape image.** Card aspect matches the image's job. Image-dominant content lives in near-square or slight-landscape cards.
- **Cards when comparison is the job.** A 5-card grid of 4-line bodies is unscannable across dimensions. Pick a table when the explicit job is cross-cutting comparison.
- **Decoration creep.** Delight can earn space, but not at the expense of reading order, traceability, or participation capacity. Never fabricate social signals.
- **Left-aligned vertical metric cards.** Center hero numbers and text.
- **Progress rings for unbounded metrics.** Rings are for percentages. For trend or raw count, use a sparkline or big number.

### Diagram failures
- **Unfinished diagrams.** Finish hierarchy and grouping without requiring saturated nodes or extra framing. Preserve a good default route instead of restyling it into ambiguity.

### Build hygiene
- **Adding `section.x` to a child's coordinate.** `child.x = section.x + 40` makes the child render at `section.x + (section.x + 40)`, way outside the section. SECTION children's coordinates are ALREADY interpreted as offsets from the section's top-left. Set `child.x = 40` to put the child 40px inside the section. Order vs `appendChild` doesn't matter. This is the single most common eval failure mode.
- **Don't build the entire board in one `use_figma` call.** Work incrementally. Each call accomplishes one logical chunk.
- **Always `textAutoResize = 'HEIGHT'`.** Never guess text height. Set the prop, then reflow downstream.
- **Reset full range before re-coloring text markers.** Setting `node.characters` inherits position-0 fill across new text; reset before re-coloring inline accents.
- **One text node per multi-line list.** Use `\n`-separated content in a single text node, never one node per bullet. Separate nodes drift, mis-space, and force manual positioning.
- **Detached shape labels.** Prefer native `shape.text`; a custom label group must retain label placement and connector attachment during a move/resize check.
- **Native text that silently ellipsizes.** Shape bounds passing is not text-fit passing. Size native shapes for finished text, including heading line heights and padding; verify every last line in a close screenshot. Shorten copy without dropping facts or change topology before shrinking body text. Standalone `textAutoResize` does not prove native shape text fits.
- **Fractional connector ports.** Prefer named magnets (`RIGHT`, `LEFT`, `TOP`, `BOTTOM`) on bound endpoints. Do not treat endpoint `position` as normalized fractions. Trace rendered arrows and test a representative move/resize; a stored node ID alone does not prove boundary attachment.
- **Clear section names.** Sections render their name in a small label at the top — clear it unless there's no title text inside.
- **Use stickies for discussion, not editorial.** Text nodes for narrative; stickies for participatory content.
- **Never combine page switching with heavy computation.** A `setCurrentPageAsync` call should do minimal additional work.
- **Don't let text overlap.** Reflow after setting content. This is a critical bug.
