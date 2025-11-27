简体中文 | [English](./README.md)

# AI Nexus - 官方网站

一个基于 Vue 3 和 Vite 构建的现代化、未来感十足的 AI 平台官方网站。本项目采用受顶级科技公司启发的高端设计，包含交互式视觉特效和完全响应式的布局。

## ✨ 功能特性

- **现代美学**：采用玻璃拟态、渐变色和“科技感”十足的流畅设计。
- **交互式视觉效果**：
  - **粒子背景**：Hero 区域的动态连接节点网络。
  - **打字动画**：主标题的引人注目的打字机效果。
  - **滚动动画**：随着用户浏览，内容会渐显并滑入。
  - **悬停效果**：卡片上的 3D 上浮和发光效果。
- **主题系统**：无缝切换明亮/暗黑模式，并持久化保存状态。
- **响应式设计**：针对桌面端、平板和移动设备进行了全面优化。
- **自定义滚动条**：与主题完美融合的精致滚动条。

## 🛠️ 技术栈

- **框架**: [Vue 3](https://vuejs.org/) (Composition API)
- **构建工具**: [Vite](https://vitejs.dev/)
- **路由**: [Vue Router](https://router.vuejs.org/)
- **图标**: [Lucide Vue Next](https://lucide.dev/guide/packages/lucide-vue-next)
- **样式**: Vanilla CSS (原生 CSS) 搭配 CSS 变量

## 🚀 快速开始

### 前置要求

- Node.js (v16.0.0 或更高版本)
- npm 或 yarn

### 安装

1.  克隆仓库：

    ```bash
    git clone <repository-url>
    cd vue3-app
    ```

2.  安装依赖：
    ```bash
    yarn install
    # 或
    npm install
    ```

### 开发

启动开发服务器：

```bash
yarn dev
# 或
npm run dev
```

访问 `http://localhost:5173` 查看网站。

### 构建

构建生产版本：

```bash
yarn build
# 或
npm run build
```

## 📂 项目结构

```
src/
├── components/        # 可复用的 UI 组件
│   ├── Footer.vue
│   ├── NavBar.vue
│   ├── ParticleBackground.vue
│   └── ThemeToggle.vue
├── router/           # 路由定义
│   └── index.js
├── views/            # 页面视图
│   └── Home.vue
├── App.vue           # 根组件
├── main.js           # 入口文件
└── style.css         # 全局样式和变量
```

## 📄 许可证

本项目采用 MIT 许可证。
