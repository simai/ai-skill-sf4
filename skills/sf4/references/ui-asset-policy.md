# SF4 UI Asset Policy: Demo vs Production

> Источник: https://sf4.simai.pro/ru/ui/, repo://bx-simai.framework/local/modules/simai.framework/install/simai/asset · Проверено: 2026-10-23 · Версия: simai.framework 5.0.133 (bx-simai.framework@7c34b9c)

## Goal

Prevent accidental transfer of documentation-only assets into production templates while keeping required interactive dependencies connected.

## Demo-Oriented Assets Seen In `/ru/ui`

Typical examples:

- `highlight`
- `bootstrap-docs`
- `clipboard`

Use these only for docs/demo pages, code snippets, or style guide mirrors.

## Production-Relevant Asset Signals

Examples from catalog pages:

- `fancybox`
- `animate`
- `sf-icon`
- `forecast-icon`
- Inputmask bundles via `addJs(...)`

These may be valid in production when the corresponding UI behavior is used.

## Policy Rules

1. For project/frontend delivery, include only assets required by real page behavior.
2. Do not copy docs helper assets into runtime blocks by default.
3. Keep asset connection in project layer and follow project conventions.
4. When adding an asset, record why it is needed and what breaks without it.
5. On refactor, remove obsolete assets if interactive behavior was removed.

## Pre-Release Verification

- Asset list was reviewed for docs-only entries.
- Interactive dependencies are connected and initialized.
- No unnecessary vendor payload was introduced.
- Critical pages pass smoke checks after cache clear.
