---
name: figjam-builder
description: Build OR modify FigJam boards via the Figma Plugin API. Use when asked to create, edit, extend, restructure, add content to, or fix layout in a FigJam board — initial creation OR follow-up changes. Triggers on "make a FigJam", "build a board", "add to this FigJam", "update this FigJam", "fix the layout", "/figjam-builder", or any time the user shares a FigJam URL (figma.com/board/...) and wants visual or content changes.
---

# FigJam Builder

Build or modify FigJam boards via the Figma Plugin API. Use whenever the work involves visual or content changes to a FigJam — initial creation, edits, extensions, layout fixes, or restructuring.

**Prerequisites:** Load `figma-plugin:figma-use` AND `figma-plugin:figma-use-figjam` before calling `use_figma`. `figma-use` covers Plugin API rules, `figma-use-figjam` covers FigJam-native node types (sticky, shape-with-text, connector, section, table, label).

**When to load this skill.** Build, edit, extend, restructure, or add content to a FigJam board — not just initial creation. If the user shares a FigJam URL (`figma.com/board/...`) and asks for any visual or content change, load this. Skip only for one-off tweaks to a single existing node (recolor one rectangle, edit one text node) that don't need composition guidance.

---

## When to Use FigJam

FigJam earns its keep when spatial layout matters: comparing things side-by-side, showing multi-team status at a glance, or building a board that accumulates content over time. If you're driving a single decision through a linear argument, use Slides or Docs.

---

## Part 1: Design Principles

Nine things to hold in your head. The rest of this skill is how they show up in code.

**Direction of fit follows ownership.** Greenfield mode — the wrapper is yours — content drives container size. Build at ideal sizes; hug containers to content in the reflow pass. Modification mode — fixed container, eval slot, parent section you don't own — container shape is an input. Read width × height *both* (not just one axis), then pick a layout that suits the shape. Wide+short → fewer wider cards or horizontal strips, with cards grown tall enough to fill the vertical. Tall+narrow → vertical stacks. Most failures here come from reading only width and leaving the container half-empty vertically — if reformatting leaves obvious whitespace on either axis, grow card heights, reflow content, or pick a denser layout. Don't force a default layout into a frame that doesn't fit; check ownership before you commit.

**Decoration doesn't carry weight.** When something matters — a decision, a callout, a primary section — signal it directly through scale, weight, color, or position. Eyebrows are category tags, not emphasis. A small label above an important thing makes the thing feel smaller.

**The first frame teaches the eye, but volume descends — it doesn't crash.** Whatever opens a board or section sets the volume; the rest of the board sits a tier quieter, not silenced. The opening is the loudest moment, supporting sections carry their own claims at reduced weight, appendix content quieter still. A giant hero with bland everything-else reads as a single firework, not a piece.

**Hierarchy reads through ratio, weight, and color together.** Size is one of three levers, not the whole game. A subtitle at 75% of its heading still feels weak if it's also Regular and gray. Push two levers when you want hierarchy to register, all three when you don't want it missed.

**Enumeration form should match meaning.** Numbers say "sequence" or "of N" — use them when order or count is the point. Bullets say "set" — use them when items are peers and the list could be reordered without losing meaning. Number a decision flow; bullet a feature list.

**Form serves function — uniformity kills curatorial signal.** Equal grids work when items are peers and the comparison is the job. Varied sizes work when judgment is the artifact — a moodboard's exact 3×3 grid flattens the curator's voice. Pick the structure that carries the meaning.

**Geometry is a communication primitive.** Circles read identity. Squares read evidence. Skinny portrait cards read as content blocks; near-square cards read as image-dominant. Pick shape to differentiate role, not as default.

**Visual mass should match argument weight.** A small chart under a big headline reads as "not the point." If the chart is the point, size it like it. Every visual element claims a level of importance through its size — be honest about what's primary.

**Spatial relationships are part of the argument.** Overlap, crossings, and tight routing degrade the read. Generative diagram tools optimize for compactness, not breathing room — when the flow matters, hand-build it or accept the limitation.

---

### Type scale

Use **Inter** exclusively. Three levers control hierarchy: ratio, weight, color. Subtitles work best at 70-75% of their parent heading size, with weight (Bold vs Regular) and color (black vs gray) doing the role differentiation. When a subtitle looks weak, it's usually under-sized AND under-weighted — push ratio toward 80% and consider Medium instead of Regular before reaching for spacing changes.

