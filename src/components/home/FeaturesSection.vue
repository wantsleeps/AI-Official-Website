<script setup lang="ts">
import { Zap, Shield, Globe, Cpu } from "lucide-vue-next";
import { onMounted } from "vue";
import { useI18n } from "vue-i18n";

const { t } = useI18n();

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
  <section id="features" class="features">
    <div class="container">
      <div class="section-header">
        <h2 class="section-title">
          {{ t("features.title") }}
          <span class="text-gradient">{{ t("features.brand") }}</span>
        </h2>
        <p class="section-subtitle">
          {{ t("features.subtitle") }}
        </p>
      </div>

      <div class="features-grid">
        <div class="feature-card scroll-animate">
          <div class="feature-icon">
            <Zap />
          </div>
          <h3>{{ t("features.fast.title") }}</h3>
          <p>
            {{ t("features.fast.desc") }}
          </p>
        </div>
        <div
          class="feature-card scroll-animate"
          style="transition-delay: 100ms"
        >
          <div class="feature-icon">
            <Shield />
          </div>
          <h3>{{ t("features.secure.title") }}</h3>
          <p>
            {{ t("features.secure.desc") }}
          </p>
        </div>
        <div
          class="feature-card scroll-animate"
          style="transition-delay: 200ms"
        >
          <div class="feature-icon">
            <Globe />
          </div>
          <h3>{{ t("features.global.title") }}</h3>
          <p>
            {{ t("features.global.desc") }}
          </p>
        </div>
        <div
          class="feature-card scroll-animate"
          style="transition-delay: 300ms"
        >
          <div class="feature-icon">
            <Cpu />
          </div>
          <h3>{{ t("features.reasoning.title") }}</h3>
          <p>
            {{ t("features.reasoning.desc") }}
          </p>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
.features {
  padding: 6rem 0;
  background-color: var(--bg-primary);
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

.features-grid {
  display: grid;
  grid-template-columns: 1fr;
  gap: 2rem;
}

.feature-card {
  background-color: var(--bg-secondary);
  padding: 2rem;
  border-radius: 1rem;
  transition: all 0.3s ease;
  border: 1px solid transparent;
  position: relative;
  overflow: hidden;
}

.feature-card:hover {
  transform: translateY(-10px);
  border-color: var(--accent-primary);
  box-shadow: 0 10px 30px -10px rgba(37, 99, 235, 0.3);
}

.feature-card::before {
  content: "";
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: radial-gradient(
    circle at center,
    rgba(37, 99, 235, 0.1) 0%,
    transparent 70%
  );
  opacity: 0;
  transition: opacity 0.3s ease;
}

.feature-card:hover::before {
  opacity: 1;
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

.feature-icon {
  width: 48px;
  height: 48px;
  background-color: rgba(37, 99, 235, 0.1);
  border-radius: 12px;
  display: flex;
  align-items: center;
  justify-content: center;
  color: var(--accent-primary);
  margin-bottom: 1.5rem;
}

.feature-card h3 {
  font-size: 1.25rem;
  font-weight: 600;
  margin-bottom: 0.75rem;
}

.feature-card p {
  color: var(--text-secondary);
  line-height: 1.6;
}

@media (min-width: 768px) {
  .features-grid {
    grid-template-columns: repeat(2, 1fr);
  }
}

@media (min-width: 1024px) {
  .features-grid {
    grid-template-columns: repeat(4, 1fr);
  }
}
</style>
