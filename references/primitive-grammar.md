# FigJam primitive grammar

Read this reference when a board needs a structural choice: greenfield composition, major reflow, comparison, diagram, data view, participatory area, media, code, or a custom visual metaphor.

The governing idea is simple: primitives are an information vocabulary. Pick the form that makes the relationship legible at overview zoom, then style it to fit the board. Availability alone is never a reason to use one.

## Selection order

1. State the section's claim or job in one sentence.
2. Identify the relationship the reader needs to see: sequence, comparison, dependency, hierarchy, participation, literal syntax, evidence, magnitude, or atmosphere.
3. Choose one dominant grammar from the table below.
4. Add at most one supporting family of marks unless the section is explicitly an exploratory primitive study.
5. Check that every extra representation answers a different question. If a table, diagram, and card grid repeat the same content, keep the strongest one.
6. Give the dominant form enough visual mass to carry the section. A technically correct table, diagram, code block, or chart is still unfinished when it sits as a small specimen in a large empty field.

Prefer the least elaborate form that fully communicates the relationship. Start with type and layout, move to a native semantic primitive, and reach for generated or custom geometry only when it adds meaning.

## Finish the chosen form

Primitive selection is the first decision, not the finish line. Screenshot the section at overview zoom and judge the visible result.

- Scale the main artifact to match the argument. Reflow when the primary content is confined to one corner or a large part of the section is unintentionally empty.
- Remove arrows, markers, and labels that merely restate adjacency or reading direction. Keep them when transition, causality, sequence, or a remote relationship is part of the meaning.
- Do not add comparison dimensions that the source does not support. A concise synthesis may explain the supplied facts, but it must not look like an additional measured criterion.
- Reserve visible capacity in participatory areas and label its purpose. Empty slots should read as an invitation for live input, not unfinished placeholder furniture.
- Keep text paths and connector labels clear of nodes and other paths. If they cannot remain readable at overview zoom, simplify or remove them.
- Treat the screenshot as authoritative. If the node tree is semantically correct but the rendered output clips, overlaps, disappears, or duplicates content, the board is not done.

## Content job to native form

| Content job | Dominant form | Why |
|---|---|---|
| Tell an editorial story or frame a claim | Sections, text, and content-sized cards | Reading order and hierarchy do the work |
| Compare the same fields across several items | Native `createTable()` | Rows and columns expose cross-cutting differences |
| Show dependencies, hierarchy, or a deliberate flow | `createShapeWithText()` + `createConnector()` | Node roles and edges carry the argument |
| Generate a standard process, sequence, state model, ERD, or gantt quickly | `generate_diagram`, then dress and validate | Mermaid handles base layout; FigJam styling supplies emphasis and context |
| Collect live input, votes, or unresolved questions | Stickies in a strict grid | The object signals that the content is participatory and movable |
| Preserve code, payloads, queries, or protocol syntax | `createCodeBlock()` | Literal formatting and syntax highlighting matter |
| Ground a claim in source UI, a chart, a photo, or other media | `upload_assets` with the real source | Evidence should remain recognizable as evidence |
| Show bounded progress, trend, rating, or a raw number | Native data shapes | The visual encoding matches the data's mathematical meaning |
| Number steps or connect callouts to a remote legend | Small ellipse labels | Compact markers preserve the main composition |
| Enact a visual metaphor or create a compact identity mark | Semantic shapes first; SVG, vector, or boolean geometry second | Custom form earns its complexity by making the idea memorable |
| Put language on a cycle, orbit, or boundary | `createTextPath(vector, startSegment, startPosition)` | The path itself carries meaning |

Do not use `createTextPath()` as decorative curved display type. Use it when following the path explains the system: a lifecycle, loop, orbit, boundary, or circular sequence. The current API requires a `VectorNode`, start segment, and start position; a zero-argument call fails atomically. Keep the copy short and verify legibility with a screenshot.

## Semantic shape vocabulary

`createShapeWithText()` offers more than generic boxes. Choose a shape only when its conventional meaning helps the reader. A rounded rectangle remains the right neutral default.

| Meaning | Shape types | Use with care |
|---|---|---|
| Neutral step or entity | `SQUARE`, `ROUNDED_RECTANGLE`, `ELLIPSE` | Ellipses read as starts, ends, or identity more readily than generic steps |
| Decision or branch | `DIAMOND` | Keep branch labels off crossing paths |
| Input or manual action | `PARALLELOGRAM_RIGHT`, `PARALLELOGRAM_LEFT`, `MANUAL_INPUT` | Direction should match the flow |
| Document or file artifact | `DOCUMENT_SINGLE`, `DOCUMENT_MULTIPLE`, `ENG_FILE`, `ENG_FOLDER` | Pick product-neutral document shapes unless engineering specificity matters |
| Data store or retained state | `ENG_DATABASE`, `INTERNAL_STORAGE` | A cylinder implies persistence, not just any service |
| Queue or asynchronous handoff | `ENG_QUEUE` | Use only where queueing or buffering is real |
| Reused subprocess | `PREDEFINED_PROCESS` | The doubled edge implies a defined subroutine |
| Safety, protection, or policy gate | `SHIELD` | Do not turn every review step into a shield |
| Hard blocker or stop condition | `OCTAGON` | Red is still reserved for a genuinely negative status |
| Merge, addition, or logical junction | `PLUS`, `SUMMING_JUNCTION`, `OR` | Best in technical or systems diagrams where the symbol is understood |
| Direction or transition | `ARROW_LEFT`, `ARROW_RIGHT`, `CHEVRON` | Connectors are usually better for relationships between objects |
| Annotation or quoted aside | `SPEECH_BUBBLE` | Captured research quotes still belong in pull-quote cards |
| Special emphasis | `STAR`, `PENTAGON`, `HEXAGON`, `TRAPEZOID`, triangles | Treat as sparse accents unless their geometry has a defined role |