**Default to the upper end of each range.** The floor of a range is the smallest the role can be without breaking. The middle is conservative; the upper end is the right default for FigJam boards, which get viewed at fit-zoom AND zoomed-in during meetings. Drop below the upper end only when a body block would outweigh its headline or a long subtitle needs to come down a tier to avoid wrapping.

**Wrap on phrase boundaries, not mid-clause.** A subtitle that breaks "the three architectural / options under review" reads worse than the same subtitle at one tier smaller that fits on a line, or sized to a wider container. If the wrap point lands awkwardly: shorten the text, drop a tier, widen the container, or break manually with `\n` at a natural phrase boundary — never ship the awkward wrap.

| Role | Size (default upper end) | Weight |
|------|--------------------------|--------|
| Board title | 96-144px | Bold |
| Board subtitle | 56-88px | Regular |
| Section heading | 64-96px | Bold |
| Section subtitle | 48-64px | Regular |
| Card title | 48-64px | Semi Bold |
| Body text | 32-48px | Regular |
| Metadata | 24-32px | Regular/Medium |

Eyebrows (small all-caps category labels) aren't in the scale. They contradict the "decoration doesn't carry weight" principle — if you reach for one, you're probably looking for emphasis and should use scale + weight instead.

### Color semantics

Every hue exists in three tiers (full palette block below in Part 2). The hue carries the meaning; the tier carries the role.

| Hue | Meaning | Most common tier |
|-----|---------|------------------|
| Yellow / gold | Attention — "look here." Neutral urgency, not negative. | Accent for marks; vibrant rarely (only for explicit attention callouts) |
| Orange | Problem / trending wrong | Accent + muted; vibrant for warning sections when scoped |
| Red | Critical / broken | Accent + muted; vibrant for one critical zone per board |
| Green | Healthy / shipped | Accent for status dots; muted for positive-state zones; vibrant only for an explicit success callout |
| Blue | Informational / in-progress | Most common zoning hue — muted blue is the default neutral section bg |
| Pink | Decision needed | Muted for aligned-outcome decisions (the common case); vibrant only for action-blocking moments |
| Purple | Exploration / ideation | Muted for zoning; vibrant for an active brainstorm column |
| Teal | Decision captured | Mostly vibrant (sticky); muted for "we agreed" zones |

**White cards inside muted containers.** That's the dominant pattern — most sections are muted-tier, cards inside are white, and the visual rhythm comes from alternating warm/cool muted tints.

**Vibrant tier is signal, used sparingly.** When a section's job is "pay attention here" (decision zone, critical callout, active brainstorm), it gets a vibrant background and the white-card-inside pattern stops applying — the section itself is the claim. Cap at one vibrant section per board.

**Key rules:**
- Gold = "look here." Red = "this is bad." Don't use red for attention.
- Vibrant tier ≠ wash. Sections washed in vibrant compete with each other; pick one to carry the signal.
- **Color difference alone isn't signal.** A hue change with no scale or weight change reads as noise, not emphasis. When you switch a section's tier or hue to signal something, also push type weight or size — color carried by undersized type doesn't register.
- **Coral/salmon (pinkVibrant, redVibrant) is the loudest move you can make. Don't default to it — even for decisions.** Reserve it strictly for *action-blocking* moments: the team is BLOCKED, the decision is alarm-bell urgent, the room has to do something before they leave. Most "decisions" on a PM board are aligned outcomes — the room already knows where it's heading, the board confirms. Those get a **muted decision treatment**: `pinkMuted` background, accent-pink left stripe or border, recommendation text at section-heading scale with strong weight, supporting rationale below. Color is the category tag; scale + weight do the emphasis. For "this is important but not urgent," reach for the softer vibrants first: `yellowVibrant` (sunshine, neutral attention), `blueVibrant` (active/discussion), `purpleVibrant` (lavender, exploration), `tealVibrant` (mint, decision-captured), `orangeVibrant` (peach, trending-wrong). Coral is for the alarm bell, not the conclusion.

### Proportion and alignment

- Size sections to fit content (greenfield); fit content to the section (modification) — see Direction of Fit principle
- Center elements in rows on the same y-axis
- Center content in portrait/vertical cards
- Position badges relative to text, not section edges
- At least 12-16px breathing room between title and body

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
  contentPadding: 24, // inside cards
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

**Rule of tier:** Pick by job. Vibrant for signal, muted for zones, accent for marks. White cards live inside muted sections; vibrant sections carry their own claim and don't need card-on-zone contrast. Cap one vibrant section per board — more than one and you're back to the circus.

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

### API surface

