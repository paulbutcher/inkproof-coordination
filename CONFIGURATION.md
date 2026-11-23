# Configuration audit

The public configuration surface of every `inkproof-` library, what a client can and cannot ask a chart to do, and where the surface is inconsistent. Read from each library's `README.md`, `API.lock` and source at `origin/main` on 2026-11-23; nothing was run. "Who sets it" is `markup` (an attribute or CSS custom property on the page), `Lean` (a value in code that builds the chart), or `neither` (fixed in source). Compared against uPlot's options and demos as the project already uses them (see `PRIOR-ART.md`), and against what ordinary charting libraries let a page choose; uPlot's own documentation was not re-read for this pass, so the "impossible" column is a floor.

The convention for combining options and where appearance lives is settled in [Convention](#convention).

## Options

One line per option. `Libraries with no configuration`: core, float, export (its entry points take a display list and nothing that changes the look beyond it), tools, consumer-metrics (a data policy, `GapPolicy`, not a chart option), gpu (its frames take sizes and a display list; the look is the display list's).

| Library | Option | Who sets it | Controls |
| --- | --- | --- | --- |
| web | `src` attribute | markup | What the loader fetches for the chart |
| web | `log-level` | markup | Console threshold, warn by default |
| web | `box-zoom` | markup | Axes a shift-drag box zooms: `x`, `y`, `xy`, `auto` |
| web | `box-min` | markup | Least box drag in pixels, read with `box-zoom` |
| web | `drag` | markup | Whether a plain drag pans (`pan`) or draws a box (`box`) |
| web | `group` | markup | Charts sharing an x range |
| web | `cursor` | markup | Cursor lines: `off`, `x`, `y`, `xy` |
| web | `cursor-dotted` | markup | Cursor lines dotted, a bare flag beside `cursor` |
| web | `tooltip` | markup | A list of words: metric (`x`, `xy`), `shared`, `no-x-value`, anchor (`point`, `pointer`), opening corner, offset number |
| web | `--inkproof-tooltip-*` (color, background, border-color, border-width, border-radius, font-family, font-size, padding, shadow), `::part(tooltip)` | markup | Tooltip look |
| web | `legend` | markup | A list of words: placement (seven), layout (`vertical`, `horizontal`), `static`, `x`, `y`, `no-x`, `no-y` |
| web | `select` | markup | Range selection over x: `off`, `x` |
| web | `show-table` | markup | Swap the canvas for the accessible data table |
| web | `data-reset`, `data-unlink`, `data-group`, `data-series` on buttons, with `register (buttons := true)` | markup, Lean | Buttons that reset, unlink or toggle a series |
| web | `Host.register`, `registerSized`, `registerPixels` | Lean | Tag, loader, sizing mode |
| web | `Host.setScene`, `setCursor`, `setSelection`, `apply` | Lean | Scene, cursor, selection and commands from the page's code |
| web | `NavConfig`, `BoxConfig`, `Legend.Style`, `Tooltip.Style`, `CursorMode`, `DragMode`, `SelectMode` | Lean (parsed from the attributes above) | Typed form of the attributes |
| web | Legend font, size, spacing, entry layout CSS | neither | Fixed in the element's stylesheet (12px sans-serif) |
| web | Wheel zoom, touch pan, drag pan on or off | neither | Wheel is claimed whenever the chart has a view; `touch-action:none` always ([web#113](http://localhost:3000/tentenths/inkproof-web/issues/113)) |
| web | Marker at the cursor on each series | neither | Does not exist ([web#115](http://localhost:3000/tentenths/inkproof-web/issues/115)) |
| web | Plot origin inset for axis labels | Lean | `Access.plotOrigin`, set by the page's code |
| scene | `AxisStyle.line`, `lineHalfWidth` | Lean | Axis line colour and width; cannot be omitted |
| scene | `AxisStyle.tick`, `tickHalfWidth`, `tickLength` | Lean | Tick marks; cannot be omitted |
| scene | `AxisStyle.grid` (`Option (Paint × Rat)`) | Lean | One gridline at each tick; none without a tick ([scene#74](http://localhost:3000/tentenths/inkproof-scene/issues/74)) |
| scene | `AxisStyle.labelGap` | Lean | Space between tick and label |
| scene | `TickSpec.locale`, `target`, `labelGap` | Lean | Number format, about how many ticks, minimum gap between labels |
| scene | `Side` | Lean | Which side of the plot an axis is on |
| scene | `compileLinearAxis`, `compileLogAxis`, `compileCategoricalAxis`, `compileTimeAxis`, `compilePixelAxis` | Lean | Scale kind of an axis |
| scene | `placeAxisTitle` | Lean | Title beside an axis |
| scene | `LineStyle.paint`, `halfWidth`, `reduce`, `dash`, `join` | Lean | Line colour, width, thinning, dash, straight, stepped or spline joins |
| scene | `BarStyle.paint`, `width` | Lean | Bar colour and width in projection units |
| scene | `ScatterStyle.paint`, `shape`, `radius` | Lean | Marker colour, shape and size |
| scene | `RingStyle.layer`, `outline`, `fill`, `radius`, `stroke` | Lean | Hollow marker |
| scene | `Paint`, `Color`, `Shading.gradient`, `Shading.steps` | Lean | Colour, gradients along x or y, up to five stops |
| scene | `SeriesStyle`, `Theme.series`, `Theme.seriesStyle` | Lean | Colour, marker and dash per series index, light or dark |
| axis | `Locale` (`decimal`, `group`, `groupSize`, `minus`) | Lean | Number format of tick labels |
| axis | `linearTicks` etc. `target` count | Lean | About how many ticks |
| axis | Tick positions of a log axis | neither | Powers of ten only ([axis#45](http://localhost:3000/tentenths/inkproof-axis/issues/45)) |
| axis | Which ticks are labelled | neither | Every tick that fits, chosen by `layoutLabels` |
| axis | `layoutLabels` gap, `Rotation` | Lean | Minimum label gap; rotation chosen by the layout, not by the client |
| axis | Time zone function, month names, `Locale` for dates | Lean | Time axis ticks and labels |
| scale | Scale kind (`Linear`, `Log`, `Asinh`, `Transformed`, `Categorical`, `Duration`) | Lean | Mapping of data to the axis |
| scale | `RangeLimits`, `autoRange`, `padExtent` | Lean | Padding, zero inclusion, soft and hard bounds of the shown range |
| scale | `Constrained` | Lean | Domain validity |
| viewport | `Interval.Config` (`length`, `minSpan`, `maxSpan`, `home`, `keyZoom`, `keyPan`) per axis, in `ViewConfig` | Lean | Zoom limits, home range, key steps |
| viewport | `Gesture.boxOver` axes and minimum distance, `Gesture.minBox` | Lean | Box zoom |
| viewport | `Selection.Config` (`domain`, `minWidth`, `tolerance`) | Lean | Range selection |
| viewport | `Linked`, `Linked.unlink`, `relink` | Lean | Shared axes |
| viewport | Wheel sensitivity, pinch behaviour | neither | Fixed in `Gesture` |
| demos | `Frame` (`left`, `right`, `top`, `bottom`, `xs`, `ys`) | Lean | Plot rectangle (margins) and data ranges; the margins are constants in each chart's `plot` |
| demos | `Lines.Chart`, `Layer`, `YAxis` (`suffix`, `grid`), `Band` | Lean | Series, a right axis, bands, per-axis gridlines on or off |
| demos | `Bars.Chart` (`stacked`, `horizontal`), `Pane` (`below`, `target`), `Timeline.Chart`, `Zoom.Chart` | Lean | Chart-kind options of the demos |
| demos | Label fonts: `12px sans-serif` for axes, `10px sans-serif` for bar labels | neither | Fixed in `Browser` ([demos#109](http://localhost:3000/tentenths/inkproof-demos/issues/109)) |

## What a client can configure, by kind of thing

"Where" is the side of the line a client uses today. "Impossible" lists reasonable configurations with no way to ask for them.

| Thing | Configurable | Where | Impossible today |
| --- | --- | --- | --- |
| Axes | Side, style, scale kind, ticks target, locale, title | Lean | Omitting the line or ticks (only transparent paint); any of it from markup; label font; showing an axis on a chosen subset of sides without a Lean call per side |
| Gridlines | Colour and width, on or off with the axis | Lean | Gridlines without ticks, minor and major gridlines, gridlines at chosen multiples ([scene#74](http://localhost:3000/tentenths/inkproof-scene/issues/74)) |
| Ticks | Target count, locale | Lean | A log axis at 1, 2, 5 ([axis#45](http://localhost:3000/tentenths/inkproof-axis/issues/45)); choosing which ticks carry labels; explicit tick values other than through `compilePixelAxis` |
| Labels | Locale, gap, title | Lean | Font, size and colour for axis labels; rotation chosen by the client; compact notation |
| Legend | Placement, layout, live or static, x and y readouts | markup (words) | Font, size, spacing, colours; number format and locale; entry order |
| Tooltip | Metric, shared, anchor, corner, offset, look | markup (words, CSS properties) | Content beyond the two built-in forms; number format and locale; shared settings with the legend |
| Cursor | Lines, dotted | markup (two attributes) | Line colour, width, dash style; markers at the cursor on each series ([web#115](http://localhost:3000/tentenths/inkproof-web/issues/115)); snapping |
| Series styling | Colour, width, dash, join, markers, fill, gradients, theme palette | Lean | From markup; per-series styling after the scene is built; bar value labels fitted to the bar |
| Plot margins | Plot rectangle | Lean (each demo hard-codes its margins) | A margin chosen by the page; margins sized to measured labels |
| Scales | Kind, range limits, padding | Lean | From markup; log base other than ten; a scale kind picked by an attribute |
| Number and time formatting | `Locale`, time zone, month names | Lean, per axis | One setting shared by axis labels, legend, tooltip and bar values; compact notation; precision |
| Theming | Light or dark series palette | Lean | Page colours, fonts and sizes for text drawn on the canvas; the legend; a dark page without a rebuild |
| Interaction | Zoom, pan, box zoom, drag mode, select, link group | markup (attributes) | Wheel, touch or drag turned off; unused input passed to the browser ([web#113](http://localhost:3000/tentenths/inkproof-web/issues/113)); zoom limits from markup |

## Inconsistencies

1. **Four ways to combine options.** One packed list of words (`legend`, `tooltip`); two attributes read together (`box-zoom` with `box-min`); a bare flag beside a valued attribute (`cursor` with `cursor-dotted`); a single enumerated value (`drag`, `select`, `cursor`). CSS custom properties carry the tooltip's look only.
2. **Appearance on different sides for similar things.** Tooltip look is markup; legend look is neither; cursor line style is a flag in markup; axis and series style are Lean only. Axis labels take their font from the host's measurement call, the legend and tooltip from CSS.
3. **Reachable from one side only.** Axis style, series style, locale, tick count and range limits cannot be reached from the page; the cursor, box-zoom and drag settings have a Lean type only through their parsers.
4. **Text style and number format are per use.** Axis labels, bar labels (10px in `Browser.barLabels`), tooltip and legend each fix or configure their own font; `TickSpec.locale` formats axis labels while the legend and tooltip use `showFloat` and bar values have no format.
5. **"None" has four spellings, and an axis cannot lose parts.** `cursor="off"`, `select="off"`, an absent `tooltip`, `no-x` in `legend`, an `Option` for a grid; `AxisStyle` has no `Option` for line or ticks, `grid` is tied to ticks, and defaults live in the structure for `BoxConfig` but in the parsers for `Legend.Style` and `Tooltip.Style`.

## Existing threads recorded here

- [web#110](http://localhost:3000/tentenths/inkproof-web/issues/110) legend placement and layout and [web#116](http://localhost:3000/tentenths/inkproof-web/issues/116) tooltip style, position and content: closed; both follow the list-of-words form.
- [web#113](http://localhost:3000/tentenths/inkproof-web/issues/113) wheel, touch and drag pass-through and [web#115](http://localhost:3000/tentenths/inkproof-web/issues/115) cursor markers: open; must follow the convention.
- [axis#45](http://localhost:3000/tentenths/inkproof-axis/issues/45) log ticks at chosen multiples: open.
- [scene#74](http://localhost:3000/tentenths/inkproof-scene/issues/74) minor gridlines: open.
- [demos#109](http://localhost:3000/tentenths/inkproof-demos/issues/109) match each page to its uPlot demo, including bar labels: open.

## Convention

Settled by the human's answer to [#228](http://localhost:3000/tentenths/inkproof-coordination/issues/228): both markup and Lean, Lean canonical. Scene content (axes, series, scales) stays Lean.

- A facet of a chart (legend, tooltip, cursor, interaction) has one Lean structure with a default for every field, and one attribute named for the facet whose value is a list of words, each word belonging to one choice; an unnamed choice takes the default, two words for one choice are an error, and a word outside the list is an error that is reported, never defaulted.
- Magnitudes are words too (`8px`), not further attributes. Appearance that is a CSS value (colour, font, spacing) is a custom property `--inkproof-<facet>-<name>`, with `::part(<facet>)` for the rest.
- "None" is the facet's attribute absent or the word `off`, and each element a facet draws can be turned off by its own `no-<element>` word.
- Text and number format are one shared option set, not one per facet.
- A new option states which convention it follows in its brief.
