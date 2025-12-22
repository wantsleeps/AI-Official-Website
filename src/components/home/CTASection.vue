<script setup lang="ts">
import { useI18n } from "vue-i18n";

const { t } = useI18n();
</script>

<template>
  <section class="cta">
    <div class="container">
      <div class="cta-card glass-panel">
        <div class="cta-liquid-bg"></div>
        <h2>{{ t("cta.title") }}</h2>
        <p>{{ t("cta.subtitle") }}</p>
        <button class="btn btn-primary btn-lg">{{ t("cta.button") }}</button>
      </div>
    </div>
  </section>
</template>

<style scoped>
.cta {
  padding: 6rem 0;
  background-color: var(--bg-primary);
  position: relative;
}

.cta-card {
  /* .glass-panel styles are inherited, but we can override or add to them */
  padding: 4rem 2rem;
  text-align: center;
  position: relative;
  overflow: hidden;
  border: 1px solid var(--glass-border); /* Ensure border is consistent */
}

.cta-liquid-bg {
  position: absolute;
  top: -50%;
  left: -50%;
  width: 200%;
  height: 200%;
  background: var(--gradient-liquid);
  filter: blur(100px);
  opacity: 0.15;
  animation: rotate 20s linear infinite;
  z-index: -1;
}

@keyframes rotate {
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(360deg);
  }
}

.cta-card h2 {
  font-size: 2.5rem;
  font-weight: 700;
  margin-bottom: 1rem;
}

.cta-card p {
  color: var(--text-secondary);
  font-size: 1.25rem;
  margin-bottom: 2.5rem;
}

.btn-lg {
  padding: 1rem 2.5rem;
  font-size: 1.1rem;
}
</style>
