<script setup lang="ts">
import { Languages } from "lucide-vue-next";
import { useI18n } from "vue-i18n";
import HeroSection from "../components/home/HeroSection.vue";
import TechnologiesSection from "../components/home/TechnologiesSection.vue";
import FeaturesSection from "../components/home/FeaturesSection.vue";
import CTASection from "../components/home/CTASection.vue";

const { locale } = useI18n();

const toggleLanguage = () => {
  console.log("Toggling language. Current:", locale.value);
  locale.value = locale.value === "en" ? "zh" : "en";
  console.log("New:", locale.value);
};
</script>

<template>
  <div class="home">
    <!-- Language Switcher -->
    <button class="lang-switch" @click="toggleLanguage">
      <Languages :size="20" />
      {{ locale === "en" ? "中文" : "English" }}
    </button>

    <HeroSection />
    <TechnologiesSection />
    <FeaturesSection />
    <CTASection />
  </div>
</template>

<style scoped>
.lang-switch {
  position: fixed;
  top: 20px;
  right: 20px;
  z-index: 9999;
  display: flex;
  align-items: center;
  gap: 8px;
  padding: 8px 16px;
  background: var(--bg-secondary);
  border: 1px solid var(--border-color);
  border-radius: 20px;
  color: var(--text-primary);
  cursor: pointer;
  transition: all 0.3s ease;
  box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1),
    0 2px 4px -1px rgba(0, 0, 0, 0.06);
}

.lang-switch:hover {
  background: var(--accent-primary);
  color: white;
  border-color: var(--accent-primary);
}
</style>
