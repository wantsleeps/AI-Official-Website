<script setup>
import { Sparkles, Github, Twitter, Linkedin } from "lucide-vue-next";
</script>

<template>
  <footer class="footer">
    <div class="container">
      <div class="footer-content">
        <div class="footer-brand">
          <div class="logo">
            <Sparkles class="logo-icon" />
            <span class="logo-text">AI Nexus</span>
          </div>
          <p class="footer-desc">
            Empowering the future with advanced artificial intelligence
            solutions.
          </p>
        </div>

        <div class="footer-links">
          <div class="link-group">
            <h3>Product</h3>
            <a href="#">Features</a>
            <a href="#">Pricing</a>
            <a href="#">API</a>
          </div>
          <div class="link-group">
            <h3>Company</h3>
            <a href="#">About</a>
            <a href="#">Blog</a>
            <a href="#">Careers</a>
          </div>
          <div class="link-group">
            <h3>Legal</h3>
            <a href="#">Privacy</a>
            <a href="#">Terms</a>
            <a href="#">Security</a>
          </div>
        </div>
      </div>

      <div class="footer-bottom">
        <p>&copy; 2024 AI Nexus. All rights reserved.</p>
        <div class="social-links">
          <a href="#" aria-label="Github"><Github /></a>
          <a href="#" aria-label="Twitter"><Twitter /></a>
          <a href="#" aria-label="LinkedIn"><Linkedin /></a>
        </div>
      </div>
    </div>
  </footer>
</template>

<style scoped>
.footer {
  background: var(--glass-bg);
  backdrop-filter: var(--glass-blur);
  -webkit-backdrop-filter: var(--glass-blur); /* Safari support */
  border-top: 1px solid var(--glass-border);
  box-shadow: var(--glass-shadow); /* Added shadow for depth */
  padding: 4rem 0 2rem;
  margin-top: auto;
  position: relative; /* Context for glass effect */
}

.footer-content {
  display: grid;
  grid-template-columns: 1fr;
  gap: 3rem;
  margin-bottom: 3rem;
}

.footer-brand {
  max-width: 300px;
}

.logo {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  font-weight: 700;
  font-size: 1.5rem;
  color: var(--text-primary);
  margin-bottom: 1rem;
}

.logo-icon {
  color: var(--accent-primary);
}

.footer-desc {
  color: var(--text-secondary);
  line-height: 1.6;
}

.footer-links {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 2rem;
}

.link-group h3 {
  font-size: 1rem;
  font-weight: 600;
  margin-bottom: 1rem;
  color: var(--text-primary);
}

.link-group a {
  display: block;
  color: var(--text-secondary);
  margin-bottom: 0.75rem;
  transition: color 0.2s ease;
}

.link-group a:hover {
  color: var(--accent-primary);
}

.footer-bottom {
  padding-top: 2rem;
  border-top: 1px solid var(--border-color);
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 1.5rem;
  color: var(--text-secondary);
}

.social-links {
  display: flex;
  gap: 1.5rem;
}

.social-links a {
  color: var(--text-secondary);
  transition: color 0.2s ease;
}

.social-links a:hover {
  color: var(--accent-primary);
}

@media (min-width: 768px) {
  .footer-content {
    grid-template-columns: 1fr 2fr;
  }

  .footer-links {
    grid-template-columns: repeat(3, 1fr);
  }

  .footer-bottom {
    flex-direction: row;
    justify-content: space-between;
  }
}
</style>
