# FigJam typography grammar

Read this reference for every greenfield board, major text reflow, leadership or editorial board, fixed-container layout, metric treatment, or section with more than two text roles. Typography should make the information structure apparent before color or containers do.

## Start from paired roles

Use Inter throughout ordinary FigJam content. Load every weight before creating text. Start from the local preset below, then scale it for the actual section-fit viewing distance. A font size is only useful in relation to the canvas it lives on.

| Role | Local size / line height | Weight | Default use |
|---|---|---|---|
| Board title | 72 / 80px | Bold | The board's primary claim |
| Board subtitle | 36 / 48px | Regular or Medium | Scope or framing for the board title |
| Section heading | 52 / 60px | Bold | A claim or question introducing one section |
| Section subtitle | 28 / 38px | Regular or Medium | The sentence that frames what follows |
| Card title | 32 / 40px | Semi Bold | The card's claim, step, or object name |
| Body | 24 / 34px | Regular | Explanation and evidence |
| Metadata | 18 / 24px | Medium | Source, timestamp, owner, or appendix note |
| Compact metadata | 16 / 22px | Medium | Only for small chips or dense tables at reading zoom |

The local preset suits a compact section roughly 1200-1600px wide that will be read near 1:1. Most meeting boards are wider and must scale up. These are coordinated roles, not seven unrelated choices.

## Scale for section-fit viewing

Normalize the overview review to a maximum dimension of 1200px. Before building, estimate `reviewScale = min(1, 1200 / max(wrapperWidth, wrapperHeight))`. Divide the rendered-size targets below by `reviewScale` to get starting canvas font sizes.

| Role | Target size in the 1200px overview |
|---|---|
| Board title | 44-56px |
| Board subtitle | 24-32px |
| Section heading | 32-44px |
| Section subtitle | 20-28px |
| Card or evidence title | 16-22px |
| Body or support text | 13-18px |
| Metadata or source | 10-13px |

Use the lower end for dense peer content and the upper end for sparse editorial content. The floor is a legibility requirement, not a recommendation to make every role equally loud. After the first screenshot, adjust the connected roles together when the hierarchy feels too loud or too quiet.

Viewing distance overrides the nominal preset. A 2700px-wide leadership board may reasonably use a 96-112px board title, 52-64px subtitle, 32-40px evidence text, and 24-28px metadata because those sizes render much smaller at section-fit zoom.

Use one role per semantic job. A card title should not drift across peer cards. A subtitle should not become body text in one section and a second headline in another.

Preserve perceptual bands while scaling:

- Board subtitle: roughly 50-60% of the board title.
- Section subtitle: roughly 50-65% of the section heading.
- Card body: roughly 70-80% of the card title.
- Metadata: roughly 70-80% of body size, while staying above the overview floor.

These are starting bands. Weight, line height, color, density, and copy length can justify a small exception; record it rather than inventing a new tier silently.

## Build one editorial stack

A heading and subtitle should read as one thought. The space after the subtitle should announce that the next thought has begun.

Use these starting relationships:

- Board title to board subtitle: 20-28px.
- Section heading to section subtitle: 12-20px.
- Subtitle to the next content group: 48-72px.
- Card title to body: 16-24px.
- Body to metadata: 20-28px.

The exact pixel value is secondary to the relationship: space inside a thought is compact; space between thoughts is visibly larger. Do not use one generic vertical gap for the entire section.

Position each block from the measured height of the previous block. Set the final font, width, characters, line height, and `textAutoResize = 'HEIGHT'`; read the rendered height; then place the next block. Reflow every downstream object after a copy or width change.

## Control measure and wrapping

Typography is easiest to read when line length matches the role.

- Primary headings usually want 18-36 characters per line.
- Subtitles and body usually want 45-70 characters per line.
- Metadata should remain short; rewrite or widen it rather than creating a tiny paragraph.
- A card body should normally occupy two to five lines. If it becomes a narrow column of many short lines, widen the card or change the composition.

Prefer natural phrase boundaries. Do not strand a final word, split a product name, or break directly after an article or preposition. Fix the copy or measure before reducing the font size. Use a manual line break only when the phrase structure is stable and intentional.

## Use weight and color with restraint

- Bold establishes the primary claim.
- Semi Bold names cards, steps, and objects.
- Medium keeps subtitles and metadata present without making them headlines.
- Regular carries explanation.

Avoid using size, weight, and color contrast at maximum strength on the same secondary role. A subtitle can be smaller and Medium, or smaller and darker Regular. Making it smaller, gray, and light at once causes it to disappear.

