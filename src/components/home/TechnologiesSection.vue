<script setup lang="ts">
import ThreeTechCard from "../ThreeTechCard.vue";
import { onMounted } from "vue";
import { useI18n } from "vue-i18n";

const { t, tm } = useI18n();

onMounted(() => {
  // Scroll Animation Observer
  const observer = new IntersectionObserver(
    (entries: IntersectionObserverEntry[]) => {
      entries.forEach((entry) => {
        if (entry.isIntersecting) {
          entry.target.classList.add("animate-in");
        }
      });
    },
    { threshold: 0.1 }
  );

  document
    .querySelectorAll(".scroll-animate")
    .forEach((el) => observer.observe(el));
});
</script>

<template>
  <section class="technologies">
    <div class="container">
      <div class="section-header">
        <h2 class="section-title">{{ t("tech.title") }}</h2>
        <p class="section-subtitle">
          {{ t("tech.subtitle") }}
        </p>
      </div>
      <div class="tech-grid">
        <ThreeTechCard
          :title="t('tech.ml.title')"
          :description="t('tech.ml.description')"
          :scenarios="tm('tech.ml.scenarios')"
          :advantages="tm('tech.ml.advantages')"
          :cases="t('tech.ml.cases')"
          class="scroll-animate"
        />
        <ThreeTechCard
          :title="t('tech.dl.title')"
          :description="t('tech.dl.description')"
          :scenarios="tm('tech.dl.scenarios')"
          :advantages="tm('tech.dl.advantages')"
          :cases="t('tech.dl.cases')"
          class="scroll-animate"
          style="transition-delay: 100ms"
        />
        <ThreeTechCard
          :title="t('tech.nlp.title')"
          :description="t('tech.nlp.description')"
          :scenarios="tm('tech.nlp.scenarios')"
          :advantages="tm('tech.nlp.advantages')"
          :cases="t('tech.nlp.cases')"
          class="scroll-animate"
          style="transition-delay: 200ms"
        />
      </div>
    </div>
  </section>
</template>

<style scoped>
.technologies {
  padding: 6rem 0;
  background-color: var(--bg-primary);
}

.tech-grid {
  display: grid;
  grid-template-columns: 1fr;
  gap: 2rem;
  perspective: 1000px;
}

.section-header {
  text-align: center;
  margin-bottom: 4rem;
}

.section-title {
  font-size: 2.5rem;
  font-weight: 700;
  margin-bottom: 1rem;
}

.section-subtitle {
  color: var(--text-secondary);
  font-size: 1.1rem;
  max-width: 600px;
  margin: 0 auto;
}

.scroll-animate {
  opacity: 0;
  transform: translateY(30px);
  transition: opacity 0.6s ease-out, transform 0.6s ease-out;
}

.scroll-animate.animate-in {
  opacity: 1;
  transform: translateY(0);
}

@media (min-width: 768px) {
  .tech-grid {
    grid-template-columns: repeat(2, 1fr);
  }
}

@media (min-width: 1024px) {
  .tech-grid {
    grid-template-columns: repeat(3, 1fr);
  }
}
</style>
