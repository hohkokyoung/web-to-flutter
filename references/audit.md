# Audit checklist

Run it against simulator screenshots, with the web page at phone width beside them.
Add the profile's house rules as extra items. Each ✗ is a fix to propose.

## Same as the website?

- [ ] Every job the web page supports is possible in the app. Anything missing is
      listed, not silently dropped.
- [ ] The same data from the same source. No rule computed only in Dart that the web
      gets from the server, or that should live on the server.
- [ ] The same visual vocabulary for the same things: badges, pickers, result layouts.
- [ ] The same words: labels, empty states, error lines.

## Not stock Material (unless the profile chose Material)

- [ ] No ink ripples, Material spinners, dropdown menus, checkbox lists, `AlertDialog`,
      `ExpansionTile`, or `ActionChip`/`ChoiceChip` used as buttons.
- [ ] Pages slide in and swipe back. No route without a transition.
- [ ] Sheets, dialogs and app bars without Material surface tint.

## Consistent

- [ ] Corners only from the token scale; no stadium or circle on a control unless the
      site does it.
- [ ] Text only from the app's type scale.
- [ ] The brand action colour only on actions and the current selection.
- [ ] The same element looks the same on every screen. Check the suggestion lists,
      send buttons, fact rows, chips and pickers across screens, not one screen alone.
- [ ] Spacing from the tokens: related items close, groups apart. No large gaps between
      list items; no chip touching its neighbouring text.

## Reads well on a phone

- [ ] Text isn't oversized; chips aren't cramped.
- [ ] Shows less by default: lists capped with "Show all", detail folded, redundant
      numbers dropped.
- [ ] Nothing runs off at 375pt; long names ellipsize; no option strips off the edge.
- [ ] Many options live in a searchable sheet, not a wall of chips.
- [ ] Hit targets ≥ 44pt, whole rows tappable.
- [ ] Loading, empty, error and offline states exist and look intentional.
- [ ] Reduced motion respected; AA contrast.

## Report

```
| # | Where | What differs | Fix |
|---|-------|--------------|-----|
```

Lead with what the user will notice most. Ask which to fix, unless they already said
"fix everything".
