# FigJam composition grammar

Read this reference for any greenfield board, major reflow, fixed-container layout, dense section, or connected model. Composition allocates attention: it determines what the reader notices first, what they understand next, and what can wait.

## Plan the glance order

Before placing nodes, write three short lines:

1. First glance: the one claim, decision, or object that should win within two seconds.
2. Second glance: the evidence or relationship that advances the argument.
3. Last glance: risks, metadata, appendix, or other material that should remain available without competing.

Build one focal object for the first glance. Repeating the primary sentence in both a board title and a hero card creates two focal objects for one idea and wastes the second glance. If the title already states the claim, let the hero carry evidence, consequence, or the next move.

Translate meta-language into hierarchy. Do not put phrases such as "the single most important message is" on the canvas; make that message visibly primary instead.

## Use a focus budget

- One primary focal object per section.
- Two or three secondary objects may support it.
- Everything else should be quieter through smaller scale, lower contrast, simpler containers, or later position.

Use position and visual mass first, typography second, and color as reinforcement. A collection of equally tinted cards may be organized but still lack a focal point. Supporting cards should not become peers with the decision merely because their statuses use different hues.

## Space by relationship

Spacing communicates grouping before labels do. Use a clear progression:

- 16-32px between title, body, and metadata within one text stack.
- 32-48px of card padding for normal narrative content. Use 24px only for compact labels or chips.
- 60-92px between peer objects in the same thought.
- 160-240px between distinct groups inside one section.
- Keep related comparisons in one labeled wrapper, typically 160-240px apart; use larger separations only when the user wants distinct board regions.

These are starting ranges, not a substitute for looking. Long lines, large type, or visually heavy shapes need more room. Unrelated groups should never be closer than the text blocks inside a card.

## Construct text without collisions

Text height is an output, not an estimate.

1. Load the font and set the final characters, width, font size, line height, and weight.
2. Set wrapping text to `textAutoResize = 'HEIGHT'`.
3. Read the resulting `height` before positioning the next node.
4. Place the next block from `previous.y + previous.height + gap`, or use `figma.createAutoLayout()` for the stack.
5. After any copy, font, width, or weight change, re-read heights and reflow everything downstream.

Do not place later text from an estimated line count. A title that gains one line can collide with every fixed-y object beneath it. Avoid final-word orphans and one-word lines in primary copy; shorten, widen, or reduce one type tier while keeping the role floor.

## Treat negative space as owned

Empty space is useful when it isolates the focal point or separates groups. It is accidental when it is trapped inside an oversized card, leaves all content in one corner, or forces long connector runs through an otherwise empty field.

- In greenfield work, hug the wrapper to the finished composition.
- In fixed containers, redistribute visual mass across both axes. Enlarge the primary object, change the layout, or move groups; do not stretch low-content cards merely to occupy space.
- Keep peer cards content-fit unless equal height materially helps comparison. A fixed wrapper may contain open space around a compact group; it does not require every card interior to become empty space.
- Check the whole section as a silhouette. If one quadrant is dense and the opposite quadrant contains only routes or empty card fill, rebalance before styling details.
- Asymmetry is welcome when it makes priority visible. It should look chosen, not like unfinished packing.

For participatory boards, reserve contribution space before examples and decoration. In each distinct activity layout, insert a temporary native note with a realistic multi-line contribution at the intended reading size. Inspect its actual bounds and a screenshot: the note and prompt must remain readable without covering examples or rearranging the board. Remove the probe and verify cleanup. A labeled empty strip is not useful capacity. If insertion or inspection fails, report capacity as unverified rather than passing it from appearance alone.

Measure the populated note, not the blank sticky: native notes can grow vertically with realistic copy. Check the resulting bottom edge against prompts, footers, and section bounds. Prefer a clear contribution area within the existing composition before expanding the canvas.

## Route connected models for tracing

Collision-free routing is only the floor. A meeting-ready model lets a reader trace each important relationship without guessing which line owns a shared trunk.

