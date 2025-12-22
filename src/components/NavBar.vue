<script setup>
import { ref, onMounted, onUnmounted } from "vue";
import { RouterLink } from "vue-router";
import { Sparkles, Menu, X } from "lucide-vue-next";
import ThemeToggle from "./ThemeToggle.vue";
import { useI18n } from "vue-i18n";

const { t } = useI18n();

const isScrolled = ref(false);
const isMobileMenuOpen = ref(false);

const handleScroll = () => {
  isScrolled.value = window.scrollY > 20;
};

const toggleMobileMenu = () => {
  isMobileMenuOpen.value = !isMobileMenuOpen.value;
};

onMounted(() => {
  window.addEventListener("scroll", handleScroll);
});

onUnmounted(() => {
  window.removeEventListener("scroll", handleScroll);
});
</script>

<template>
  <nav class="navbar" :class="{ 'is-scrolled': isScrolled }">
    <div class="container navbar-content">
      <RouterLink to="/" class="logo">
        <Sparkles class="logo-icon" />
        <span class="logo-text">AI Nexus</span>
      </RouterLink>

      <div class="desktop-menu">
        <a href="#features" class="nav-link">{{ t("nav.features") }}</a>
        <a href="#about" class="nav-link">{{ t("nav.about") }}</a>
        <a href="#pricing" class="nav-link">{{ t("nav.pricing") }}</a>
        <ThemeToggle />
        <button class="btn btn-primary">{{ t("nav.getStarted") }}</button>
      </div>

      <div class="mobile-actions">
        <ThemeToggle />
        <button class="menu-btn" @click="toggleMobileMenu">
          <Menu v-if="!isMobileMenuOpen" />
          <X v-else />
        </button>
      </div>
    </div>

    <!-- Mobile Menu -->
    <div class="mobile-menu" :class="{ 'is-open': isMobileMenuOpen }">
      <a
        href="#features"
        class="mobile-link"
        @click="isMobileMenuOpen = false"
        >{{ t("nav.features") }}</a
      >
      <a href="#about" class="mobile-link" @click="isMobileMenuOpen = false">{{
        t("nav.about")
      }}</a>
      <a
        href="#pricing"
        class="mobile-link"
        @click="isMobileMenuOpen = false"
        >{{ t("nav.pricing") }}</a
      >
      <button class="btn btn-primary mobile-cta">
        {{ t("nav.getStarted") }}
      </button>
    </div>
  </nav>
</template>

<style scoped>
.navbar {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  z-index: 1000;
  padding: 1.5rem 0;
  transition: all 0.3s ease;
  background: transparent;
}

.navbar.is-scrolled {
  padding: 1rem 0;
  background: var(--glass-bg);
  backdrop-filter: var(--glass-blur);
  -webkit-backdrop-filter: var(--glass-blur);
  border-bottom: 1px solid var(--glass-border);
  box-shadow: var(--glass-shadow);
}

.navbar-content {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.logo {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  font-weight: 700;
  font-size: 1.5rem;
  color: var(--text-primary);
}

.logo-icon {
  color: var(--accent-primary);
  width: 24px;
  height: 24px;
}

.desktop-menu {
  display: none;
  align-items: center;
  gap: 2rem;
}

.nav-link {
  color: var(--text-secondary);
  font-weight: 500;
  transition: color 0.2s ease;
}

.nav-link:hover {
  color: var(--accent-primary);
}

.mobile-actions {
  display: flex;
  align-items: center;
  gap: 1rem;
}

.menu-btn {
  color: var(--text-primary);
  padding: 0.5rem;
}

.mobile-menu {
  position: fixed;
  top: 70px;
  left: 0;
  width: 100%;
  background: var(--bg-primary);
  padding: 2rem;
  display: flex;
  flex-direction: column;
  gap: 1.5rem;
  border-bottom: 1px solid var(--border-color);
  transform: translateY(-150%);
  transition: transform 0.3s ease;
  z-index: 999;
}

.mobile-menu.is-open {
  transform: translateY(0);
  box-shadow: 0 10px 30px -10px rgba(0, 0, 0, 0.1);
}

.mobile-link {
  font-size: 1.1rem;
  font-weight: 500;
  color: var(--text-primary);
}

.mobile-cta {
  width: 100%;
  margin-top: 1rem;
}

@media (min-width: 768px) {
  .desktop-menu {
    display: flex;
  }

  .mobile-actions {
    display: none;
  }

  .mobile-menu {
    display: none;
  }
}
</style>
