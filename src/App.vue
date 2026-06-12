<script setup>
import { RouterLink, RouterView } from 'vue-router'
</script>

<template>
    <header class="hud-topbar">
        <div class="hud-brand">
            <span class="hud-title">Satisfactory</span>
            <span class="hud-subtitle">Planner</span>
            <span class="hud-tag">1.2</span>
        </div>
        <nav class="hud-nav">
            <RouterLink to="/">Home</RouterLink>
            <RouterLink to="/planner">Planner</RouterLink>
        </nav>
    </header>

    <main>
        <RouterView />
    </main>
</template>

<style scoped>
.hud-topbar {
    position: sticky;
    top: 0;
    z-index: 100;
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 0 2rem;
    height: 56px;
    background: var(--bg-panel);
    border-bottom: 2px solid var(--accent);
    box-shadow: var(--shadow);
    margin-bottom: 2rem;
}

.hud-brand {
    display: flex;
    align-items: baseline;
    gap: 0.4rem;
}

.hud-title {
    font-family: 'Oxanium', sans-serif;
    font-size: 1.15rem;
    font-weight: 700;
    letter-spacing: 0.1em;
    text-transform: uppercase;
    color: var(--text);
}

.hud-subtitle {
    font-family: 'Oxanium', sans-serif;
    font-size: 1.15rem;
    font-weight: 400;
    letter-spacing: 0.06em;
    text-transform: uppercase;
    color: var(--accent);
}

.hud-tag {
    font-family: 'Oxanium', sans-serif;
    font-size: 0.7rem;
    font-weight: 700;
    letter-spacing: 0.08em;
    color: var(--bg-deep);
    background: var(--accent);
    padding: 1px 6px;
    border-radius: var(--radius-sm);
    margin-left: 2px;
}

.hud-nav {
    display: flex;
    align-items: center;
    gap: 0;
}

.hud-nav a {
    display: flex;
    align-items: center;
    height: 56px;
    padding: 0 1.25rem;
    font-family: 'Oxanium', sans-serif;
    font-size: 0.85rem;
    font-weight: 600;
    letter-spacing: 0.06em;
    text-transform: uppercase;
    color: var(--text-muted);
    text-decoration: none;
    border-bottom: 2px solid transparent;
    margin-bottom: -2px;
    transition: color 150ms ease, border-color 150ms ease;
}

.hud-nav a:hover {
    color: var(--text);
    border-bottom-color: var(--accent-lo);
}

.hud-nav a.router-link-exact-active {
    color: var(--accent);
    border-bottom-color: var(--accent);
}

main {
    padding-top: 1rem;
}
</style>
