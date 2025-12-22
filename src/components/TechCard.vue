<script setup lang="ts">
import { type Component } from "vue";

interface Props {
  title: string;
  icon: Component;
  description: string;
  scenarios: string[];
  advantages: string[];
  cases: string;
}

defineProps<Props>();
</script>

<template>
  <div class="tech-card-container">
    <div class="tech-card">
      <!-- Front Side -->
      <div class="card-face card-front">
        <div class="icon-wrapper">
          <component :is="icon" :size="48" />
        </div>
        <h3>{{ title }}</h3>
        <p>{{ description }}</p>
        <div class="hint">Hover to explore</div>
      </div>

      <!-- Back Side -->
      <div class="card-face card-back">
        <div class="back-content">
          <h4>Application Scenarios</h4>
          <ul>
            <li v-for="(item, index) in scenarios" :key="index">{{ item }}</li>
          </ul>

          <h4>Key Advantages</h4>
          <ul>
            <li v-for="(item, index) in advantages" :key="index">{{ item }}</li>
          </ul>

          <div class="cases"><strong>Use Cases:</strong> {{ cases }}</div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.tech-card-container {
  width: 100%;
  height: 400px;
  perspective: 1000px;
  cursor: pointer;
}

.tech-card {
  position: relative;
  width: 100%;
  height: 100%;
  text-align: center;
  transition: transform 0.8s cubic-bezier(0.4, 0, 0.2, 1);
  transform-style: preserve-3d;
}

.tech-card-container:hover .tech-card {
  transform: rotateY(180deg);
}

.card-face {
  position: absolute;
  width: 100%;
  height: 100%;
  -webkit-backface-visibility: hidden; /* Safari */
  backface-visibility: hidden;
  border-radius: 1.5rem;
  padding: 2rem;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  box-shadow: var(--glass-shadow);
  border: 1px solid var(--glass-border);
  overflow: hidden;
  background: var(--glass-bg);
  backdrop-filter: var(--glass-blur);
  -webkit-backdrop-filter: var(--glass-blur);
}

/* Front Styling */
.card-front {
  background: linear-gradient(
    145deg,
    rgba(255, 255, 255, 0.1) 0%,
    rgba(255, 255, 255, 0.05) 100%
  );
}

.icon-wrapper {
  width: 80px;
  height: 80px;
  background: rgba(37, 99, 235, 0.1);
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  color: var(--accent-primary);
  margin-bottom: 1.5rem;
  transition: transform 0.3s ease;
}

.tech-card-container:hover .icon-wrapper {
  transform: scale(1.1);
}

.card-front h3 {
  font-size: 1.75rem;
  font-weight: 700;
  margin-bottom: 1rem;
  background: linear-gradient(to right, #fff, #94a3b8);
  background-clip: text;
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}

.card-front p {
  color: var(--text-secondary);
  line-height: 1.6;
  font-size: 1.1rem;
}

.hint {
  margin-top: auto;
  font-size: 0.875rem;
  color: var(--accent-primary);
  opacity: 0.8;
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

/* Back Styling */
.card-back {
  background: var(--accent-primary);
  background: linear-gradient(135deg, var(--accent-primary) 0%, #1d4ed8 100%);
  color: white;
  transform: rotateY(180deg);
  text-align: left;
  align-items: flex-start;
  justify-content: flex-start;
}

.back-content {
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.card-back h4 {
  font-size: 1.1rem;
  font-weight: 600;
  margin-bottom: 0.5rem;
  border-bottom: 1px solid rgba(255, 255, 255, 0.2);
  padding-bottom: 0.25rem;
}

.card-back ul {
  list-style: none;
  padding: 0;
  margin: 0;
}

.card-back li {
  font-size: 0.95rem;
  margin-bottom: 0.5rem;
  position: relative;
  padding-left: 1.25rem;
  opacity: 0.9;
}

.card-back li::before {
  content: "•";
  position: absolute;
  left: 0;
  color: rgba(255, 255, 255, 0.6);
}

.cases {
  margin-top: auto;
  font-size: 0.9rem;
  background: rgba(0, 0, 0, 0.2);
  padding: 0.75rem;
  border-radius: 0.5rem;
}
</style>
