# Widget templates for Intelligent UI in Muse

These are patterns, not finished answers. Adapt the data, keep the state
pattern exactly: getState with literal defaults, setState on every change,
re-render on hatch-widget-state. All HTML fragments must be theme-aware
and under 1 MB.

## 1. Working calculator with chart (savings, bill split, loan)

State: `{monthly: 5000, years: 20, rate: 10}`
Controls: three range sliders.
Output: three KPI cards (final value, amount put in, growth) and an SVG
polyline chart. Recompute the whole series on every input change:
monthly compounding, yearly points, x from year 0 to years, y scaled by
the maximum point. Label the chart as an illustration, never as a return
promise. Same pattern works for a bill splitter: a guest stepper
(+ / - buttons, clamped 1 to 20) rescales every line amount, totals
recompute, and a copy action joins the lines.

## 2. Route / trip page (places)

Step 1: native map widget with real markers, one per stop, in route order.
Step 2: HTML widget with a pace selector (Relaxed / Packed buttons).
Relaxed shows the first 3 stops, Packed shows all. Stops, total hours
and total cost are all computed from the selected list, never hard-coded
per pace. Each stop row: number + title, one badge (View / Food / Ferry),
one short sentence, hours and cost. No checklist unless the user asked
to tick stops off.

## 3. Step-by-step drawing (how-to)

State: `{step: 0}`
Content: an array of steps, each with a title, one short text, and one
inline SVG (viewBox about 240x160, about a dozen shapes, labels).
Viewer: badge "Step X of N", percent done, the current drawing, Back /
Next buttons clamped to the range, and a thumbnail strip where tapping a
step sets the step. This mirrors the repo's bookcase example.

## 4. Comparison table

One HTML table, same columns for every row, no prose in cells. Above it,
a segmented control that filters the rows; the table, the count and any
"best" badge recompute from the filter. Example columns: Name, Price,
Best for, Trade-off.

## 5. Flashcards for learning

State: `{index: 0, flipped: false, done: false}`
Card shows the question, tap to flip to the answer, Next moves on.
When the last card is passed, show a completed state. Never shuffle on
render; shuffle only inside a button handler, so the order stays still.

## 6. Dashboard

KPI cards on top (count, sum, average, all computed from the rows),
then a chart, then a top-N table. A filter control (region, channel,
category) applies to every block at once. This mirrors the repo's sales
and support dashboards.

## Fallback rule for every template

If the HTML widget cannot render on the user's client, present the same
answer as a native list / map widget, or as short text with the key
numbers. A broken preview with no fallback is a failed answer.
