<script setup>
import ThePlanner from '../components/planner/ThePlanner.vue'
</script>

<template>
    <div class="planner-view">
        <div class="planner-view-header">
            <h1 class="planner-view-title">Planner de Producción</h1>
            <p class="planner-view-sub">Calculá cadenas completas con cantidades por minuto</p>
        </div>
        <ThePlanner />
    </div>
</template>

<style scoped>
.planner-view {
    width: 100%;
}

.planner-view-header {
    margin-bottom: 2rem;
    padding-bottom: 1rem;
    border-bottom: 1px solid var(--border);
}

.planner-view-title {
    font-size: 1.6rem;
    color: var(--text);
    margin-bottom: 0.25rem;
}

.planner-view-sub {
    font-size: 0.9rem;
    color: var(--text-muted);
    letter-spacing: 0.02em;
}
</style>
