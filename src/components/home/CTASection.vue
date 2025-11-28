<script setup lang="ts">
import { useI18n } from "vue-i18n";

const { t } = useI18n();
</script>

<template>
  <section class="cta">
    <div class="container">
      <div class="cta-card">
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
}

.cta-card {
  background: var(--gradient-hero);
  border-radius: 2rem;
  padding: 4rem 2rem;
  text-align: center;
  position: relative;
  overflow: hidden;
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
