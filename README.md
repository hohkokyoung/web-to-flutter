# web-to-flutter

A Claude Code skill for turning a website into a Flutter app that keeps the website's
design language with phone-first UX. It also audits existing app screens against their
web pages.

- `SKILL.md` is the workflow:
  - set up the project's profile;
  - check the current docs;
  - read the web side;
  - map it and show the map;
  - build;
  - test;
  - check on a simulator;
  - record the work.
- `references/`:
  - `profile.md`: how the per-repo profile is created and kept current, with the
    template;
  - `translation.md`: web patterns → phone patterns;
  - `flutter.md`: the default stack, theme recipe, shared widgets, test gotchas and
    the simulator loop;
  - `audit.md`: the screen-by-screen checklist.

Each project gets its own profile at `.claude/web-to-flutter.md`, which the skill
creates on first use. For an example, see `pokedex-rag/.claude/web-to-flutter.md`.

## Install

Claude Code loads skills from `~/.claude/skills/`. Link this folder there:

```bash
ln -s ~/Projects/web-to-flutter ~/.claude/skills/web-to-flutter
```

A new session picks it up. Edits here apply directly, since the link points at this
folder.
