<script setup>
import { useRouter } from 'vue-router'
const router = useRouter()
</script>

<template>
    <div class="home-hero">
        <div class="hero-content">
            <p class="hero-eyebrow">FICSIT Inc. — Production Intelligence</p>
            <h1 class="hero-title">Satisfactory<br><span class="accent">Planner</span></h1>
            <p class="hero-desc">
                Calculá cadenas de producción completas para Satisfactory 1.2.
                Seleccioná máquinas, elegí recetas y visualizá el árbol de dependencias
                con cantidades por minuto en tiempo real.
            </p>
            <button class="cta-btn" @click="router.push('/planner')">
                <span>Abrir Planner</span>
                <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5">
                    <path d="M5 12h14M13 6l6 6-6 6" />
                </svg>
            </button>
        </div>
        <div class="hero-deco" aria-hidden="true">
            <div class="deco-grid"></div>
        </div>
    </div>
</template>

<style scoped>
.home-hero {
    position: relative;
    display: flex;
    align-items: center;
    justify-content: center;
    min-height: calc(100vh - 56px - 2rem);
    overflow: hidden;
}

.hero-content {
    position: relative;
    z-index: 1;
    max-width: 600px;
    text-align: center;
    padding: 2rem;
}

.hero-eyebrow {
    font-family: 'Oxanium', sans-serif;
    font-size: 0.78rem;
    font-weight: 600;
    letter-spacing: 0.16em;
    text-transform: uppercase;
    color: var(--accent);
    margin-bottom: 1.2rem;
}

.hero-title {
    font-family: 'Oxanium', sans-serif;
    font-size: clamp(2.8rem, 7vw, 5rem);
    font-weight: 700;
    letter-spacing: 0.04em;
    text-transform: uppercase;
    color: var(--text);
    line-height: 1.05;
    margin-bottom: 1.5rem;
}

.hero-title .accent {
    color: var(--accent);
}

.hero-desc {
    font-size: 1.05rem;
    color: var(--text-muted);
    line-height: 1.7;
    margin-bottom: 2.5rem;
    max-width: 480px;
    margin-left: auto;
    margin-right: auto;
}

.cta-btn {
    display: inline-flex;
    align-items: center;
    gap: 0.6rem;
    padding: 0.85rem 2rem;
    background: var(--accent);
    color: #0E1419;
    font-family: 'Oxanium', sans-serif;
    font-size: 0.9rem;
    font-weight: 700;
    letter-spacing: 0.1em;
    text-transform: uppercase;
    border: none;
    border-radius: var(--radius);
    cursor: pointer;
    transition: background 150ms ease, box-shadow 150ms ease, transform 150ms ease;
}

.cta-btn:hover {
    background: var(--accent-hi);
    box-shadow: var(--glow-accent);
    transform: translateY(-1px);
}

.cta-btn:active {
    background: var(--accent-lo);
    transform: translateY(0);
}

/* Decorative grid background */
.hero-deco {
    position: absolute;
    inset: 0;
    z-index: 0;
    opacity: 0.04;
    pointer-events: none;
}

.deco-grid {
    width: 100%;
    height: 100%;
    background-image:
        linear-gradient(var(--accent) 1px, transparent 1px),
        linear-gradient(90deg, var(--accent) 1px, transparent 1px);
    background-size: 48px 48px;
    mask-image: radial-gradient(ellipse 70% 80% at 50% 50%, black 20%, transparent 80%);
}
</style>