**Native primitives:** `createText`, `createFrame`, `createRectangle`, `createEllipse`, `createLine`, `createStar`, `createPolygon`, `createVector`, `figma.union()` / `figma.subtract()`

**FigJam-specific:** `createSticky`, `createShapeWithText`, `createConnector`, `createSection`, `createTable`, `createCodeBlock`, `createNodeFromSvg`

**Not available:** `createComponent`, `createComponentSet`

### Choosing the right node type

| Need | Use | Why |
|------|-----|-----|
| Flowchart node with label | `createShapeWithText` | Built-in text centering, connector endpoints |
| Card | `createSection` | Native FigJam grouping with background fill, nests inside parent sections |
| Badge / pill | `createFrame` + auto-layout + text | Precise padding, radius, auto-centered text |
| Data viz (donut, bar, sparkline) | `createEllipse`/`createRectangle`/`createVector` | Native shape primitives |
| Emphasis marker with text | `createFrame` container + shape + text | Frame guarantees centering |
| Emphasis marker (no text) | `createPolygon`/`createStar`/`createEllipse` | Triangle, starburst, dot |
| Top-level zone | `createSection` | FigJam native grouping, zoom-to behavior |
| Divider | `createRectangle` at 1-2px height | Simple |

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

Frames behave the same way for their children (frame-relative coordinates), but they also support auto-layout.

### Text

Load Inter Bold / Regular / Semi Bold / Medium before any `createText` call. For body, set `resize(440-520, 10)` then `textAutoResize = 'HEIGHT'`. Rich text via `setRangeFontName` and `setRangeHyperlink`.

### Data visualization

Use when a number has context to convey (percentage, trend, rating). Not every metric needs a visualization — a big number with a one-line caption often wins. Size the viz to its role: hero when it's the point of the card, inline when it's supporting evidence.

| Data shape | Primitive | Build |
|------------|-----------|-------|
| Bounded percentage (retention, completion) | Progress ring / donut | `createEllipse` + `arcData` (innerRadius 0.55-0.7 for donut, 0 for pie, half-arc for gauge) |
| Trend over time (WAU, revenue) | Sparkline | `createVector` with smooth SVG path, stroke only, no fill |
| Score on a scale (rating) | Star row | N x `createStar({ pointCount: 5, innerRadius: 0.4 })`, filled vs gray |
| Project completion | Progress bar | Gray bg rect + colored fill rect, `cornerRadius: 6`, height ~12 |
| Number speaks for itself | Just the number | Large text, optional one-line caption |

Don't use progress rings for unbounded metrics — they imply a 100% ceiling. For trend or raw count, sparkline or big number.

### Images

An "image" in FigJam is anything visual that wasn't drawn natively on the canvas — a photo, a Figma file or chart screenshot, a device mock, a wireframe, a logo. Each carries its own meta-text: the chart has axis labels, the screenshot shows its own UI, the device mock includes brand framing. Don't re-label the image; bridge it to the argument only if a bridge is needed.

The card is the argument step. Headlines stay dominant; images support them. Inside a card the image flexes — sometimes the full surface, sometimes a small inline glyph, sometimes a mosaic, sometimes nothing. Size each image to what it needs to communicate, not to a card template.

**Shape carries role.** A circle reads as identity (avatar, portrait); a square reads as evidence (chart, screenshot, photo); a tall rectangle reads as content. Image-dominant cards (hero shots, screenshots, mocks) want near-square aspect — skinny portrait cards make the image fight the frame. Match the card aspect to the image's job: square-ish for evidence, portrait only when the card is text-led with a supporting visual.

**No source? Default to skipping.** Don't fabricate placeholders or filler. Generate a placeholder only when the user's ask explicitly calls for one — template skeletons, wireframes, mock layouts where the placeholder *is* the artifact. When you're tempted to invent, ask for a source.

**Upload flow.** Call `mcp__figma__upload_assets({ fileKey, count: N })` for N single-use `submitUrl`s (max 5 per call, 10-min expiry). POST each image to its `submitUrl` as multipart/form-data. The response includes `placedOnNodeId` you can reference in a follow-up `use_figma` call to resize, position, or parent.

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

Style header rows with Bold weight and tinted fill. Size table width to match section width minus padding. Don't leave tables floating in whitespace.

**Express the comparison, don't just lay it out.** A correctly-built but visually flat table reads as scaffolding. When comparison patterns matter — and they usually do, otherwise pick a different primitive — push the table to surface them: vibrant-tier header row (not muted) for high-signal comparisons; alternating row fills to anchor the eye across columns; cell-level tints to call out winners, blockers, or status (green cell for "yes/healthy," red for "no/blocked," muted for neutral). Don't color every cell — pick the dimension that carries the verdict and color that column or row. A baseline tinted-header-only table is the floor; a table that uses color to *answer the question* is the finished artifact.

