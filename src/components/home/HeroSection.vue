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
  for (const line of codeBlockData) {
    const currentLine: CodeLine = { indent: line.indent, tokens: [] };
    displayedCode.value.push(currentLine);

    for (const token of line.tokens) {
      const currentToken: Token = { ...token, text: "" };
      currentLine.tokens.push(currentToken);

      for (const char of token.text) {
        currentToken.text += char;
        await new Promise((r) => setTimeout(r, 30));
      }
    }
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
          <button class="btn btn-secondary">
            {{ t("hero.documentation") }} <ArrowRight class="icon-right" />
          </button>
        </div>
      </div>
      <div class="hero-visual">
        <div class="orb"></div>
        <div class="card glass-card">
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
  background: var(--gradient-hero);
  position: relative;
  overflow: hidden;
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
  min-height: 1.2em; /* Reserve space for the line */
  white-space: nowrap; /* Prevent wrapping mid-typing */
  overflow: hidden; /* Hide overflow if it happens */
  text-overflow: ellipsis; /* Ellipsis if it really doesn't fit */
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
  background-color: transparent;
  border: 1px solid var(--border-color);
  color: var(--text-primary);
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

.btn-secondary:hover {
  background-color: var(--bg-secondary);
}

.icon-right {
  width: 18px;
  height: 18px;
}

.hero-visual {
  position: relative;
  display: flex;
  justify-content: center;
}

.orb {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 300px;
  height: 300px;
  background: radial-gradient(
    circle,
    var(--accent-primary) 0%,
    transparent 70%
  );
  opacity: 0.2;
  filter: blur(60px);
  z-index: -1;
  animation: pulse 4s infinite ease-in-out;
}

@keyframes pulse {
  0% {
    transform: translate(-50%, -50%) scale(1);
    opacity: 0.2;
  }
  50% {
    transform: translate(-50%, -50%) scale(1.2);
    opacity: 0.3;
  }
  100% {
    transform: translate(-50%, -50%) scale(1);
    opacity: 0.2;
  }
}

.glass-card {
  background: rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(16px);
  border: 1px solid rgba(255, 255, 255, 0.2);
  border-radius: 16px;
  padding: 1.5rem;
  width: 100%;
  max-width: 400px;
  box-shadow: 0 20px 40px rgba(0, 0, 0, 0.1);
}

.dark .glass-card {
  background: rgba(15, 23, 42, 0.6);
  border: 1px solid rgba(255, 255, 255, 0.1);
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
}
.yellow {
  background-color: #f59e0b;
}
.green {
  background-color: #22c55e;
}

.card-body {
  font-family: "Fira Code", monospace;
  font-size: 0.9rem;
  line-height: 1.6;
  min-height: 160px; /* Prevent layout jump */
}

.code-line {
  margin-bottom: 0.25rem;
  min-height: 1.5em; /* Maintain line height */
}

.indent {
  padding-left: 1.5rem;
}

.keyword {
  color: #c084fc;
}
.variable {
  color: #60a5fa;
}
.function {
  color: #f472b6;
}
.string {
  color: #34d399;
}
.property {
  color: #93c5fd;
}
.comment {
  color: #6b7280;
  margin-top: 0.5rem;
}

.cursor {
  display: inline-block;
  margin-left: 2px;
  -webkit-text-fill-color: var(--text-primary);
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
