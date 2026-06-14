# AGENTS.md

Guía para agentes de IA que trabajan en este repositorio.

## Project Skills

| Skill                                      | Cuándo usarlo                                                                                                                                                                               |
| ------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [source-data](skills/source-data/SKILL.md) | Agregar o editar máquinas, recetas, recursos o materiales en `src/components/planner/SourceData.vue`, o cuando un nodo no conecta / muestra `id + '?'` / aparece en "Recetas desconocidas". |

## Stack

-   Vue 3 (Options API), Vite, Vue Router.
-   El planner vive en `src/components/planner/`: `SourceData.vue` (datos, mixin), `ThePlanner.vue` (lógica de árbol), `PlannerNode.vue` (nodo recursivo).
