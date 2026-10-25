# SF4 `sf.grid` Editor Features (Backend Notes)

> Источник: https://sf4.simai.pro/ru/bx/components/sfgrid.php, repo://bx-simai.framework/local/modules/simai.framework/install/bitrix/components/simai/sf.grid · Проверено: 2026-10-23 · Версия: simai.framework 5.0.133 (bx-simai.framework@7c34b9c)

## Goal

Use modern `sf.grid` capabilities safely in production projects, including row order editing, block presets, and modifier tooling.

Source page:

- `https://sf4.simai.pro/ru/bx/components/sfgrid.php`

Version note: the row-wrap parameters (`USE_SINGLE_ROW_WRAP_MODIFIER`, `EVEN_/ODD_ROW_WRAP_MODIFIER`), drag-and-drop order and presets are described on the documentation site but are absent from the `simai.framework` copies in `bx-simai.framework` (`local/modules` 5.0.115 and `source/simai.framework/module` 5.0.133: `sf.grid/.parameters.php` has only `ROW_WRAP_MODIFIER`, no `sf.grid/ajax.php`). Check the `sf.grid` version on the target site before relying on them.

## Key Features

### 1) Single Even/Odd Row Modifiers

- component params:
  - `USE_SINGLE_ROW_WRAP_MODIFIER`
  - `EVEN_ROW_WRAP_MODIFIER`
  - `ODD_ROW_WRAP_MODIFIER`
- render logic should apply modifiers by visible row index.

### 2) Public Drag-and-Drop Row Order

- requires:
  - `grid_edit_mode = Y`
  - non-empty `COMPONENT_ID`
- client sends row order to `sf.grid/ajax.php`.

Security note:

- if edit mode is exposed too broadly, page order can be changed by unintended users.

### 3) Block Categories

- block metadata can include `CATEGORY` in `.description.php`.
- if missing, fallback may use folder/section convention.
- custom project blocks must make `.description.php` return an array. The
  public component editor builds the block selector through
  `SIMAI\Main\Block\Section::getNameList()`, so files that only assign
  `$arTemplateDescription` and return `1` can break `component_props.php` with
  `Cannot use a scalar value as an array`.

### 4) Custom Block Selector + Preview

- selector stores code value and can show preview.
- should preserve already-selected blocks even when section source changes.

### 5) Block Presets

- expected actions:
  - create/update/delete preset
  - rename preset
  - apply preset values to current block params
- keep preset naming and ownership conventions clear.

### 6) Visual Modifier Editor

- replaces manual class-string editing with controlled UI.
- unknown modifiers should be visible as custom values, not silently dropped.

## Implementation Checklist

1. `COMPONENT_ID` is stable and unique for editable grid instances.
2. Row order persistence is verified after cache clear.
3. Preset actions are restricted to allowed editor roles.
4. Modifier editor does not remove required project-specific classes.
5. Regression test includes reorder + preset apply + revert flow.
