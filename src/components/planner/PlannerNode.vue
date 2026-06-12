<script>
import SourceData from './SourceData.vue'

export default {
    mixins: [SourceData],
    props: {
        node: {
            type: Object,
            required: true
        },
        level: {
            type: Number,
            default: 0
        }
    },
    data() {
        return {
            expanded: true
        }
    },
    created() {
        console.log('PlannerNode created', this.node)
    },
    computed: {
        machine_type() {
            return this.machine_types.find(machine_type => machine_type.id === this.node?.machine_type_id)
        },
        next_level() {
            return this.level + 1
        },
        asset() {
            return this.global_assets_dictionary[this.node?.recipe?.asset_id];
        }
    },
    methods: {
        toggle() {
            this.expanded = !this.expanded
        }
    }
}
</script>

<template>
    <div class="pnode" v-if="node">
        <div class="pnode-header" @click="toggle">
            <div class="pnode-left">
                <span class="pnode-machine" :style="{ color: machine_type.color }">{{ machine_type.name }}</span>
                <span class="pnode-count">
                    ×&thinsp;<strong>{{ node.net_number_of_machines }}</strong>
                    <span class="dimmed">&nbsp;({{ Math.round(node.number_of_machines * 100) / 100 }})</span>
                </span>
            </div>
            <div class="pnode-right">
                <span class="pnode-asset">{{ asset?.name || node?.recipe?.asset_id + '?' }}</span>
                <span class="pnode-qty">{{ Math.round(node.quantity * 100) / 100 }}<em>/min</em></span>
                <span class="pnode-toggle" :class="{ open: expanded }">
                    <svg v-if="node.children.length" width="12" height="12" viewBox="0 0 12 12" fill="none">
                        <path d="M2 4l4 4 4-4" stroke="currentColor" stroke-width="1.8" stroke-linecap="round"
                            stroke-linejoin="round" />
                    </svg>
                </span>
            </div>
        </div>
        <div class="pnode-body" :class="{ expanded }">
            <div class="pnode-children">
                <planner-node v-for="child in node.children" :key="child.recipe.asset_id" :node="child"
                    :level="next_level" />
            </div>
        </div>
    </div>
</template>

<style scoped>
.pnode {
    position: relative;
    margin-top: 0.5rem;
}

/* Vertical guide line for children */
.pnode-children {
    position: relative;
    padding-left: 1.5rem;
    border-left: 2px solid var(--border);
    margin-left: 1rem;
}

.pnode-header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    gap: 1rem;
    padding: 0.6rem 0.9rem;
    background: var(--bg-panel-2);
    border: 1px solid var(--border);
    border-radius: var(--radius-sm);
    cursor: pointer;
    min-height: 44px;
    transition: border-color 150ms ease, background 150ms ease;
}

.pnode-header:hover {
    border-color: var(--border);
    background: color-mix(in srgb, var(--bg-panel-2) 90%, var(--accent) 10%);
}

.pnode-left {
    display: flex;
    align-items: center;
    gap: 0.75rem;
    flex-shrink: 0;
}

.pnode-machine {
    font-family: 'Oxanium', sans-serif;
    font-size: 0.75rem;
    font-weight: 700;
    letter-spacing: 0.05em;
    text-transform: uppercase;
}

.pnode-count {
    font-size: 0.85rem;
    color: var(--text);
}

.pnode-count strong {
    font-size: 1rem;
    font-weight: 700;
}

.pnode-right {
    display: flex;
    align-items: center;
    gap: 0.75rem;
    min-width: 0;
}

.pnode-asset {
    font-size: 0.9rem;
    font-weight: 600;
    color: var(--text);
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
}

.pnode-qty {
    font-size: 0.9rem;
    font-weight: 700;
    color: var(--accent);
    white-space: nowrap;
}

.pnode-qty em {
    font-style: normal;
    font-size: 0.75em;
    color: var(--text-muted);
    font-weight: 400;
}

.pnode-toggle {
    display: flex;
    align-items: center;
    color: var(--text-muted);
    transition: transform 200ms ease, color 150ms ease;
    flex-shrink: 0;
}

.pnode-toggle.open {
    transform: rotate(180deg);
    color: var(--accent);
}

/* Expand/collapse animation */
.pnode-body {
    overflow: hidden;
    max-height: 0;
    transition: max-height 200ms ease;
}

.pnode-body.expanded {
    max-height: 9999px;
}
</style>