Use near-black for primary text and a dark neutral gray for secondary text. Do not use low-contrast gray as a substitute for hierarchy. Leave Inter tracking at its default unless the runtime and the specific role have been visually tested.

Sentence case is the default. All-caps text is limited to tiny interface-like tokens whose category function is real; it should not carry a section's importance.

## Align optically

- Align narrative stacks to one left edge. A badge may sit outside that edge only when it clearly acts as a marker rather than the start of the sentence.
- Align peer card titles and bodies to the same internal inset.
- Center only content whose meaning benefits from it, such as a single metric or a compact portrait card. Long narrative text stays left-aligned.
- Give large numerals enough width that punctuation and percent signs do not force the visual center sideways.
- Compare the visible glyphs, not only node boxes. If a badge, numeral, or bold initial makes a mathematically aligned row look off, adjust the local container rather than moving unrelated objects.

## Let type carry the display

When a section is editorial, leadership-facing, restrained, or explicitly typography-led, prove the hierarchy with type and spacing before adding containers or color.

- Use at most one quiet rule, stripe, or marker for an editorial title stack. Prefer a short horizontal rule after the last role: it closes the stack without adding a second vertical axis. Use a vertical rule only when the stack is intentionally a callout; if used, span the complete title, subtitle, and metadata unit.
- Do not place a short answer inside an oversized field when a larger transition gap and stronger answer weight already separate it from the question. If a container adds real meaning, hug it to the text.
- When a large gap marks a real semantic jump, one short rule may make the transition feel deliberate. Keep it anchored to the shared left edge and roughly 5-12% of the text measure; a long line floating midway through the gap reads as another object rather than a transition cue.
- Keep peer cards and evidence rows neutral unless their colors encode a supplied difference. White or very light neutral row fills may support repeated scanning; several distinct hues create semantic-looking emphasis and weaken the common type rhythm.
- Use one restrained status accent on a metric: the numeral, a short rule, or a narrow stripe. Avoid a large tinted field when the value itself is meant to dominate.
- In two-column evidence rows, place the body column after the natural title measure plus a consistent gap. Do not push it toward the midpoint merely to fill the row.

Remove the surrounding treatment during the overview review and compare. If the hierarchy becomes clearer, the treatment was compensating for or competing with the typography.

## Treat metrics as a small family

A metric composition should not look like five arbitrary font sizes. Scale it from both the viewing distance and the surrounding hierarchy:

- Metric value: at least 2.25 times the section heading and usually 90-120px tall in the normalized overview.
- Metric label: 28-36% of the metric value, Semi Bold.
- Explanation: the board's normal body role, Regular with 1.35-1.45 line height.
- Source: the board's normal metadata role, Medium.

Keep value and label close enough to read as one fact. Separate the explanation by a larger gap, then let the source finish the stack quietly.

Left-align a metric when its label, explanation, and source form an editorial reading stack. Center it only when the card is genuinely numeral-led and the supporting copy is short enough to remain symmetrical.

## Review typography at two zoom levels

At overview zoom, check:

- Can the reader identify the board claim, section claim, and first evidence object in order?
- Does any title feel inflated merely to fill space?
- Do subtitles remain visible without competing with headings?
- Do peer cards still look like peers?
- Does every required support and metadata role meet its rendered overview floor?

At reading zoom, check:

- Are line height and measure comfortable?
- Do wraps follow phrases?
- Does the heading-to-subtitle gap say "same thought"?
- Is subtitle-to-content spacing visibly larger?
- Are metadata and sources readable without pulling focus?
- Are weights, line heights, and insets consistent across peers?

Record the final font size, line height, weight, width, and measured height for every text role in the build audit. A clean screenshot is necessary, but the recorded system reveals accidental one-off values that visual review can miss.

Use identical screenshot dimensions when comparing alternatives. A 1000px overview makes every role appear 20% smaller than a 1200px overview and can reverse a legibility judgment.

## Common failures

- A giant heading paired with a subtitle that looks like footnote text.
- A subtitle nearly as large as its heading, creating two competing headlines.
- Equal gaps between every text block, which erases grouping.
- Large type used to occupy a large canvas rather than to express importance.
- Broad cards with one short line and empty interiors.
- Narrow measures that turn simple sentences into tall text columns.
- Metadata made faint, tiny, or both.
- Peer cards with slightly different title sizes or insets.
- Centered paragraphs that become difficult to scan.
- Five text roles where three would communicate the same hierarchy.