### Pull quotes

For editorial use (research synthesis, customer interviews, sourced statements), build a dedicated quote card — `createSection` with an opening glyph or color stripe, the quote at body-text size, attribution at metadata size, and an optional one-line context underneath. Stickies are for *live* discussion; pull quotes are for *captured* voice.

### Stickies

For discussion, not editorial content. Color semantics: blue=discussion, yellow=question, green=positive, pink=concern, red=blocker, teal=decision, violet=ideation.

**Always lay out stickies in a grid.** Rows and columns, consistent 64px gap, aligned to the top-left of the usable inset. Stickies are 240x240 (square) or 416x240 (wide, for longer text). These sizes are fixed; stickies cannot be resized. Never stagger, overlap, or let stickies touch section edges.

### Diagrams and connectors

For flowcharts, sequence diagrams, ERDs, state machines, and gantts, use `mcp__figma__generate_diagram` — it handles layout and routing. Load the `figma-generate-diagram` skill first.

`generate_diagram` optimizes for compactness. Nodes pack tight, connectors take the shortest route, and labels can land on top of other paths. That's fine for utility flows but degrades fast when the diagram is meant to be read carefully — the spatial relationships *are* the argument. When breathing room matters (decision trees in a review, customer journeys, anything you'd zoom into during a meeting), hand-build with `createShapeWithText` + `createConnector` and space nodes deliberately. Plan for 200-400px between nodes vertically, 300-500px horizontally, label gaps off the connector paths, and route arrows around content rather than through it. One-way arrows: `connectorStartStrokeCap = 'NONE'`, `connectorEndStrokeCap = 'ARROW_LINES'`.

**Dress the output, don't ship it naked.** `generate_diagram` drops its nodes at the canvas root with default styling — usable but unframed, and the nodes themselves are white-on-white. Dressing happens in two layers, and you need both. *Frame layer* (around the diagram): tinted section background matching role, strong title block at section-heading scale with the diagram's claim, optional one-line context note, accent stroke or stripe. *Content layer* (inside the diagram): color the nodes by role — request/response, success/fail, sync/async — using accent-tier fills with white text, OR if generate_diagram's output styling resists customization, hand-build the diagram with colored `createShapeWithText` nodes instead. A diagram that's well-framed but visually monotone inside still reads as naked; the frame is necessary but not sufficient. Aim for both layers to land.

**Centering hygiene for hand-built diagrams.** Connectors out of a decision node should originate from the node's center, not an arbitrary edge. Branch nodes in a row should sit on the same y-axis and be centered on the path coming from above. Off-axis branches make even well-spaced flows feel sloppy.

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
- Equalize card heights within rows where cards share a content pattern
- Resize each section to hug its children (greenfield only — measure rightmost/bottommost content edge + padding)
- Resize the board wrapper to hug all sections (greenfield only)

### 4. Audit pass
- No overflow (child exceeds parent bounds)
- No overlap (consecutive text nodes collide)
- Section names cleared
- Spacing grid compliance
- Type scale compliance — subtitles wrap on phrase boundaries, not mid-clause
- Color consistency — tier picks match section role (zoning sections muted, signal sections vibrant capped at one)
- Centering hygiene — connectors from decision nodes centered on the decision; objects in rows centered to each other on a shared axis; badges centered relative to text, not container edges
- Volume descent — does the board have an opening louder than the rest? Are supporting sections quieter but not silenced?

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
- **Eyebrows carrying weight.** "OPTION A," "CRITICAL," "DECISION" labels above an important card make the card feel smaller. If it matters, signal with scale, weight, color, or position — not a small all-caps label.
- **Color difference without scale difference.** Switching a section's tier or hue but keeping the type the same reads as noise, not emphasis. When you push color, push type too.
- **Volume crash.** A giant hero with bland everything-else reads as a single firework. Supporting sections carry their own claim at reduced weight, not silenced.
- **Over-emphasis.** One or two markers per section, max. Five "important" things means none are.

