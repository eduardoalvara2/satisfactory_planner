<script>

import SourceData from './SourceData.vue'
import PlannerNode from './PlannerNode.vue'

export default {
    components: { PlannerNode },
    mixins: [SourceData],
    data() {
        return {
            pivot_overclock: 100,
            pivot_machine_type: null,
            pivot_recipe: null,
            pivot_number_of_machines: 1,
            unknown_asset_recipes: []
        }
    },
    computed: {
        net_pivot_overclock() {
            return this.pivot_overclock / 100
        },
        plan() {
            if (this.pivot_machine_type && this.pivot_recipe) {
                return this.buildNode(this.pivot_machine_type, this.pivot_recipe, this.pivot_recipe.quantity_per_minute * this.pivot_number_of_machines, this.net_pivot_overclock,)
            }
            return null;
        },
        total_ores() {
            let ores = {};
            this.getOreNodes(this.plan).forEach(node => {
                if (!ores[node.recipe.asset_id]) {
                    ores[node.recipe.asset_id] = 0;
                }
                ores[node.recipe.asset_id] += node.quantity;
            });
            return ores;
        },
        machines() {
            const machines = {};
            this.getMachines(this.plan).map(node => {
                const machine_type_id = node.machine_type_id;
                const asset_id = node.recipe.asset_id || 'unknown';

                if (!machines[machine_type_id]) {
                    machines[machine_type_id] = {};
                }

                if (!machines[machine_type_id][asset_id]) {
                    machines[machine_type_id][asset_id] = {
                        asset_name: this.global_assets_dictionary[asset_id]?.name || asset_id + '?',
                        machine_type_name: node.machine_type_name,
                        number_of_machines: 0
                    }
                }

                machines[machine_type_id][asset_id].number_of_machines += node.number_of_machines;

            })
            Object.values(machines).map(machine => {
                machine.total = Object.values(machine).reduce((acc, curr) => {
                    if (curr.asset_name) {
                        acc.asset_name = null;
                    }
                    acc.number_of_machines += Math.ceil(curr.number_of_machines);
                    return acc;
                }, {
                    asset_name: null,
                    machine_type_name: 'Total ' + machine[Object.keys(machine)[0]].machine_type_name,
                    number_of_machines: 0
                });
            });

            return machines;

        },
        total_machines() {
            let machines = 0;
            const countNodes = (node) => {
                if (!node) {
                    return;
                }
                machines += node.net_number_of_machines;
                node.children.forEach(child => countNodes(child));
            }
            countNodes(this.plan);
            return machines;
        }
    },
    watch: {
        pivot_machine_type: {
            handler: 'save',
            deep: true
        },
        pivot_recipe: {
            handler: function () {
                this.unknown_asset_recipes = [];
                this.save()
            },
            deep: true
        },
        pivot_overclock: {
            handler: 'save',
            deep: true
        },
        pivot_number_of_machines: {
            handler: 'save',
            deep: true
        }
    },
    created() {
        console.log('Planner created')
        this.load()
    },
    methods: {
        save() {
            window.localStorage.setItem('pivot_machine_type_id', this.pivot_machine_type?.id)
            window.localStorage.setItem('pivot_recipe_id', this.pivot_recipe?.id)
            window.localStorage.setItem('pivot_overclock', this.pivot_overclock)
            window.localStorage.setItem('pivot_number_of_machines', this.pivot_number_of_machines)
        },
        load() {
            try {
                const pivot_machine_type_id = window.localStorage.getItem('pivot_machine_type_id')
                if (pivot_machine_type_id) {
                    this.pivot_machine_type = this.machine_types.find(machine_type => machine_type.id === pivot_machine_type_id)
                }
                const pivot_recipe_id = window.localStorage.getItem('pivot_recipe_id')
                if (pivot_recipe_id) {
                    this.pivot_recipe = this.pivot_machine_type?.recipes.find(recipe => recipe.id === pivot_recipe_id)
                }
                const pivot_overclock = window.localStorage.getItem('pivot_overclock')
                if (pivot_overclock) {
                    this.pivot_overclock = pivot_overclock
                }
                const pivot_number_of_machines = window.localStorage.getItem('pivot_number_of_machines')
                if (pivot_number_of_machines) {
                    this.pivot_number_of_machines = pivot_number_of_machines
                }
            } catch (error) {
                console.log('Error loading saved data', error)
            }
        },
        selectPivotMachineType(machine_type) {
            this.pivot_machine_type = machine_type
            this.pivot_recipe = null
        },
        selectPivotRecipe(recipe) {
            this.pivot_recipe = recipe
        },
        findRecipeMachineByAssetId(asset_id) {
            const machine_type = this.machine_types.find(machine_type => machine_type.recipes?.find(recipe => recipe.asset_id == asset_id))
            const recipe = machine_type?.recipes?.find(recipe => recipe.asset_id === asset_id)
            return { machine_type, recipe }
        },
        buildNode(machine_type, recipe, quantity = 1, overclock = 1) {
            if (!machine_type || !recipe) {
                return null;
            }
            const net_quantity = quantity * overclock;
            const net_quantity_per_minute = recipe.quantity_per_minute * overclock;
            const node = {
                machine_type_id: machine_type.id,
                machine_type_name: machine_type.name,
                quantity: net_quantity,
                number_of_machines: net_quantity / (net_quantity_per_minute),
                net_number_of_machines: Math.ceil(net_quantity / (net_quantity_per_minute)),
                recipe
            };

            node.children = recipe.ingredients.map(ingredient => {
                const found = this.findRecipeMachineByAssetId(ingredient.asset_id);
                const child_machine_type = found.machine_type;
                const child_recipe = found.recipe;
                if (!child_machine_type || !child_recipe) {
                    this.unknown_asset_recipes.push(ingredient.asset_id)
                    return null;
                }
                return this.buildNode(child_machine_type, child_recipe, ((ingredient.quantity_per_minute / recipe.quantity_per_minute) * net_quantity))
            }).filter(child => child);

            return node;
        },
        getOreNodes(node) {
            if (!node) {
                return [];
            }
            if (node.recipe.asset_id.includes('_ore') || node.recipe.asset_id == 'coal') {
                return [node];
            }
            return node.children.flatMap(child => this.getOreNodes(child));
        },
        getMachines(node) {
            if (!node) {
                return [];
            }
            return [node].concat(node.children.flatMap(child => this.getMachines(child)));
        }
    }
}
</script>

