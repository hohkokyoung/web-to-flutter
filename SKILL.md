---
name: web-to-flutter
description: Convert a web app's pages or components (React/Next, Vue, Svelte, plain HTML/CSS) into a Flutter app that keeps the website's design language with phone-first UX; or audit an existing Flutter screen against its web page and fix the drift. Works in any repo through a per-project profile it creates on first use. Use when the user says "bring X to the app", "port this page to mobile/Flutter", "make the app match the website", "audit the app screen", or names a web component to convert.
---

# Web → Flutter

Two modes. Pick from the request; ask if it's unclear.

- **Convert:** a web page or component becomes a Flutter screen, or part of one.
- **Audit:** compare an existing app screen with its web page, list the differences,
  then fix the ones the user picks.

The goal in both: **the website's design language with phone UX.** That means neither
a copy of the web layout nor stock Material.

## Files

| File | Read when |
|---|---|
| The project profile, `.claude/web-to-flutter.md` in the repo | **Always first.** Paths, commands, shared widgets, house rules |
| [references/profile.md](references/profile.md) | No profile yet: how to create one; also how to keep it current |
| [references/translation.md](references/translation.md) | Mapping web patterns to phone patterns |
| [references/flutter.md](references/flutter.md) | The default stack and practices, and recipes for the shared widgets an app needs |
| [references/audit.md](references/audit.md) | Audit mode, and the final check in Convert mode |

**The profile wins over this skill's defaults.** When they disagree, follow the
profile and mention the difference once.

## 0. Profile

1. Look for `.claude/web-to-flutter.md`.
2. If it's missing, create it by following `references/profile.md`:
   - scan the repo;
   - draft the profile, marking guesses;
   - ask only what you couldn't find;
   - show the draft and save it once the user confirms.
3. Then continue the original task.

## 1. Current docs

Ask once per session: *"Check the current Flutter / state / router docs before
building? (recommended)"* Default to yes.

- If yes, look up the APIs you are about to use with the context7 tools
  (`resolve-library-id`, then `query-docs`).
- Pin to the versions in the app's `pubspec.lock` (and `.fvmrc` if present).
- If the docs show a better pattern or a newer version than the repo uses, **flag it;
  don't change it** as part of the conversion.

## 2. Read the web side

1. Read the component, its styles and the data it loads (the profile says where).
2. Look at it running at phone width. Use the browser at the profile's web URL, at the
   mobile size, and screenshot. If the web app isn't running, say so and work from the
   code.
3. Write down briefly:
   - the jobs the user does there;
   - the data each part needs;
   - **rules that live only in the web client.** Don't port those to Dart if the repo
     has a backend. Propose moving them server-side so both clients read one source.
     The profile says whether that's the house rule.

## 3. Map it (show before building)

For each web part, pick its phone form from `references/translation.md` and its widget
from the profile's shared-widget list (or `references/flutter.md` when the app lacks
one). Show a short map:

| Web part | App part | Why (only when the UX changes) |
|---|---|---|

Ask about anything that changes behaviour, not just look. Examples: dropping a column,
moving a control into a sheet, hiding rows behind "Show all".

## 4. Build

- **Follow the profile's stack;** when it doesn't say, follow `references/flutter.md`.
- **Reuse the shared widgets.** When a pattern repeats a second time, extract it into
  the shared widgets file and add it to the profile.
- **Tokens only:** colours, radii, spacing and type come from the app's theme or tokens.
  No literals for things the design system names.
- **Keys:** give interactive widgets stable keys so tests can drive them.

## 5. Test

- Write widget tests through the profile's test harness, against a fake backend with
  recorded fixtures (recorded from the running backend, not typed by hand).
- **Bug fixes:** a failing test first.
- **Features:** a test per behaviour.
- **Run** the profile's test command, unless the user said to skip it. Then say you
  skipped it.

## 6. Check on a simulator (always)

1. Build and run on the iOS Simulator (or the Android emulator, if that's the
   profile's target), using the profile's commands.
2. Screenshot with the simulator tool, beside the web screenshot from step 2.
3. Check:
   - the default state;
   - scrolled;
   - empty;
   - a sheet open;
   - the narrowest width (375pt), with long names.
4. Show the user the screenshots before calling it done.

## 7. Record it

- **Docs:** update the docs the profile names, in the same commit.
- **Spec process:** if the repo has one (the profile says; e.g. OpenSpec when
  `openspec list --json` returns a root), follow it: one change per shippable piece.
  Otherwise skip this quietly.
- **Commit** on the current branch, with a message saying what the user sees. Push only
  when asked.
- **Update the profile** with anything settled this time: a new shared widget, or a
  decision the user made ("no fade under the header"). See `references/profile.md`.

## Audit mode

1. Screenshot the web page (phone width) and the app screen (simulator).
2. Walk `references/audit.md` and the profile's house rules. List each difference,
   which side is right, and the fix.
3. Show the list and let the user pick, unless they said "fix everything". Then follow
   steps 4–7.

## How to work with the user

- They review visually and fast. One well-built screen they can see beats options to
  choose from.
- When they say something looks off, change it; don't defend it. Record the decision
  in the profile so it isn't repeated.
- Don't re-ask decisions already in the profile.