1. Place the main reading backbone first.
2. Put shared dependencies near the nodes that consume them.
3. Add secondary branches after the backbone is clear.
4. Start with node-bound auto or side magnets. Two edges may arrive at the same node boundary if their directions remain distinguishable; separate ports are optional, not a requirement.
5. Avoid shared trunks when they make edge ownership ambiguous. First separate routes through node placement or connector sides. Do not invent junctions or trade resize-safe attachments for a tidier static screenshot.
6. Keep labels at the 24px metadata floor, offset from paths and nodes. Remove labels when the endpoints already make the relationship obvious.
7. Use quieter color and weight for analytics, infrastructure, or other secondary edges so the primary path survives overview zoom.

Before declaring the graph clean, trace every supplied source-to-target pair from end to end in the rendered screenshot. Record a failure if the eye can jump from one relationship onto another, if a route appears to end in open space, or if a decorative line can be mistaken for an extra edge. Programmatic bounding-box checks do not prove edge ownership; this trace test is visual.

Keep labels native to connected shapes and bind endpoints to nodes. Do not solve clipping by leaving independent label siblings behind. Prefer `AUTO` or explicit side magnets; custom positions require checking the available API's coordinate contract, not guessing from current node dimensions.

For a newly built or rerouted connected model, check one representative multi-edge node:
- Capture the target node ID, parent, original geometry, incident endpoints, and a before screenshot. Do not probe unrelated or protected content.
- Move the node, then enlarge it enough to expose stale attachment coordinates. Inspect both states without repairing connectors between captures.
- Confirm the native label moves with the card and remains fully visible, endpoints track its boundary, arrowheads retain their meaning, and neighboring content stays usable. If a custom port ends inside the resized card, replace it with an edge magnet and adjust the layout before rechecking.
- Inspect nearby non-incident routes too: a resized card can obstruct another relationship even when its own endpoints remain attached. Record attachment and route clearance separately, including the tested dimensions. A smaller passing resize does not erase a larger stress-test failure or establish arbitrary resize safety.
- Restore geometry and any changed endpoint settings in a cleanup path even if inspection fails; verify restoration and remove temporary nodes. Record the same target IDs with the observed result. A missing screenshot or a mismatched target means unverified, not passed.

Static tracing and editing are separate checks. A stored endpoint ID alone proves neither. A copy-only edit does not require rerunning unaffected connector probes.

Do not remove a required source-to-target relationship merely to make the graph cleaner. A grouped lane, table, or explicit source-and-target statement may replace individual edges only when every supplied relationship remains unambiguous and directly traceable.

If one area is crowded while another holds only long connector runs, change the topology rather than nudging labels. A different row, column, or duplicated small reference node can be clearer than perfect graph-theory purity.

## Run a geometry audit

After the reflow pass, audit actual bounds before trusting the screenshot.

- Collect visible content nodes: text, shape-with-text, sticky, code, table cells, and meaningful custom marks.
- Compare unrelated node pairs by absolute bounding box. Any positive-area overlap requires inspection; ancestor-container overlap is expected and should be excluded.
- For vertically consecutive text blocks in the same parent whose horizontal spans overlap, require at least 12px and prefer the 16-32px text-stack range.
- Check every non-connector child against its intended parent bounds.
- Recheck type floors and primary-copy wraps after the final width is known.
- Judge connector crossings from the rendered screenshot; a connector bounding box does not describe its path.
- Use explicit node-type checks before text getters; some properties throw on unsupported node types. On edits, inventory hidden as well as visible obsolete content and preserve protected nodes exactly.

The audit should cover all content pairs, not only consecutive text nodes. Semantic shapes, table cells, badges, and fallback code treatments can collide with text while a text-only check still passes.

## Run the two-zoom review

Take two screenshots:

1. Overview zoom: identify what actually wins first, second, and last. If that order differs from the plan, revise scale, position, contrast, or grouping.
2. Reading zoom: verify comfortable line length, clean phrase-boundary wraps, breathing room, connector labels, and absence of clipping or overlap.

Then ask four questions:

- Is any important statement repeated instead of advanced?
- Does each gap correctly say "same thought" or "new group"?
- Is negative space helping focus, or exposing unfinished layout?
- Would removing one card, tint, label, or connector make the argument clearer?

A structurally valid board can still be compositionally unresolved. Ship only when both zoom levels work.
