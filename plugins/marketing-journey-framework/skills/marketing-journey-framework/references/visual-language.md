# Canonical Marketing Journey Map visual language

Read this reference whenever the Marketing Journey Framework produces an interactive possibility map or Customer Marketing Journey visualization.

## Required outcome

Use one consistent visual system across every company. The company name, research date, possibilities, sources, journey names, stories, and selected option IDs change. The composition, hierarchy, color sequence, controls, card treatment, option treatment, and interaction model stay fixed unless the user explicitly requests a redesign.

The canonical design is a quiet editorial strategy canvas: spacious, neutral, legible, and left-to-right. It should feel like a working strategic map rather than a dashboard, infographic, network diagram, or branded campaign page.

## Fixed page composition

Render these elements in this order:

1. Small uppercase kicker: `THE MARKETING JOURNEY FRAMEWORK · RESEARCH SNAPSHOT: [DATE]`.
2. Heading: `[Company] Marketing Journey Map`.
3. One muted orientation sentence stating the number of researched possibilities, the 18 Atomic Units, journey isolation, and hover/tap explanations.
4. One horizontal row of rounded controls: `All possibilities`, then three named journey controls.
5. A compact journey-story card that remains hidden in the initial All Possibilities state and appears below the controls when a journey is selected.
6. Three supergroup headings spanning two columns each: `Marketing Context`, `Marketing Activities`, and `Offer`.
7. Six left-to-right group columns in canonical order.
8. An optional collapsed research-basis and source section below the map.

Do not add a selected-item chip or permanent item-summary pill above the journeys.

## Grid and hierarchy

- Use a six-column CSS grid with `repeat(6, minmax(250px, 1fr))`, a `14px` gap, and a minimum map width of approximately `1570px`.
- Wrap the grid in a `width: 100%; overflow-x: auto` container. Narrow screens scroll horizontally; columns never collapse into a vertical sequence that destroys the conceptual flow.
- Supergroups use the same six-column grid. Marketing Context spans columns 1–2, Marketing Activities spans 3–4, and Offer spans 5–6.
- Each group begins with a slim four-pixel color bar, followed by the group heading.
- Each group contains exactly three atomic-unit cards in canonical order.
- Atomic-unit cards use a pale neutral background, subtle border, generous padding, and rounded corners. They should read as light containers around the unit as a whole, not as heavy dashboard panels.
- Possibilities are quiet rows inside the atomic-unit card. Do not put every possibility inside a separate pill or boxed tile.
- Begin each possibility with a short horizontal color rule aligned with the text. Long labels wrap naturally without overlapping adjacent columns.

Recommended sizing:

- Canvas background: white or the host surface background.
- Kicker: 12–13px, uppercase, muted.
- Main heading: 25–30px, medium weight.
- Orientation line: 15–18px, muted, maximum readable width.
- Supergroup and group headings: 20–24px, medium weight.
- Atomic-unit headings: 15–17px, semibold.
- Possibility labels: 15–18px, regular weight, comfortable line-height.
- Atomic-unit radius: 16–20px.
- Atomic-unit padding: 14–18px.
- Vertical space between atomic-unit cards: approximately 20–24px.

## Fixed six-color sequence

Use the host visualization series tokens when available so light and dark modes remain accessible:

1. Human Context: `var(--viz-series-1)` — blue.
2. Decision Environment: `var(--viz-series-2)` — yellow/gold.
3. Market Engagement: `var(--viz-series-3)` — pink.
4. Brand World: `var(--viz-series-4)` — teal.
5. Value Proposition: `var(--viz-series-5)` — violet.
6. Pricing and Proof: `var(--viz-series-6)` — the sixth host series color.

Apply each group color only to its slim heading bar, possibility rules, and restrained selected accents. Keep the rest of the canvas neutral. Do not recolor the map to match each company’s brand.

## Controls and journey states

