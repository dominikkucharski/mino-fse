# mino-fse

A rebuild of [mino.pl](https://mino.pl), the Polish portfolio and blog of Dominik Kucharski, as a WordPress block theme (Full Site Editing), following [Human Made](https://humanmade.com) engineering conventions.

> **Status:** work in progress. Phase 1: project skeleton.

## Why

I have built WordPress sites with ACF Blocks for years. This project rebuilds my own site as a block theme to evaluate Full Site Editing on real content: what gets simpler, what gets harder, and what it means for clients. Decisions are recorded as [architecture decision records](docs/adr/README.md), and a retrospective will close the project.

## What's inside (planned)

- `content/themes/mino`: the block theme. `theme.json` is generated from design tokens; templates, patterns and section styles are built from core blocks.
- `content/mu-plugins/mino-core`: data and behaviour, such as custom post types, post meta, block bindings and custom blocks.
- `docs/adr`: architecture decision records explaining each significant choice.

Setup instructions will be added together with the local environment.

## Licence

[GPL-2.0-or-later](LICENSE)
