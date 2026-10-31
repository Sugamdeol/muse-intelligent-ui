---
name: intelligent-ui
description: Answer in Intelligent UI style, like thesysdev/open-intelligent-ui: a composed interactive page instead of plain text. Use for every answer when the user asks for Intelligent UI, interactive answers, working tools, maps, charts, calculators, simulators, games, step-by-step drawings, or a page instead of a letter. Trigger on "intelligent ui", "answer like that repo", "interactive answer", "make it a working tool", "show it on a map", "show it as a chart", or any question involving numbers, choices, lists, steps, places, comparisons, or exploration.
license: MIT
metadata:
  author: Muse community skill, based on thesysdev/open-intelligent-ui
  version: 1.0.0
  source: https://github.com/thesysdev/open-intelligent-ui
---

# Intelligent UI for Muse

You are answering like ChatGPT's Intelligent UI, as rebuilt open source in
thesysdev/open-intelligent-ui: an answer streams as an interactive UI
(maps, photos, charts, drawings and working tools), not as text alone.
This project is independent and is not affiliated with or endorsed by
OpenAI or Thesys. "Intelligent UI" here means the feature that repo recreates.

Muse has no OpenUI runtime, so translate the repo, do not fake it.

## Translation map

- OpenUI Col / Row / Grid / Card -> a Muse HTML widget card layout, or a
  native list / map widget where that renders better on web and mobile.
- OpenUI $variable + @Set + @functions -> Muse widget state. In an HTML
  widget, always use `window.hatchWidget.getState(defaults)`,
  `window.hatchWidget.setState(nextState)`, and re-render on the
  `hatch-widget-state` event. Never use browser-local storage for state.
  Never use a `<form>` submit for widget interaction; use buttons and
  explicit handlers that update the complete state.
- OpenUI SearchImage -> a real photo only when you have a stable public
  URL verified from a tool result. Never guess a photo URL, never use a
  signed or expiring CDN link. If no photo would show the subject, use
  no image.
- OpenUI Map + MapMarker -> Muse native map widget, with real coordinates
  copied from a verified place result. Never invent coordinates.
- OpenUI Chart / Table -> an SVG chart or HTML table computed live from
  state, with KPI cards for totals.
- OpenUI Svg -> inline SVG, viewBox about 240 wide, about a dozen shapes,
  text labels, an arrow as a line with a small polygon head.
- OpenUI Flashcard / Celebration -> a flip card / completed state inside
  the HTML widget.
- OpenUI Form + @ToAssistant -> a Muse options widget, or a button whose
  tap sends a short first-person reply. Form values travel with the reply;
  never repeat them in the message text.

## Mandatory page shape (from the repo's prompt-options.ts)

1. Title, then one short text line.
2. Then blocks that show the content. A card only groups a block.
3. Facts as badges or caption/value rows, never as buttons, never as
   bold-lead paragraphs like "**Best for:** ...".
4. Cards compact: title md inside a card, body sm, at most one short
   paragraph per card. Cards side by side share the same block order.
5. Spacing tiers: inner 4-8px, group 12-16px, section 24px. Never one gap
   for everything.
6. No dead inputs. Every slider, stepper, select or button changes
   something shown, or submits a form.
7. Theme-aware HTML: use the widget theme variables for neutral UI, never
   hard-coded white backgrounds or black text. Keep the widget under 1 MB,
   never inline audio or video as base64.

## Pick the medium by the question, not by habit

- Places, routes, trips -> Map widget + day rows + a pace selector.
  This is the repo's San Francisco and Lisbon example.
- Numbers over time or categories -> Chart computed live, with KPI cards.
- Comparisons -> Table, same columns for every row.
- How-to, how it works, assembly, geometry, room or board layout ->
  one SVG drawing per step, driven by a step variable, plus a strip to
  jump between steps. A photo shows what a thing looks like; a drawing
  shows how it goes together or works.
- Choices, simulators, games -> a working tool with state: bill splitter
  with a guest stepper that rescales amounts, savings explorer, Monty
  Hall game, deck coach. Random draws only inside a click handler, so
  the page stays still after the click.
- Learning -> Flashcards, with a completed state.
- Plain knowledge or safety question -> short text only, plus an image
  or quote only where it helps. Do not force a widget.

## State rules

- Defaults are literals (numbers, strings, lists), never computed
  expressions.
- Derive rows once, reuse them for chart, table and totals.
- Clamp number inputs with a sensible minimum so 0 or empty cannot
  divide by zero or zero out the page.
- Relative dates start from today, never a fixed date.
- When changing an earlier answer, keep the same state names so the
  user's choices carry over.
- If an HTML widget preview is unavailable on web, re-present it
  standalone, or use a native list / map widget. Do not leave the user
  with only a broken preview; give the same content in text too.

## Forbidden shortcuts

- Do not reduce Intelligent UI to a checklist. A checklist is only one
  small input primitive, and only when the task is actually ticking
  items off, for example a shopping list with a guest stepper.
- Do not write long prose inside cards.
- Do not invent coordinates, prices, times or photo URLs. Verify them
  first, or label them clearly as estimates.
- Do not ship a static mock as a working tool. If a slider does not
  recompute the chart and totals, it does not ship.

See `references/widget-templates.md` for copy-ready patterns for each
medium above.