<template>
    <div class="planner-root">

        <!-- ── SELECTION PHASE ─────────────────────────────── -->
        <div class="selection-phase">

            <!-- Step 1: Elegí máquina -->
            <div v-if="pivot_machine_type == null" class="selection-step">
                <h3 class="selection-label">Seleccioná una Máquina</h3>
                <ul class="machine-grid">
                    <li v-for="machine_type in machine_types" :key="machine_type.id" class="machine-chip"
                        :style="{ '--chip-color': machine_type.color }" @click="selectPivotMachineType(machine_type)">
                        <span class="chip-bar"></span>
                        <span class="chip-name">{{ machine_type.name }}</span>
                    </li>
                </ul>
            </div>

            <!-- Step 2: Elegí receta -->
            <div v-if="pivot_machine_type && pivot_recipe == null" class="selection-step">
                <h3 class="selection-label">
                    <span class="back-btn" @click="selectPivotMachineType(null)" title="Cambiar máquina">&#8592;</span>
                    Recetas — {{ pivot_machine_type.name }}
                </h3>
                <p v-if="!pivot_machine_type?.recipes?.length" class="no-recipes">
                    Esta máquina no tiene recetas disponibles.
                </p>
                <ul class="recipe-grid">
                    <li v-for="recipe in pivot_machine_type?.recipes" :key="recipe.id" class="recipe-card"
                        @click="selectPivotRecipe(recipe)">
                        <span class="recipe-name">{{ recipe.name }}</span>
                        <span class="recipe-rate">{{ recipe.quantity_per_minute }} <em>/min</em></span>
                    </li>
                </ul>
            </div>

        </div>

        <!-- ── ACTIVE SELECTION INFO ───────────────────────── -->
        <div v-if="pivot_machine_type || pivot_recipe" class="active-info">
            <div v-if="pivot_machine_type" class="info-card" :style="{ '--card-color': pivot_machine_type.color }"
                @click="selectPivotMachineType(null)">
                <div class="info-card-header">
                    <span class="info-card-label">Máquina</span>
                    <button class="dismiss-btn" title="Cambiar">✕</button>
                </div>
                <span class="info-card-value">{{ pivot_machine_type.name }}</span>
            </div>

            <div v-if="pivot_recipe" class="info-card recipe-info-card" @click="selectPivotRecipe(null)">
                <div class="info-card-header">
                    <span class="info-card-label">Receta</span>
                    <button class="dismiss-btn" title="Cambiar">✕</button>
                </div>
                <span class="info-card-value">{{ pivot_recipe.name }}</span>
                <div class="recipe-details">
                    <span class="recipe-output">
                        {{ global_assets_dictionary[pivot_recipe.asset_id]?.name }}
                        <strong class="qty">{{ pivot_recipe.quantity_per_minute }}/min</strong>
                    </span>
                    <ul v-if="pivot_recipe.ingredients.length" class="ingredient-list">
                        <li v-for="ingredient in pivot_recipe.ingredients" :key="ingredient.asset_id">
                            <span>{{ global_assets_dictionary[ingredient.asset_id]?.name || ingredient.asset_id + '?'
                                }}</span>
                            <strong class="qty">{{ ingredient.quantity_per_minute }}/min</strong>
                        </li>
                    </ul>
                </div>
            </div>
        </div>

        <!-- ── PLAN PANEL ──────────────────────────────────── -->
        <div v-if="pivot_recipe" class="plan-panel">

            <!-- Controls -->
            <div class="plan-controls">
                <h2 class="plan-title">Plan de Producción</h2>
                <div class="controls-row">
                    <label class="control-field">
                        <span class="control-label">Máquinas</span>
                        <input type="number" v-model="pivot_number_of_machines" min="1" />
                    </label>
                    <label class="control-field">
                        <span class="control-label">Overclock %</span>
                        <input type="number" v-model="pivot_overclock" min="1" max="250" />
                    </label>
                </div>
            </div>

            <!-- Unknown recipes warning -->
            <div v-if="unknown_asset_recipes.length > 0" class="warn-block">
                <span class="warn-title">⚠ Recetas desconocidas</span>
                <ul>
                    <li v-for="asset_id in unknown_asset_recipes" :key="asset_id">
                        {{ global_assets_dictionary[asset_id]?.name || asset_id + '?' }}
                    </li>
                </ul>
            </div>

            <!-- Summary grid -->
            <div class="summary-grid">

                <!-- Total ores -->
                <div class="summary-block" v-if="Object.keys(total_ores).length">
                    <h4 class="summary-block-title">Recursos Totales</h4>
                    <ul class="summary-list">
                        <li v-for="(quantity, asset_id) in total_ores" :key="asset_id">
                            <span>{{ global_assets_dictionary[asset_id]?.name || asset_id + '?' }}</span>
                            <strong class="qty">{{ quantity }}/min</strong>
                        </li>
                    </ul>
                </div>

                <!-- Machines needed -->
                <div class="summary-block" v-if="Object.keys(machines).length">
                    <h4 class="summary-block-title">Máquinas Necesarias</h4>
                    <div v-for="(machines_by_assets, machine_id) in machines" :key="machine_id" class="machine-group">
                        <ul class="summary-list">
                            <li v-for="({ asset_name, machine_type_name, number_of_machines }, j) in machines_by_assets"
                                :key="j" :class="{ 'total-row': j === 'total' }">
                                <span>
                                    {{ machine_type_name }}
                                    <em v-if="asset_name"> ({{ asset_name }})</em>
                                </span>
                                <strong class="qty" :class="{ 'qty-total': j === 'total' }">
                                    {{ Math.ceil(number_of_machines) }}
                                </strong>
                            </li>
                        </ul>
                    </div>
                </div>

                <!-- Total machines -->
                <div class="summary-block summary-block--total">
                    <h4 class="summary-block-title">Total de Máquinas</h4>
                    <span class="total-machines-num">{{ total_machines }}</span>
                </div>

            </div>

            <!-- Production tree -->
            <div class="tree-section">
                <h4 class="summary-block-title">Árbol de Producción</h4>
                <planner-node :node="plan" />
            </div>

        </div>

    </div>
