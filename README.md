[简体中文](./README.zh-CN.md) | English

# AI Nexus - Official Website

A modern, futuristic official website for an AI platform, built with Vue 3 and Vite. This project features a high-end design inspired by top-tier tech companies, complete with interactive visual effects and a fully responsive layout.

## ✨ Features

- **Modern Aesthetics**: sleek design with glassmorphism, gradients, and a "tech" feel.
- **Interactive Visuals**:
  - **Particle Background**: A dynamic network of connecting nodes.
  - **Typing Animation**: Engaging typewriter effect for the main headline.
  - **Scroll Animations**: Content fades and slides in as you explore.
  - **Hover Effects**: 3D-like lift and glow effects on cards.
- **Theme System**: Seamless light and dark mode toggling with persistent state.
- **Responsive Design**: Fully optimized for desktops, tablets, and mobile devices.
- **Custom Scrollbar**: A polished scrollbar that integrates perfectly with the theme.

## 🛠️ Tech Stack

- **Framework**: [Vue 3](https://vuejs.org/) (Composition API)
- **Build Tool**: [Vite](https://vitejs.dev/)
- **Routing**: [Vue Router](https://router.vuejs.org/)
- **Icons**: [Lucide Vue Next](https://lucide.dev/guide/packages/lucide-vue-next)
- **Styling**: Vanilla CSS with CSS Variables

## 🚀 Getting Started

### Prerequisites

- Node.js (v16.0.0 or higher)
- npm or yarn

### Installation

1.  Clone the repository:

    ```bash
    git clone <repository-url>
    cd vue3-app
    ```

2.  Install dependencies:
    ```bash
    yarn install
    # or
    npm install
    ```

### Development

Start the development server:

```bash
yarn dev
# or
npm run dev
```

Visit `http://localhost:5173` to view the site.

### Build

Build for production:

```bash
yarn build
# or
npm run build
```

## 📂 Project Structure

```
src/
├── components/        # Reusable UI components
│   ├── Footer.vue
│   ├── NavBar.vue
│   ├── ParticleBackground.vue
│   └── ThemeToggle.vue
├── router/           # Route definitions
│   └── index.js
├── views/            # Page views
│   └── Home.vue
├── App.vue           # Root component
├── main.js           # Entry point
└── style.css         # Global styles and variables
```

## 📄 License

This project is licensed under the MIT License.
