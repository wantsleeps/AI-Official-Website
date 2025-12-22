<script setup lang="ts">
import { ArrowRight } from "lucide-vue-next";
import ParticleBackground from "../ParticleBackground.vue";
import { ref, onMounted, watch } from "vue";
import { useI18n } from "vue-i18n";

const { t, locale } = useI18n();

// Headline Typing Logic
const typedText = ref("");
const isTyping = ref(true);

const typeText = () => {
  const fullText = t("hero.typing");
  typedText.value = ""; // Reset
  isTyping.value = true;

  let i = 0;
  const speed = 100;

  const type = () => {
    if (i < fullText.length) {
      typedText.value += fullText.charAt(i);
      i++;
      setTimeout(type, speed);
    } else {
      isTyping.value = false;
    }
  };

  type();
};

// Watch for locale changes to re-trigger typing
watch(locale, () => {
  typeText();
});

// Code Block Typing Logic
interface Token {
  text: string;
  class?: string;
}

interface CodeLine {
  indent?: boolean;
  tokens: Token[];
}

const displayedCode = ref<CodeLine[]>([]);
const codeBlockData: CodeLine[] = [
  {
    tokens: [
      { text: "const", class: "keyword" },
      { text: " " },
      { text: "future", class: "variable" },
      { text: " = " },
      { text: "await", class: "keyword" },
      { text: " " },
      { text: "AI.create", class: "function" },
      { text: "({" },
    ],
  },
  {
    indent: true,
    tokens: [
      { text: "intelligence", class: "property" },
      { text: ": " },
      { text: "'limitless'", class: "string" },
      { text: "," },
    ],
  },
  {
    indent: true,
    tokens: [
      { text: "creativity", class: "property" },
      { text: ": " },
      { text: "'unbound'", class: "string" },
    ],
  },
  {
    tokens: [{ text: "});" }],
  },
  {
    tokens: [{ text: "// Welcome to the new era", class: "comment" }],
  },
];

const typeCode = async () => {
  displayedCode.value = []; // Reset

  for (const line of codeBlockData) {
    const currentLine: CodeLine = { indent: line.indent, tokens: [] };
    displayedCode.value.push(currentLine);

    for (const token of line.tokens) {
      const currentToken: Token = { ...token, text: "" };
      currentLine.tokens.push(currentToken);

      for (const char of token.text) {
        currentToken.text += char;
        // Natural typing delay
        // await new Promise((r) => setTimeout(r, 30 + Math.random() * 40));
      }
    }
    // Pause at end of line
    await new Promise((r) => setTimeout(r, 150));
  }
};

onMounted(() => {
  typeText();
  // Start code typing after a short delay
  setTimeout(typeCode, 500);
});
</script>

<template>
  <section class="hero">
    <ParticleBackground />
    <!-- Liquid Shapes -->
    <div class="liquid-shape shape-1"></div>
    <div class="liquid-shape shape-2"></div>

    <div class="container hero-content">
      <div class="hero-text">
        <h1 class="hero-title">
          {{ t("hero.title") }}
          <div class="typing-wrapper">
            <span class="text-gradient">{{ typedText }}</span>
            <span class="cursor" v-if="isTyping">|</span>
          </div>
        </h1>
        <p class="hero-subtitle">
          {{ t("hero.subtitle") }}
        </p>
        <div class="hero-actions">
          <button class="btn btn-primary">{{ t("hero.getStarted") }}</button>
          <button class="btn btn-secondary glass-panel">
            {{ t("hero.documentation") }} <ArrowRight class="icon-right" />
          </button>
        </div>
      </div>
      <div class="hero-visual">
        <div class="card glass-panel">
          <div class="card-header">
            <div class="dot red"></div>
            <div class="dot yellow"></div>
            <div class="dot green"></div>
          </div>
          <div class="card-body">
            <div
              v-for="(line, index) in displayedCode"
              :key="index"
              class="code-line"
              :class="{ indent: line.indent }"
            >
              <span
                v-for="(token, tIndex) in line.tokens"
                :key="tIndex"
                :class="token.class"
                >{{ token.text }}</span
              >
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
.hero {
  min-height: 100vh;
  display: flex;
  align-items: center;
  padding-top: 80px;
  background: var(--bg-primary); /* Use primary bg, liquid shapes add color */
  position: relative;
  overflow: hidden;
}

