# Regression Checklist

> Источник: repo://bx-simai.framework/docs/update-policy.md · Проверено: 2026-10-23 · Версия: simai.framework 5.0.133 (bx-simai.framework@7c34b9c)

## Core

- [ ] Updated pages render without fatal/runtime errors.
- [ ] Active `grid_view_*` mapping resolves to expected view templates.
- [ ] Referenced blocks exist and render from expected layer.
- [ ] Changed settings follow expected precedence chain.

## Feature Scope

- [ ] Changed block/view behavior matches acceptance criteria.
- [ ] Editor params/labels for changed blocks are visible and correct.
- [ ] If touched: iblock/HL flows (list/view/edit) are intact.
- [ ] If touched: wizard/install/update pipeline remains reproducible.

## Platform Safety

- [ ] No unintended edits in system layer.
- [ ] Cache clear + re-check completed.
- [ ] Rollback steps are defined for risky changes.

## Result

- Status:
- Evidence links/notes:
- Remaining risks:
