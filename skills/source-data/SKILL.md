---
name: source-data
description: 'Trigger: SourceData, add machine, add recipe, add material, connect nodes, planner data, asset_id. Explains how src/components/planner/SourceData.vue data connects so the production tree resolves.'
license: Apache-2.0
metadata:
    author: gentleman-programming
    version: '1.0'
---

## Activation Contract

Use this skill when adding or editing machines, recipes, resources, or materials in `src/components/planner/SourceData.vue`, or when a node fails to connect, shows `id + '?'`, or appears under "Recetas desconocidas".

## Mental Model

`SourceData.vue` is a Vue mixin holding three arrays. On `created()` they merge into `global_assets_dictionary`, keyed by `id`:

| Array            | `type` injected | Role                                          |
| ---------------- | --------------- | --------------------------------------------- |
| `machine_types`  | `machine`       | Buildings; hold `recipes[]`                   |
| `resource_types` | `resource`      | Raw ores / fluids (display names)             |
| `material_types` | `material`      | Intermediate & final products (display names) |

`resource_types` and `material_types` are ONLY name lookups. They do NOT produce anything — production comes from recipes.

## The Connection Rule (most important)

Nodes connect by `asset_id` ONLY. There are no explicit edges.

-   A recipe `asset_id` = what it OUTPUTS.
-   Each `ingredients[].asset_id` = an INPUT.
-   `findRecipeMachineByAssetId` (in `ThePlanner.vue`) finds the FIRST `machine_types` recipe whose `asset_id` equals the ingredient's `asset_id`. `buildNode` then recurses on that recipe's ingredients.
-   Leaf nodes are recipes with `ingredients: []` (e.g. Miner ore, Water Extractor).

So to connect node A → B: make B a recipe whose `asset_id` matches the `asset_id` A lists in `ingredients`.

## Recipe Shape

```js
{ name, id, asset_id, quantity_per_minute, ingredients: [ { asset_id, quantity_per_minute } ] }
```

## Hard Rules

-   Every `asset_id` (output AND every ingredient) MUST exist in the dictionary via `resource_types` or `material_types`, else the UI renders `asset_id + '?'`.
-   Every ingredient `asset_id` MUST be produced by some recipe, else it lands in `unknown_asset_recipes` ("Recetas desconocidas").
-   Duplicate outputs: if two recipes share an `asset_id`, the FIRST in `machine_types` order wins. Order matters.
-   `getOreNodes` counts any `asset_id` containing `_ore` or equal to `coal` as a terminal resource for totals.
-   `quantity_per_minute` is the 100% clock-speed rate; the planner scales it.

## Execution Steps

1. Add output/ingredient names to `material_types` or `resource_types` if missing (prevents `?`).
2. Add the producing recipe under the correct `machine_types[].recipes`.
3. For each ingredient, confirm a recipe already outputs that `asset_id`; if not, add one or expect the warning.
4. Verify no unintended duplicate `asset_id` shadows an existing recipe.

## Output Contract

Report: arrays touched, new `asset_id`s, which recipe produces each, and any intentionally unresolved ingredients.
