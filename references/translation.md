# Web → phone translation

The default answer for each web pattern. The profile's house rules override these. If
a pattern isn't here, pick the closest row and tell the user what you chose.

## Layout

| On the web | On the phone |
|---|---|
| Multi-column page | One column. Long pages split into a segmented control (3–4 parts), each keeping its scroll and state |
| Card / panel | A white or surface panel with the site's corner and border. Never a panel inside a panel |
| Card grid | Two columns of compact cards; paging triggered from the last items, not a footer |
| Sticky header / filter bar | A fixed header above an expanded list. The list cuts off cleanly under it |
| Sidebar navigation | The bottom tab bar (3–5 destinations); the rest are pages pushed from them |
| Collapsible / "advanced" section | A folding panel with the title and a muted one-line summary of its state |
| Long list (20+ similar rows) | The top few that matter, then "Show all N" |
| Data table | One row per item: the name, a line of the two or three key values, the rest in a detail sheet |
| Modal / popover / drawer | A bottom sheet over the tab bar (root navigator), with a drag handle |
| Hover tooltip / `title` | A visible note line, or a tap that opens a sheet. Phones don't hover |
| Breadcrumbs | The back button and the page title |

## Controls

| On the web | On the phone |
|---|---|
| `<select>` / dropdown | A settings row (label left, value right, chevron) that opens a sheet of check rows; a search box when there are more than ~12 options |
| A row of filter chips/buttons | Pills that each open their own sheet; a set pill fills and names its choice ("Fire", "3 types") |
| A horizontal strip of toggles | A sheet or a wrapping grid, never a strip that hides options off the edge |
| Tabs / segmented buttons | A segmented control |
| Checkbox / toggle | A switch row |
| Range input | Label and value on one line, a full-width slider below |
| Radio group | A segmented control for ≤ 5 short options; otherwise a check sheet |
| Icon button | A square icon button with the control corner |
| Primary / secondary button | Filled (brand action colour) / outlined, both with the control corner |
| Text input + submit button | A field with the send action inside it (a small square or rounded icon button) |
| Suggested queries as chips | Rows in one panel, each with an arrow |
| `confirm()` / dialog | A native-feeling alert; destructive actions marked |
| Toast | A floating snack bar |
| Skeleton / spinner | A platform activity indicator; a thin progress bar for a section loading in place |

## Content

| On the web | On the phone |
|---|---|
| Rendered markdown answer | First sentence as the lead (larger, the display face); list items as rows (with an image when one applies); citations as small tappable chips; notices as a tinted box |
| Badges / tags | Chips with real padding (8–10pt sides); counts inside the chip |
| Key/value facts | Rows with a fixed label column |
| Numbers / stats | The monospace or tabular-figure style |
| Charts | The same chart simplified to the phone width, or the key numbers as rows if the chart can't read at 375pt |

## Always right on a phone

- Hit targets ≥ 44pt; the whole row is the target.
- Show less by default; fold or cap the rest.
- Nothing runs off the screen at 375pt; long text ellipsizes or wraps.
- A wall of options (variants, forms, a long list of moves) goes in a searchable sheet.
- Reduced motion turns animation off.
