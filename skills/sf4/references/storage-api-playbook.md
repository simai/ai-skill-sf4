# SF4 `simai.storage` Playbook

> Источник: https://sf4.simai.pro/ru/bx/storage/, repo://bx-simai.storage/example/simai.storage, repo://bx-simai.storage/local/modules/simai.storage · Проверено: 2026-10-23 · Версия: legacy simai.storage 2.6.0 (bx-simai.storage@02c6cfea)

## Scope and version

This playbook describes the **legacy `simai.storage` API** (`\SIMAI\Storage\Storage`, `Element`, `Property`, `Set`, events `OnBefore*/OnAfter*`), as documented on `https://sf4.simai.pro/ru/bx/storage/` and kept in `bx-simai.storage/example/simai.storage` (module version 2.6.0, 2023). It applies to SF4 sites that still run that module.

The current implementation in `bx-simai.storage/local/modules/simai.storage` is a different architecture (`Simai\Storage\Entity`, `Repository`, `Service`; module-owned tables, pipelines, stages, storage groups). Do not apply the legacy class/method names to it: first read `bx-simai.storage/README.md` and `docs/developer/` and check which module the target site actually has installed (`/bitrix/modules/simai.storage/install/version.php`).

## Goal

Implement backend data flows using `simai.storage` with explicit CRUD, access, events, and search/sort update steps.

## Data Model Essentials

- Storage:
  - logical container of elements;
  - has code (`STORAGE_ID`), access, language-aware settings.
- Element:
  - record (`TYPE=I`) or section (`TYPE=S`);
  - can be linked via parent relations.
- Property:
  - typed field definition;
  - may be multilingual and/or multiple.
- Set:
  - grouping mechanism for storage/element objects.

## Core Classes

- `\SIMAI\Storage\Storage`
- `\SIMAI\Storage\Element`
- `\SIMAI\Storage\Property`
- `\SIMAI\Storage\Set`
- `\SIMAI\Storage\Search`

Entity table classes (ORM-level):

- `StorageTable`, `PropertyTable`, `PropertyLanTable`, `SetTable`, `SetLanTable`, `PropertyType`, `UserAccess`

## CRUD Pattern (Recommended)

1. Create storage:
   - `Storage::add([...])`
2. Configure storage/element properties:
   - `Property::add([...])`
   - set values via `Property::setPropValue(...)`
3. Add elements:
   - `Element::add($storageId, [...])`
4. After writing property values that affect sort/search:
   - `Property::UpdateStorageElementSorts($storageId, $elementId)`
   - `Search::UpdateStorageElementSearch($storageId, $elementId)`
5. Query:
   - `Element::getList(...)`, `Storage::getList(...)`, etc.

## Access And Validation

- Always set storage `ACCESS` and `SITE_ID` explicitly on creation.
- Check effective user access where required:
  - `Storage::getUserAccess(...)`
- Track errors via:
  - `$GLOBALS['SF_STORAGE_ERRORS']`

## Event Hooks To Respect

Common lifecycle hooks:

- `OnBeforeStorageAdd/Update/Delete`, `OnAfterStorageAdd/Update/Delete`
- `OnBeforeElementAdd/Update/Delete`, `OnAfterElementAdd/Update/Delete`
- `OnBeforePropertyAdd/Update/Delete`, `OnAfterPropertyAdd/Update/Delete`
- `OnBeforeSetAdd/Update/Delete`, `OnAfterSetAdd/Update/Delete`

Rule:

- before changing critical data flows, inspect whether project handlers depend on these events.

## Naming Constraints

- `STORAGE_ID`:
  - lowercase latin + digits + `_`;
  - starts with a letter;
  - max length 16.
- property `CODE`:
  - uppercase latin + digits + `_`;
  - starts with a letter;
  - max length 32.

## Release Safety Checklist

1. CRUD scenario tested on at least one real storage.
2. Sort/search update calls executed where required.
3. Access rules validated for editor and regular user roles.
4. Event side effects checked (or explicitly confirmed absent).
5. Rollback path documented (what to delete/revert and in what order).