</template>

<style scoped>
/* ── Root layout ─────────────────────────────────────────── */
.planner-root {
    display: flex;
    flex-direction: column;
    gap: 1.5rem;
}

/* ── Selection phase ─────────────────────────────────────── */
.selection-phase {
    background: var(--bg-panel);
    border: 1px solid var(--border);
    border-radius: var(--radius);
    padding: 1.5rem;
}

.selection-label {
    font-size: 0.8rem;
    color: var(--text-muted);
    margin-bottom: 1rem;
    letter-spacing: 0.1em;
}

.back-btn {
    display: inline-flex;
    align-items: center;
    justify-content: center;
    width: 28px;
    height: 28px;
    background: var(--bg-panel-2);
    border: 1px solid var(--border);
    border-radius: var(--radius-sm);
    cursor: pointer;
    margin-right: 0.5rem;
    color: var(--accent);
    font-size: 1rem;
    transition: background 150ms ease, border-color 150ms ease;
}

.back-btn:hover {
    background: var(--bg-panel-2);
    border-color: var(--accent);
}

/* Machine grid */
.machine-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(160px, 1fr));
    gap: 0.75rem;
    list-style: none;
    padding: 0;
}

.machine-chip {
    display: flex;
    align-items: center;
    gap: 0.75rem;
    background: var(--bg-panel-2);
    border: 1px solid var(--border);
    border-radius: var(--radius);
    padding: 0.75rem 1rem;
    cursor: pointer;
    min-height: 48px;
    transition: border-color 150ms ease, background 150ms ease, box-shadow 150ms ease;
    overflow: hidden;
}

