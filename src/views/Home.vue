<script setup>
import { ArrowRight, Zap, Shield, Globe, Cpu } from "lucide-vue-next";
import ParticleBackground from "../components/ParticleBackground.vue";
import { ref, onMounted } from "vue";

// Headline Typing Logic
const typedText = ref("");
const fullText = "Artificial Intelligence";
const isTyping = ref(true);

const typeText = () => {
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

// Code Block Typing Logic
const displayedCode = ref([]);
const codeBlockData = [
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
    const currentLine = { indent: line.indent, tokens: [] };
    displayedCode.value.push(currentLine);

    for (const token of line.tokens) {
      const currentToken = { ...token, text: "" };
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

  // Scroll Animation Observer
  const observer = new IntersectionObserver(
    (entries) => {
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
  <div class="home">
    <!-- Hero Section -->
    <section class="hero">
      <ParticleBackground />
      <div class="container hero-content">
        <div class="hero-text">
          <h1 class="hero-title">
            Experience the Future of
            <div class="typing-wrapper">
              <span class="text-gradient">{{ typedText }}</span>
              <span class="cursor" v-if="isTyping">|</span>
            </div>
          </h1>
          <p class="hero-subtitle">
            Unlock limitless possibilities with our advanced AI model. Built for
            speed, accuracy, and creativity.
          </p>
          <div class="hero-actions">
            <button class="btn btn-primary">Get Started</button>
            <button class="btn btn-secondary">
              View Documentation <ArrowRight class="icon-right" />
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

    <!-- Features Section -->
    <section id="features" class="features">
      <div class="container">
        <div class="section-header">
          <h2 class="section-title">
            Why Choose <span class="text-gradient">AI Nexus</span>?
          </h2>
          <p class="section-subtitle">
            Powered by state-of-the-art algorithms designed to scale with your
            needs.
          </p>
        </div>

        <div class="features-grid">
          <div class="feature-card scroll-animate">
            <div class="feature-icon">
              <Zap />
            </div>
            <h3>Lightning Fast</h3>
            <p>
              Process millions of data points in milliseconds with our optimized
              engine.
            </p>
          </div>
          <div
            class="feature-card scroll-animate"
            style="transition-delay: 100ms"
          >
            <div class="feature-icon">
              <Shield />
            </div>
            <h3>Secure by Design</h3>
            <p>
              Enterprise-grade security ensures your data remains private and
              protected.
            </p>
          </div>
          <div
            class="feature-card scroll-animate"
            style="transition-delay: 200ms"
          >
            <div class="feature-icon">
              <Globe />
            </div>
            <h3>Global Scale</h3>
            <p>
              Deployed across 20+ regions worldwide for low-latency access
              anywhere.
            </p>
          </div>
          <div
            class="feature-card scroll-animate"
            style="transition-delay: 300ms"
          >
            <div class="feature-icon">
              <Cpu />
            </div>
            <h3>Advanced Reasoning</h3>
            <p>
              Capable of complex problem solving and nuanced understanding of
              context.
            </p>
          </div>
        </div>
      </div>
    </section>

    <!-- CTA Section -->
    <section class="cta">
      <div class="container">
        <div class="cta-card">
          <h2>Ready to transform your workflow?</h2>
          <p>Join thousands of developers building the future with AI Nexus.</p>
          <button class="btn btn-primary btn-lg">Start Building Now</button>
        </div>
      </div>
    </section>
  </div>
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

/* Features Section */
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

/* CTA Section */
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

@media (min-width: 768px) {
  .hero-content {
    grid-template-columns: 1fr 1fr;
  }

  .hero-title {
    font-size: 4rem;
  }

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
