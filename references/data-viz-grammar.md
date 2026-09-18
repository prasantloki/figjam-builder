# FigJam data-visualization grammar

Read this reference for any chart, dashboard, metric comparison, funnel, time series, distribution, or set of percentages. The chart's first job is to preserve the mathematical relationship and make the requested answer retrievable. Expression can strengthen that read, but cannot compensate for a worse analytical form.

## Begin with the reading task

Write the question the reader must answer before choosing a primitive. Distinguish among:

- Retrieve one value.
- Compare categories or rank them.
- Detect change, a crossing, or a stall over ordered observations.
- Compare stage populations.
- Diagnose transition loss.
- Read a bounded part of one whole.
- Compare several independent proportions.
- Understand a distribution or relationship.

The same values can need different charts when the question changes. A funnel silhouette can show stage populations; it is weak for comparing transition losses. A ring can show one completion value; four rings are weak for ranking independent percentages.

## Build in two layers

1. **Analytical substrate.** Choose the scale, position encoding, order, labels, units, baselines, targets, and derived values that make the answer true and retrievable. This layer must work without a headline, hero number, or decorative shape.
2. **Expressive layer.** Add claim hierarchy, semantic color, a focal annotation, or a content-specific shape only when it makes the same reading faster. Do not change chart grammar to make the board feel bolder.

Analytical clarity wins every tie. When the more expressive treatment makes exact comparison slower, keep the plainer chart.

## Match form to relationship

| Reading task | Preferred form | Guardrail |
|---|---|---|
| One raw value | Large numeral and concise label | Do not imply a scale or denominator that was not supplied |
| Ordered trend | Line chart with ordered positions | Use a sparkline only when exact values, crossings, and intervals are not the reading task |
| Target crossing or plateau | Line chart plus a distinct target and one interval annotation | Do not replace the ordered line with columns or a dramatic callout |
| Category comparison or ranking | Aligned bars or dots on one shared quantitative scale | Bar length starts at zero; disclose a nonzero dot-plot domain |
| One bounded proportion | Progress bar or ring | The denominator and 100% ceiling must be real |
| Mutually exclusive composition | Pie, donut, or 100%-stacked bar when part-to-whole is the question | Shares must refer to the same whole and should sum to it, allowing rounding |
| Independent or non-exclusive percentages | Aligned bars or dots on a shared 0–100% scale | Never use pie, donut, or 100%-stacked forms |
| Funnel stage populations | Ordered stage values; proportional widths only when population is the question | Width does not diagnose transition quality by itself |
| Funnel transition loss | Aligned loss bars by transition | Separate people lost from percentage lost because the units and denominators differ |
| Rating on a fixed scale | Dot, bar, or star row with the full scale disclosed | Do not use decorative stars when precise comparison matters |
| Distribution | Histogram, strip plot, or dot plot | Use only when bins or observations were supplied |

For a plotted series with three or more observations, prefer one `createVector` SVG path per series. Rotated `LINE` nodes are easy to invert, shift, or leave visually disconnected when repeated segment by segment; reserve them for axes, grid lines, targets, and other truly straight references. If separate line segments are unavoidable, verify every segment's slope and endpoint continuity in the rendered screenshot before treating the chart as complete.

## Scales, denominators, and derived values

- Put comparable quantities on the same scale. Repeated mini-charts with independent scales defeat comparison.
- Keep different units on different scales. Counts and percentages may sit side by side, but never share one length scale.
- Preserve the denominator that gives a percentage meaning. For transition loss, label the starting stage or state the denominator once.
- Use a zero baseline whenever bar length encodes magnitude. If position rather than length encodes a value and the domain is truncated, disclose the domain visibly.
- Distinguish a target, benchmark, or forecast from observed data through stroke or mark treatment and a direct label.
- Derive only what the supplied values support. Show the calculation or denominator when a derived answer could be interpreted more than one way.

## Give the answer one useful echo

The claim normally appears twice: once in the headline and once in the chart's visual relationship. Add one callout only when it answers a second question, exposes a derived value, or identifies an interval that is otherwise difficult to see.

Delete panels, badges, and footer readouts that repeat the same conclusion without reducing inference. A headline, hero number, side panel, chart label, and footer all saying “620 ms” create visual volume without more understanding.

Make this a countable final audit. List every place the primary answer appears. Keep the headline and the chart relationship. For every additional callout, side panel, footer, or hero number, write the distinct question it answers; delete it when that question duplicates another element. Reclaim the space for the chart or tighten the wrapper rather than leaving a mostly empty summary container. A low-content panel that uses more space than the evidence is a composition failure even when nothing overlaps.

When two answers are genuinely different, give each its own lane. For example, “largest absolute loss” and “largest proportional loss” deserve separate common-scale views and can share a concise headline.

## Use expression without weakening the chart

- Let the finding, not the question prompt, carry the largest type after the board title.
- Use semantic color for the supplied role: orange for worse or slower, green for healthy or faster, slate-blue for neutral comparison. Keep peers neutral when category colors have no meaning.
- Use a tinted interval, endpoint span, or one focal annotation when it directly marks the answer. Remove it when the chart already makes the same fact obvious.
- Keep axes and grid lines quiet but available. Styling them away is not sophistication when readers need the scale.
- Size the chart as primary evidence. A small chart under a large explanatory panel reverses the argument.
- Prefer integrated annotation over a detached answer card when both point to the same marks.

## Encode series accessibly

For multi-series charts, make series identity retrievable without hue. Use direct endpoint labels and at least one redundant mark channel such as line style or point shape. Keep those identities stable across every observation and distinguish adjacent or crossing series strongly enough to survive grayscale printing.

Color may reinforce the series system but cannot be its only key. Avoid a detached legend when direct labels fit; if a legend is necessary, show the same line and point samples used in the plot. When lines cross, label the first supplied observation where the order changes and do not claim an exact between-observation crossover unless the source provides it.

Treat label placement as part of the plot layout, especially when series are close. Reserve gutters outside the first and last plot positions so value labels do not sit on the y-axis or endpoint leaders. When adjacent values are closer than a label line height or two marker diameters, assign labels to separate lanes or use short non-crossing leaders. Keep every mark at its truthful quantitative position; move the label, not the data. Audit text against axes, grid lines, series paths, markers, and leaders in the rendered screenshot. A clean text-to-text bounding-box report does not override a visible text-to-path collision.

## Audit at two zoom levels

At overview zoom, verify:

- The requested answer is visible within two seconds.
- The chart form remains recognizable.
- Semantic emphasis identifies the finding rather than a decorative container.
- The headline, one focal annotation, and chart do not compete as three separate heroes.
- No mostly empty summary panel duplicates a conclusion already visible in the headline and marks.

At reading zoom, verify:

- Every supplied value, unit, order, denominator, target, and source is correct.
- Common scales align and bar baselines are honest.
- Labels do not collide with marks, axes, or one another.
- Derived values can be recomputed from what is shown.
- No duplicate legend, callout, or readout restates information already visible.
- Multi-series identity remains readable with hue removed: direct labels plus stable line style or point shape still distinguish every series.
- First/last labels clear the plot axes, close-valued labels occupy separate lanes, and no label or leader intersects another series path.

If the analytical and expressive audits disagree, fix the analytical read first. A visually striking wrong chart is still wrong; a precise chart can then be made authored through type, spacing, semantic color, and one well-chosen focal move.