.machine-chip:hover {
    border-color: var(--chip-color, var(--accent));
    background: color-mix(in srgb, var(--chip-color, var(--accent)) 8%, var(--bg-panel-2));
    box-shadow: 0 0 0 1px var(--chip-color, var(--accent)), 0 0 12px color-mix(in srgb, var(--chip-color, var(--accent)) 35%, transparent);
}

.chip-bar {
    flex-shrink: 0;
    width: 4px;
    height: 28px;
    background: var(--chip-color, var(--accent));
    border-radius: 2px;
}

.chip-name {
    font-family: 'Oxanium', sans-serif;
    font-size: 0.82rem;
    font-weight: 600;
    letter-spacing: 0.03em;
    text-transform: uppercase;
    color: var(--text);
}

/* Recipe grid */
.recipe-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
    gap: 0.75rem;
    list-style: none;
    padding: 0;
}

.recipe-card {
    display: flex;
    flex-direction: column;
    gap: 0.25rem;
    background: var(--bg-panel-2);
    border: 1px solid var(--border);
    border-radius: var(--radius);
    padding: 0.85rem 1rem;
    cursor: pointer;
    min-height: 56px;
    transition: border-color 150ms ease, background 150ms ease;
}

.recipe-card:hover {
    border-color: var(--accent);
    background: color-mix(in srgb, var(--accent) 6%, var(--bg-panel-2));
}

.recipe-name {
    font-size: 0.9rem;
    font-weight: 600;
    color: var(--text);
}

.recipe-rate {
    font-size: 0.8rem;
    color: var(--accent);
    font-weight: 600;
}

.recipe-rate em {
    font-style: normal;
    color: var(--text-muted);
    font-weight: 400;
}

.no-recipes {
    color: var(--text-muted);
    font-size: 0.9rem;
}

/* ── Active info cards ───────────────────────────────────── */
.active-info {
    display: flex;
    flex-wrap: wrap;
    gap: 1rem;
}

.info-card {
    flex: 1;
    min-width: 220px;
    background: var(--bg-panel);
    border: 1px solid var(--border);
    border-top: 3px solid var(--card-color, var(--accent));
    border-radius: var(--radius);
    padding: 1rem 1.25rem;
    cursor: pointer;
    transition: border-color 150ms ease;
    clip-path: polygon(0 0, calc(100% - 12px) 0, 100% 12px, 100% 100%, 0 100%);
}

.info-card:hover {
    border-color: var(--accent-lo);
}

.info-card-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 0.5rem;
}

.info-card-label {
    font-size: 0.7rem;
    font-weight: 600;
    letter-spacing: 0.12em;
    text-transform: uppercase;
    color: var(--text-muted);
}

.dismiss-btn {
    display: flex;
    align-items: center;
    justify-content: center;
    width: 22px;
    height: 22px;
    border-radius: 50%;
    background: transparent;
    border: 1px solid var(--border);
    color: var(--text-muted);
    font-size: 0.65rem;
    cursor: pointer;
    transition: border-color 150ms ease, color 150ms ease, background 150ms ease;
    padding: 0;
    line-height: 1;
}

.dismiss-btn:hover {
    border-color: var(--accent);
    color: var(--accent);
    background: color-mix(in srgb, var(--accent) 10%, transparent);
}

.info-card-value {
    display: block;
    font-family: 'Oxanium', sans-serif;
    font-size: 1rem;
    font-weight: 600;
    color: var(--text);
}

.recipe-details {
    margin-top: 0.75rem;
    padding-top: 0.75rem;
    border-top: 1px solid var(--border);
}

.recipe-output {
    display: flex;
    justify-content: space-between;
    font-size: 0.9rem;
    color: var(--text-muted);
    margin-bottom: 0.5rem;
}

.ingredient-list {
    list-style: none;
    padding: 0;
    display: flex;
    flex-direction: column;
    gap: 0.3rem;
}

.ingredient-list li {
    display: flex;
    justify-content: space-between;
    font-size: 0.85rem;
    color: var(--text-muted);
}

/* ── Qty accent ──────────────────────────────────────────── */
.qty {
    color: var(--accent);
    font-weight: 700;
    font-size: 0.9em;
    white-space: nowrap;
}