Use `TRIANGLE_UP` and `TRIANGLE_DOWN` only for a literal directional shift, warning, or navigation cue. Never invent a semantic legend after the fact to justify a shape choice.

## Structural primitives

- `createSection()` owns board-level grouping, nesting, and zoom-to behavior. Clear the visible section name when an internal title exists.
- `figma.createAutoLayout()` creates reflow-safe local clusters. Use it inside cards or sections for title/body stacks, badge rows, legends, and other relationships that should survive copy edits. Do not replace the board's section hierarchy with a forest of frames.
- `createFrame()` is useful for custom overlays, centered text-on-shape constructions, and small layout containers.
- `createRectangle()`, `createEllipse()`, `createLine()`, `createPolygon()`, `createStar()`, `createVector()`, and `createNodeFromSvg()` cover custom marks and data visualization.
- `figma.union()`, `figma.subtract()`, `figma.intersect()`, `figma.exclude()`, and `figma.flatten()` can build a custom silhouette. Prefer a semantic native shape when it already exists.

## Tables

Choose a table when readers need to scan across repeated dimensions. Do not translate every list into rows and columns.

- Give the table a question or claim, not a generic "comparison" label.
- Use a vibrant header only when the comparison itself is a high-signal section.
- Use alternating row fills for tracking, then tint only the cells that carry the verdict.
- Resize rows and columns for the content. A native table that is technically correct but floating in whitespace is unfinished.
- Keep the case for each decision direction inline with that direction. A detached comparison table should not recreate the numbering mismatch the decision-board pattern avoids.

## Connected models and generated diagrams

Hand-build when spacing, metaphor, or meeting readability is the argument. Use `generate_diagram` when a standard Mermaid grammar can establish the base faster.

For hand-built diagrams:

- Let shape semantics distinguish node roles before color does.
- Use connectors for relationships, not decorative underlines.
- Keep branch nodes aligned and route edges around content.
- Use labels on connectors only when the relationship itself needs a verb or condition.
- On cycles, keep the directional edges continuous and traceable. Curved text can label the loop, but it does not replace arrowed connectors or another explicit directional path; detached arrowheads are too ambiguous.

For generated diagrams:

- Load the `figma-generate-diagram` skill first.
- Dress both layers: a framed section with a claim, plus role-aware styling inside the diagram.
- Generated connectors may retain off-canvas bend geometry after their nodes are reparented. Screenshot the destination section. If its rendered bounds explode, keep the nodes and recreate the connectors inside the section with local endpoints.

## Code, references, and media

Code blocks are for content that must remain literal. Use them for source snippets, JSON payloads, SQL, GraphQL, shell commands, and protocol examples. Prose that merely describes a technical idea remains prose.

Before assigning `code`, load the code block's renderer font. In the current staging runtime that is `{ family: "Source Code Pro", style: "Medium" }`; setting `code` without the load throws even though older FigJam references say code blocks need no font setup. Verify the font if the runtime changes.

After assigning code, screenshot the block. The current staging renderer can retain a valid `.code` value while rendering an empty block, and it may later render the native text again. Never stack fallback text directly over a live native code block: that can produce two colliding copies. If the native block is blank, replace or hide it and use one dedicated dark frame or section containing a single Source Code Pro text node with the exact literal content. The final screenshot must show exactly one readable copy.

The MCP harness exposes shared Plugin API typings that include `createLinkPreviewAsync`, `createImage`, and `createImageAsync`, but those entry points are not supported for this workflow. Use `upload_assets` for FigJam media. For a reference link without an asset, use an ordinary linked text node or a compact source chip.

No source means no invented screenshot or filler rectangle. Placeholders are appropriate only when the requested artifact is itself a template or wireframe.

## Primitive budget and restraint

- One dominant grammar per section.
- One or two emphasis markers per section.
- Keep one clear first-glance focus; several vibrant workshop zones can work when grouping and reading order remain clear.
- Stickies stay participatory; pull quotes stay editorial.
- Data shapes follow mathematical meaning: rings for bounded percentages, sparklines for trends, big numbers for raw counts.
- Custom vectors, SVG, boolean marks, and text paths must communicate something the standard composition cannot.
- Reserve workshop capacity as a labeled open field or a small number of obvious add-here affordances. A grid of empty card outlines reads as unfinished output, even when it is meant as future capacity.

The best board may still use only sections, text, and one marker. Expanded capability should raise semantic precision, not primitive count.

## Runtime-verified surface

Verified in a staging FigJam file on 2026-08-28:

- All documented `ShapeWithText` variants above render in FigJam.
- `createTextPath()` works in FigJam.
- `createTextPath()` requires a vector node plus start segment and start position; a zero-argument call fails.
- `figma.createAutoLayout()` works for local frame clusters in FigJam.
- Boolean shape operations including `intersect()` work in FigJam.
- Setting `CodeBlockNode.code` requires `Source Code Pro Medium` to be loaded in the current runtime.
- A code block can still render blank after a valid assignment. Screenshot it; if fallback text is needed, keep exactly one visible code layer rather than overlaying two renderers.
- `createLinkPreviewAsync()` is present in shared typings but rejected by the MCP execution harness; do not use it.

If the runtime contradicts this list, trust the runtime. Run a minimal controlled test before changing the rule.