### Color failures
- **Coral defaulting.** `pinkVibrant`/`redVibrant` for any "decision" or "important" moment is the loudest move on the board — reserve it for *action-blocking* moments. Most decisions are aligned outcomes; they get muted treatment (`pinkMuted` bg + accent-pink stripe + scale).
- **Mono-container palette.** Every container or row washed in the same hue (lightBlue everywhere, soft-yellow everywhere) flattens the rhythm. Vary muted tints across rows — peach, lavender, mint, blue, yellow — to give each section its own room.
- **Cap one vibrant section per board.** More than one and you're back to the circus.
- **Red for attention.** Red = "this is bad," not "look here." Use yellow/gold for neutral attention.
- **Green for large backgrounds.** Reserve green for small status indicators and "we shipped this" callouts.

### Layout & spacing failures
- **Half-empty containers in modification mode.** Reading only width and leaving 25-50% of the vertical empty is the #1 mode-rule failure. Read both axes; grow card heights to fill.
- **Stretching a card to fill its parent.** Card width should serve readability, not container math. Hug to content in greenfield; fit shape to container in modification mode.
- **Overlap or crossings in flowcharts.** `generate_diagram` optimizes for compactness. When the flow matters, hand-build with 200-400px vertical and 300-500px horizontal between nodes.
- **Stickies staggered or touching edges.** Always a grid with consistent gap (default 64px), aligned to the top-left of the usable inset.
- **Skipping the wrapper section.** Every board is one movable unit; skipping it makes future rearrangement painful.
- **Skipping the entry point.** Every board needs a visible title.
- **Sizing participatory zones to their pre-filled content.** Size for expected activity, not what's already there.

### Type failures
- **Body below 32px or metadata below 24px.** Default to the upper end of each range. Drop only when a body block would outweigh its headline.
- **Subtitle too small or same size as body.** Subtitles want 70-75% of their parent heading. If a subtitle looks weak, push toward 80% and consider Medium over Regular.
- **Em dashes in board text.** Periods, commas, or restructure.
- **Awkward wraps.** A subtitle that breaks mid-clause reads worse than the same subtitle one tier smaller. Shorten, drop a tier, widen the container, or break manually with `\n`.
- **Bullets where order matters.** Number the steps when sequence is the point. Bullet only when items are peers and could be reordered without losing meaning.

### Image & data failures
- **Placeholder rectangles by default.** No source? Skip the image entirely. Generate a placeholder only when the ask is "show the shape of this brief I'll fill in tomorrow."
- **Equal-grid moodboards.** A perfect 3×2 of equal squares flattens the curator's voice. Vary tile sizes; one hero tile claims the eye.
- **Skinny portrait card with a landscape image.** Card aspect matches the image's job. Image-dominant content lives in near-square or slight-landscape cards.
- **Cards when comparison is the job.** A 5-card grid of 4-line bodies is unscannable across dimensions. Pick a table when the explicit job is cross-cutting comparison.
- **Decoration creep.** Hero-sized avatars, icons in their own cards, decorative shapes without a role. Every shape and image earns its size from what it communicates.
- **Left-aligned vertical metric cards.** Center hero numbers and text.
- **Progress rings for unbounded metrics.** Rings are for percentages. For trend or raw count, use a sparkline or big number.

### Diagram failures
- **Naked `generate_diagram` output.** Default is white-on-white at the canvas root. Dress in two layers: *frame* around (tinted bg, title block, optional accent stroke), *content* inside (color nodes by role with accent fills + white text, or hand-build with `createShapeWithText`). One layer alone still reads as raw output.

### Build hygiene
- **Adding `section.x` to a child's coordinate.** `child.x = section.x + 40` makes the child render at `section.x + (section.x + 40)`, way outside the section. SECTION children's coordinates are ALREADY interpreted as offsets from the section's top-left. Set `child.x = 40` to put the child 40px inside the section. Order vs `appendChild` doesn't matter. This is the single most common eval failure mode.
- **Don't build the entire board in one `use_figma` call.** Work incrementally. Each call accomplishes one logical chunk.
- **Always `textAutoResize = 'HEIGHT'`.** Never guess text height. Set the prop, then reflow downstream.
- **Reset full range before re-coloring text markers.** Setting `node.characters` inherits position-0 fill across new text; reset before re-coloring inline accents.
- **One text node per multi-line list.** Use `\n`-separated content in a single text node, never one node per bullet. Separate nodes drift, mis-space, and force manual positioning.
- **Centering text over shapes.** Use a frame container with auto-layout. Never manual x/y math.
- **Clear section names.** Sections render their name in a small label at the top — clear it unless there's no title text inside.
- **Use stickies for discussion, not editorial.** Text nodes for narrative; stickies for participatory content.
- **Never combine page switching with heavy computation.** A `setCurrentPageAsync` call should do minimal additional work.
- **Don't let text overlap.** Reflow after setting content. This is a critical bug.