- Initial state: `All possibilities` is active and all possibilities have equal visual weight. No journey story is visible.
- Controls are rounded rectangles with restrained borders. The active control has a dark neutral fill with high-contrast text; inactive controls remain white or transparent.
- Label journey controls with both number and concise identity, such as `Journey 1 · Global treasurer`.
- Selected journey state: reveal the compact story card, softly emphasize possibilities named in the journey’s `touches` array, and reduce every other possibility to approximately `0.25` opacity.
- Keep secondary possibilities visible so the full strategic space remains legible.
- Do not draw relationship lines between selected possibilities.
- Returning to All Possibilities hides the story card and restores every option to full opacity.

## Hover, focus, and tap behavior

Attach concise explanations to:

- all three supergroup headings;
- all six group headings;
- all 18 atomic-unit headings; and
- every company-specific possibility.

Use the host tooltip mechanism when available, such as `data-tooltip`. Possibility tooltips should contain the strategic interpretation and, when relevant, an evidence label such as Observed, Inferred, or Hypothesis to test. Provide equivalent accessible labels for keyboard and assistive-technology users.

## Data contract and validation

Use this conceptual structure:

```js
const groups = [
  {
    title: "Human Context",
    atoms: [
      {
        title: "ICPs/Personas",
        options: [
          { id: "unique-id", label: "Visible possibility", status: "observed", summary: "Tooltip interpretation" }
        ]
      }
    ]
  }
];

const journeys = {
  journeyKey: {
    title: "Journey 1 · ...",
    story: "Compact customer journey story.",
    touches: ["existing-option-id", "another-relevant-option-id"]
  }
};
```

Before presenting the map, verify:

- there are exactly six groups and 18 atomic units in canonical order;
- every option ID is unique;
- every journey has `title`, `story`, and an array named `touches`;
- each journey's `touches` array includes only the possibilities genuinely involved in that customer path; it does not need to represent all 18 atomic units;
- every referenced option ID exists;
- the initial state is All Possibilities;
- selecting each journey shows its story and highlights the correct options;
- returning to All Possibilities clears all dimming and selection;
- no text, controls, cards, or columns overlap at wide or narrow viewport widths; and
- the visualization loads without console errors.

## Canonical implementation cues

Use scoped classes and host design tokens. This CSS skeleton captures the required geometry:

```css
.mj-map { width: 100%; overflow-x: auto; padding-bottom: 8px; }
.mj-superheaders,
.mj-columns {
  display: grid;
  grid-template-columns: repeat(6, minmax(250px, 1fr));
  gap: 14px;
  min-width: 1570px;
}
.mj-superheader-context { grid-column: 1 / 3; }
.mj-superheader-activities { grid-column: 3 / 5; }
.mj-superheader-offer { grid-column: 5 / 7; }
.mj-column-bar { width: 100%; height: 4px; margin-bottom: 8px; }
.mj-atom {
  margin-bottom: 24px;
  padding: 14px 16px;
  border: 1px solid var(--border);
  border-radius: 18px;
  background: color-mix(in srgb, var(--muted) 38%, transparent);
}
.mj-option-content {
  display: grid;
  grid-template-columns: 30px minmax(0, 1fr);
  gap: 8px;
  align-items: start;
}
.mj-option-line { height: 3px; margin-top: .62em; }
.mj-option-label { overflow-wrap: anywhere; }
.mj-option-wrap { transition: opacity 140ms ease; }
.mj-option-wrap.is-secondary { opacity: .25; }
@media (prefers-reduced-motion: reduce) {
  .mj-option-wrap { transition: none; }
}
```

The implementation may adapt prefixes and host utility classes, but the visible outcome and behavior must remain consistent with this contract.

## Prohibited default variations

Unless explicitly requested, do not:

- replace the six columns with cards arranged in rows;
- stack the groups vertically on mobile;
- use relationship lines, webs, or force-directed nodes;
- turn possibilities into dense pills or badges;
- use a dark, gradient, glassmorphic, highly branded, or decorative canvas;
- hide non-selected possibilities entirely;
- preselect a journey;
- move the journey story above the controls or into a persistent side panel; or
- simplify away the supergroup hierarchy.