/* ── Plan panel ──────────────────────────────────────────── */
.plan-panel {
    background: var(--bg-panel);
    border: 1px solid var(--border);
    border-radius: var(--radius);
    overflow: hidden;
}

.plan-controls {
    display: flex;
    flex-wrap: wrap;
    align-items: center;
    justify-content: space-between;
    gap: 1rem;
    padding: 1.25rem 1.5rem;
    background: var(--bg-panel-2);
    border-bottom: 1px solid var(--border);
}

.plan-title {
    font-size: 1rem;
    letter-spacing: 0.08em;
    color: var(--text);
}

.controls-row {
    display: flex;
    gap: 1.5rem;
    flex-wrap: wrap;
}

.control-field {
    display: flex;
    flex-direction: column;
    gap: 0.3rem;
    cursor: default;
}

.control-label {
    font-size: 0.7rem;
    font-weight: 600;
    letter-spacing: 0.1em;
    text-transform: uppercase;
    color: var(--text-muted);
}

input[type="number"] {
    width: 90px;
    padding: 0.5rem 0.75rem;
    background: var(--bg-deep);
    color: var(--text);
    border: 1px solid var(--border);
    border-radius: var(--radius-sm);
    font-family: 'Rajdhani', sans-serif;
    font-size: 1rem;
    font-weight: 600;
    text-align: center;
    transition: border-color 150ms ease, box-shadow 150ms ease;
    appearance: textfield;
}

input[type="number"]:focus-visible {
    outline: none;
    border-color: var(--accent);
    box-shadow: var(--glow-accent);
}

/* Warning block */
.warn-block {
    margin: 1rem 1.5rem;
    padding: 0.85rem 1rem;
    background: color-mix(in srgb, var(--warn) 10%, var(--bg-panel-2));
    border: 1px solid var(--warn);
    border-radius: var(--radius-sm);
    color: var(--warn);
}

.warn-title {
    display: block;
    font-family: 'Oxanium', sans-serif;
    font-size: 0.78rem;
    font-weight: 700;
    letter-spacing: 0.1em;
    text-transform: uppercase;
    margin-bottom: 0.5rem;
}

.warn-block ul {
    padding-left: 1.2rem;
    font-size: 0.9rem;
}

/* Summary grid */
.summary-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(240px, 1fr));
    gap: 0;
    border-bottom: 1px solid var(--border);
}

.summary-block {
    padding: 1.25rem 1.5rem;
    border-right: 1px solid var(--border);
}

.summary-block:last-child {
    border-right: none;
}

.summary-block--total {
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    gap: 0.5rem;
}

.summary-block-title {
    font-size: 0.7rem;
    font-weight: 700;
    letter-spacing: 0.12em;
    text-transform: uppercase;
    color: var(--text-muted);
    margin-bottom: 0.75rem;
}

.summary-list {
    list-style: none;
    padding: 0;
    display: flex;
    flex-direction: column;
    gap: 0.45rem;
}

.summary-list li {
    display: flex;
    justify-content: space-between;
    align-items: center;
    gap: 1rem;
    font-size: 0.9rem;
    color: var(--text);
    padding: 0.3rem 0;
    border-bottom: 1px solid color-mix(in srgb, var(--border) 50%, transparent);
}

.summary-list li:last-child {
    border-bottom: none;
}

.total-row {
    background: color-mix(in srgb, var(--accent) 6%, transparent);
    border-radius: var(--radius-sm);
    padding: 0.35rem 0.5rem !important;
    margin-top: 0.25rem;
}

.total-row em {
    font-style: italic;
    color: var(--text-muted);
    font-size: 0.85em;
}

.qty-total {
    font-size: 1.1em;
}

.machine-group {
    margin-bottom: 0.75rem;
}

.total-machines-num {
    font-family: 'Oxanium', sans-serif;
    font-size: 3rem;
    font-weight: 700;
    color: var(--accent);
    line-height: 1;
}

/* Tree section */
.tree-section {
    padding: 1.25rem 1.5rem;
}

.tree-section .summary-block-title {
    margin-bottom: 1rem;
}

/* ── Responsive ──────────────────────────────────────────── */
@media (max-width: 900px) {
    .summary-grid {
        grid-template-columns: 1fr;
    }

    .summary-block {
        border-right: none;
        border-bottom: 1px solid var(--border);
    }

    .summary-block:last-child {
        border-bottom: none;
    }

    .active-info {
        flex-direction: column;
    }

    .plan-controls {
        flex-direction: column;
        align-items: flex-start;
    }
}
</style>
