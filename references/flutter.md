# Flutter defaults

Use these only where the profile is silent. When the profile names a different stack,
follow it.

## Stack (for a new app, or gaps)

- **Flutter stable**, pinned with fvm (`.fvmrc`).
- **State:** flutter_riverpod (3.x):
  - `FutureProvider` (`.family`, `autoDispose`) for reads;
  - `NotifierProvider` for screen state;
  - an override point for anything tests must replace (HTTP adapter, storage, images).
- **Routing:** go_router:
  - `StatefulShellRoute.indexedStack` for tabs, so each keeps its stack;
  - **always build pages explicitly** (`pageBuilder` returning a `MaterialPage` /
    `CupertinoPage`). Left to infer the app type, go_router can fall back to pages with
    no transition, which flash and can't be swiped back.
- **HTTP:** dio. Generate the API client from the backend's OpenAPI with a codegen tool
  (e.g. swagger_parser + retrofit + json_serializable). Snapshot only the paths the app
  uses, and add a backend test that fails when the snapshot goes stale.
- **Images:** cached_network_image behind a small `Artwork(url, size)`-style widget
  that tests can override.
- **Tokens:** generate Dart tokens from the web's design source (CSS variables or a
  design doc) with a small script, so the two can't drift. Never hand-edit generated
  files.

## Theme recipe (the web's look, not Material's)

- `ThemeData(useMaterial3: true)` with the colour scheme from the tokens, and the
  scaffold transparent over a page background that matches the site.
- **Kill the Material tells:**
  - `splashFactory: NoSplash.splashFactory`;
  - surface tints off;
  - a themed `bottomSheetTheme`, `dialogTheme` and `snackBarTheme` (floating);
  - `sliderTheme` with a thin track and a round knob;
  - `inputDecorationTheme` with the site's border and a subtle focus;
  - every button theme on the control corner.
- **Page transitions:** Cupertino on every platform, with each page painting its own
  background inside the transition (wrap the child), so the outgoing page never shows
  through.
- **A phone type scale:** web scales usually read about one step too big on a phone
  (body 14–15, small 12.5–13, label 12). Define the text fields' `bodyLarge` too, or
  inputs fall back to Material's 16.
- **One corner scale:** chip, inset, control and panel radii from the tokens. No
  stadiums or circles on controls unless the site uses them.

## Shared widgets an app needs (build once, reuse)

| Widget | What it does |
|---|---|
| `Pressable` | Tap feedback by dimming and a slight scale (60ms down, 180ms up); the whole box is the hit target |
| `Spinner` | `CupertinoActivityIndicator` |
| `pageBar(title, actions)` | The app bar: a left-aligned title on the page background |
| Bottom tab bar | Equal tabs in one container; a soft highlight slides to the current one; haptic on change |
| `Segmented<T>` | Segmented control with a sliding fill |
| `showChoices<T>` + `ChoiceRow` | A sheet of check rows, single or multi; search past ~12 options; keys per row |
| `PickerRow<T>` / `PickerButton<T>` | The dropdown replacement (in a panel / standalone) |
| `FilterPill` | A filter that opens its sheet; fills when set |
| `showPanelSheet` | A titled sheet with free content |
| `FoldPanel` / `FoldRow` | Folding panel / row with a summary |
| `SwitchRow` | Label and `CupertinoSwitch` |
| `LabeledSlider` | Label/value line over a full-width slider |
| `SquareButton` | An icon button |
| `Section` | A titled panel |
| `SuggestionRows` | Suggested questions as rows |
| `AnswerText` | Markdown-lite answers (lead sentence, rows, citation chips) |
| `CantReach` / error and empty states | Every async screen has loading, empty, error and offline states |

## Tests

- A `pumpApp(tester, backend, {location, size})` harness that builds the **real app
  widget** (theme included), overrides the HTTP adapter with a fake backend, and
  overrides images.
- **Fake backend:** routes on path and query to recorded JSON fixtures; records the
  requests so tests can assert what was asked.
- **Gotchas:**
  - dio's default transformer decodes JSON over 50 KB in an isolate, which never
    completes under fake async. Use a synchronous transformer in tests via a provider
    override;
  - a request started on the last settled frame can leave a pending timer: pump once
    more before the test ends;
  - set a phone-sized view (e.g. 390×844 @3x) and reset it in tearDown.
- Drive the UI by `Key`s; assert on requests and visible text, not widget internals.
- Regression tests for layout bugs measure the thing that broke (e.g. a chip's width).

## Accessibility and motion

- AA contrast for text, including muted text and placeholders.
- `Semantics(button:, selected:, label:)` on custom tappables.
- Every animation honours `MediaQuery.disableAnimations`; ease out with
  `Cubic(0.16, 1, 0.3, 1)`; no bounce.

## Simulator loop (iOS)

```bash
xcrun simctl list devices booted
fvm flutter build ios --simulator --debug
xcrun simctl install <udid> build/ios/iphonesimulator/Runner.app
xcrun simctl launch <udid> <bundle-id>
```

Then screenshot with the simulator tool. A localhost server is reachable from the iOS
Simulator; on the Android emulator, use `10.0.2.2`.
