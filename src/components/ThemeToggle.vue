<script setup>
import { ref, onMounted } from "vue";
import { Sun, Moon } from "lucide-vue-next";

const isDark = ref(false);

const toggleTheme = () => {
  isDark.value = !isDark.value;
  if (isDark.value) {
    document.documentElement.classList.add("dark");
    localStorage.setItem("theme", "dark");
  } else {
    document.documentElement.classList.remove("dark");
    localStorage.setItem("theme", "light");
  }
};

onMounted(() => {
  const savedTheme = localStorage.getItem("theme");
  if (
    savedTheme === "dark" ||
    (!savedTheme && window.matchMedia("(prefers-color-scheme: dark)").matches)
  ) {
    isDark.value = true;
    document.documentElement.classList.add("dark");
  } else {
    isDark.value = false;
    document.documentElement.classList.remove("dark");
  }
});
</script>

<template>
  <button @click="toggleTheme" class="theme-toggle" aria-label="Toggle theme">
    <div class="icon-container" :class="{ 'is-dark': isDark }">
      <Sun class="icon sun" />
      <Moon class="icon moon" />
    </div>
  </button>
</template>

<style scoped>
.theme-toggle {
  position: relative;
  width: 40px;
  height: 40px;
  border-radius: 50%;
  background-color: var(--bg-secondary);
  border: 1px solid var(--border-color);
  overflow: hidden;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.3s ease;
}

.theme-toggle:hover {
  border-color: var(--accent-primary);
}

.icon-container {
  position: relative;
  width: 20px;
  height: 20px;
}

.icon {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  transition: all 0.5s cubic-bezier(0.4, 0, 0.2, 1);
}

.sun {
  opacity: 1;
  transform: rotate(0deg) scale(1);
  color: #f59e0b;
}

.moon {
  opacity: 0;
  transform: rotate(90deg) scale(0);
  color: #60a5fa;
}

.is-dark .sun {
  opacity: 0;
  transform: rotate(-90deg) scale(0);
}

.is-dark .moon {
  opacity: 1;
  transform: rotate(0deg) scale(1);
}
</style>
