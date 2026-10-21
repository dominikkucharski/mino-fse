# Agent instructions

This file is the single source of instructions for AI coding agents working in this repository. `CLAUDE.md` only imports it. People are welcome to read it too.

## What this project is

`mino-fse` rebuilds [mino.pl](https://mino.pl), the Polish portfolio and blog of Dominik Kucharski, as a WordPress block theme (Full Site Editing). The previous version is a classic theme built with ACF Blocks and Tailwind CSS in a private repository; it is the visual reference for this one.

It is also an evaluation: the same site, rebuilt without ACF, to compare block themes with an ACF Blocks workflow on real content. Significant decisions are recorded as ADRs, and a retrospective will summarise the findings at the end of the project.

**Status: Phase 1, project skeleton.** The architecture below is decided but mostly not in the repository yet. Commands and tooling notes are added here as each tool lands.

## Architecture (planned)

- `wordpress/`: WordPress core, installed by Composer. Never edit it.
- `content/`: the `WP_CONTENT_DIR`.
  - `mu-plugins/mino-core/`: data and behaviour, such as custom post types, post meta, block bindings sources, custom blocks and icons. Anything stored in post content must survive a theme switch, so it belongs here.
  - `themes/mino/`: presentation only, such as `theme.json`, templates, template parts, patterns, section styles and styles for core and custom blocks.
- `tokens/`: design tokens in the DTCG format, the single source of truth for `theme.json` presets.
- `scripts/`: build-time scripts, such as the `theme.json` generator.
- `docs/adr/`: architecture decision records. Read [the index](docs/adr/README.md) before changing anything architectural.

## Conventions

### General

- Everything committed is in **English**: code, comments, commit messages, documentation and ADRs. The site content is Polish; user-facing strings are written in English in code and translated to Polish (`pl_PL`) through translation files kept in the repository.
- One task means one branch and one pull request. Fill in the pull request template.
- Commits follow [Conventional Commits](https://www.conventionalcommits.org/) and stay small and focused, for example `feat(theme): …`, `fix(core): …`, `docs(adr): …` or `chore: …`.
- Every architecturally significant decision gets an ADR in `docs/adr/` in the same pull request (format in [ADR-0001](docs/adr/0001-record-architecture-decisions.md)).

### PHP

- Follow the [Human Made coding standards](https://github.com/humanmade/coding-standards).
- Prefer namespaced functions; use classes only for state or interfaces.
- `plugin.php` and `functions.php` are the only files with side effects: they `require` code and call `Namespace\bootstrap()`, which registers the hooks. Everything else is a function in `inc/namespace.php` or `inc/{area}/namespace.php`. See [Human Made: file structure](https://engineering.hmn.md/standards/structure/).
- Sanitise input, escape output late, and use type declarations.
- Text domains: `mino` for the theme, `mino-core` for the plugin.

### Blocks and the editor

- Target WordPress **7.1**: `theme.json` version 3 with the 7.1 schema.
- Custom blocks use `apiVersion: 3`, are **dynamic by default** (`render.php`) and are registered from the blocks manifest.
- Never use `__experimental*` or `__unstable*` APIs, `wp.editor` (use `wp.blockEditor`) or `apiVersion: 2`.
- The editor canvas is an iframe. Editor code must not use the global `window` or `document`; use the element's `ownerDocument` instead.
- Prefer core blocks, patterns and section styles. Write a custom block only when there is logic or data behind it.
- Never set responsive (`@mobile` or `@tablet`) styles on an individual block: WordPress saves them into post content and outputs them with `!important`. Structural exceptions go into `theme.json` or section styles.

### CSS

- SCSS with BEM class names.
- Values come only from `var(--wp--preset--*)` and `var(--wp--custom--*)`. No hard-coded values outside the tokens, no `!important` and no utility classes.
- Core block overrides load only when the block is on the page (`wp_enqueue_block_style()`).

## Verify, don't guess

WordPress and Gutenberg change quickly. Before relying on an API, confirm it in the WordPress 7.1 source code or the official handbooks on developer.wordpress.org. If you are not sure, say so. AI skills and older tutorials may describe outdated behaviour.

## Working with agents

AI agents assist with this project; the maintainer owns every decision and every merged line.

- Before changing code, explain what will change and why, which convention it follows (with a link) and which alternatives were rejected. Keep changes small and reviewable: one concept at a time.
- After a change, walk through the diff.
- The maintainer hand-writes the first implementation of each new core pattern; agents review it and may build on it afterwards:
  - the first namespaced `bootstrap()`,
  - the first custom block (`block.json`, `edit.js`, `render.php`),
  - the first section style,
  - the first block bindings source,
  - the first Interactivity API block.
- Stop and ask before destructive or hard-to-reverse actions, such as deleting data, force-pushing or publishing.

## Security, licences and content

- No secrets in the repository: no `.env` files, salts, keys, tokens or webhooks.
- No commercial plugins or assets in the repository, for example ACF PRO, WS Form PRO, WP Rocket or Lordicon PRO.
- Fonts must be OFL-licensed. Images must be original or CC0.
- No real client names, testimonials, photos or blog comments in seeds or fixtures. Use fictional content.
- `notes/` (the maintainer's private notes), `private/` (content exports) and `.kandev/` (task tool attachments) are git-ignored and must never be committed.
- Licence: GPL-2.0-or-later.