.shape-1 {
  top: -10%;
  left: -10%;
  width: 600px;
  height: 600px;
  background: radial-gradient(circle, var(--accent-primary), transparent 70%);
  animation-duration: 20s;
}

.shape-2 {
  bottom: -10%;
  right: -5%;
  width: 500px;
  height: 500px;
  background: radial-gradient(circle, var(--accent-hover), transparent 70%);
  animation-duration: 15s;
  animation-direction: reverse;
}

.hero-content {
  display: grid;
  grid-template-columns: 1fr;
  gap: 4rem;
  align-items: center;
  position: relative;
  z-index: 1;
}

.hero-title {
  font-size: 3rem;
  font-weight: 800;
  line-height: 1.2;
  margin-bottom: 1.5rem;
  letter-spacing: -0.02em;
}

.typing-wrapper {
  display: block;
  min-height: 1.2em;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.hero-subtitle {
  font-size: 1.25rem;
  color: var(--text-secondary);
  margin-bottom: 2.5rem;
  max-width: 600px;
}

.hero-actions {
  display: flex;
  gap: 1rem;
  flex-wrap: wrap;
}

.btn-secondary {
  /* Inherits glass-panel properties */
  color: var(--text-primary);
  display: flex;
  align-items: center;
  gap: 0.5rem;
  border-radius: 9999px; /* Override glass-panel radius */
}

.btn-secondary:hover {
  background: rgba(255, 255, 255, 0.2); /* Slightly lighter on hover */
}

.icon-right {
  width: 18px;
  height: 18px;
}

.hero-visual {
  position: relative;
  display: flex;
  justify-content: center;
  perspective: 1000px;
}

.glass-panel.card {
  padding: 1.5rem;
  width: 100%;
  max-width: 450px;
  transform: rotateY(-5deg) rotateX(5deg);
  transition: transform 0.5s ease;
}

.glass-panel.card:hover {
  transform: rotateY(0) rotateX(0) scale(1.02);
}

.card-header {
  display: flex;
  gap: 0.5rem;
  margin-bottom: 1.5rem;
}

.dot {
  width: 12px;
  height: 12px;
  border-radius: 50%;
}

.red {
  background-color: #ef4444;
  box-shadow: 0 0 10px rgba(239, 68, 68, 0.5);
}
.yellow {
  background-color: #f59e0b;
  box-shadow: 0 0 10px rgba(245, 158, 11, 0.5);
}
.green {
  background-color: #22c55e;
  box-shadow: 0 0 10px rgba(34, 197, 94, 0.5);
}

.card-body {
  font-family: "Fira Code", monospace;
  font-size: 0.9rem;
  line-height: 1.6;
  min-height: 160px;
}

.code-line {
  margin-bottom: 0.25rem;
  min-height: 1.5em;
}

.indent {
  padding-left: 1.5rem;
}

.keyword {
  color: #c084fc;
  text-shadow: 0 0 5px rgba(192, 132, 252, 0.3);
}
.variable {
  color: #60a5fa;
  text-shadow: 0 0 5px rgba(96, 165, 250, 0.3);
}
.function {
  color: #f472b6;
  text-shadow: 0 0 5px rgba(244, 114, 182, 0.3);
}
.string {
  color: #34d399;
  text-shadow: 0 0 5px rgba(52, 211, 153, 0.3);
}
.property {
  color: #93c5fd;
}
.comment {
  color: #6b7280;
  font-style: italic;
}

.cursor {
  display: inline-block;
  margin-left: 2px;
  color: var(--text-primary);
  animation: blink 1s infinite;
  font-weight: 400;
}

@keyframes blink {
  0%,
  100% {
    opacity: 1;
  }
  50% {
    opacity: 0;
  }
}

@media (min-width: 768px) {
  .hero-content {
    grid-template-columns: 1fr 1fr;
  }
  .hero-title {
    font-size: 4rem;
  }
}
</style>
