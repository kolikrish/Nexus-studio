# Nexus Product Studio — Interactive Portfolio

A pixel-perfect, highly responsive interactive portfolio website built with **Vue 3**, **Vite**, **Three.js**, **GSAP**, **Lenis**, **SplitType**, **Tailwind CSS v4**, and **SCSS**.

![Vue 3](https://img.shields.io/badge/Vue.js-3.5-4FC08D?style=flat&logo=vuedotjs&logoColor=white)
![Vite](https://img.shields.io/badge/Vite-8.1-646CFF?style=flat&logo=vite&logoColor=white)
![Three.js](https://img.shields.io/badge/Three.js-WebGL-black?style=flat&logo=three.js&logoColor=white)
![GSAP](https://img.shields.io/badge/GSAP-Animation-green?style=flat&logo=greensock&logoColor=white)
![Tailwind CSS](https://img.shields.io/badge/Tailwind_CSS-v4-38B2AC?style=flat&logo=tailwindcss&logoColor=white)

---

## 🌟 Overview

**Nexus Product Studio** is a modern digital interface and product design studio website. It features interactive WebGL shader effects, custom mouse follower interactions, kinetic typography, and smooth inertia scrolling.

---

## ✨ Key Features

- 🎯 **GSAP Entrance Animations**:
  - Hero section video player animates smoothly from the **right side**.
  - Main headline (*Selected works\**) animates in from the **left side**.
  - Paragraph descriptions across Hero, Experience, and Brands sections reveal **word-by-word from the top** on scroll.
- 🖱️ **Custom Mouse Follower**:
  - Smooth lerp cursor trailing with dynamic scaling (`MouseFollower.vue`).
  - Scales up with **`VIEW`** overlay when hovering over project video cards.
  - Scales up with high-contrast **`mix-blend-difference`** over major typography.
  - Scales up with **`DRAG`** indicator over horizontal carousels and **`NEXT`** over WebGL sliders.
  - Shrinks on click (`scale-85`) for tactile feedback.
- 🌊 **3D WebGL Radial Ripple Slider**:
  - Custom Three.js shader plane (`WebGLSlider.vue`) featuring radial water displacement, specular light wave highlights, `object-fit: cover` aspect ratio preserving UV coordinates, and smoothstep crossfading between slides.
  - Kinetic typography transitions driven by **SplitType** and **GSAP**.
- 📜 **Lenis Smooth Inertia Scroll**:
  - Enhanced momentum smooth scrolling tuned with a `2.2s` duration for a silky, floating scroll experience.
- 📹 **Selected Works Grid**:
  - Responsive 3-column grid displaying project video cards with auto-play on scroll and hover.
- 🏷️ **Interactive Brands Section**:
  - Interactive brand list featuring cursor-following floating image previews and active item highlighting.
- 🔤 **Interactive Footer Typography**:
  - *"Let's Collaborate!"* split letter-by-letter with a smooth `1.5x` scale effect on hover.

---

## 🛠️ Tech Stack

- **Framework**: [Vue 3](https://vuejs.org/) (`<script setup>` SFCs)
- **Build Tool**: [Vite](https://vite.dev/)
- **3D / WebGL**: [Three.js](https://threejs.org/) & Custom GLSL Shaders
- **Animations**: [GSAP (GreenSock)](https://greensock.com/gsap/) & [SplitType](https://github.com/lucasyomi/split-type)
- **Smooth Scroll**: [Lenis](https://lenis.darkroom.engineering/)
- **Styling**: [Tailwind CSS v4](https://tailwindcss.com/) & [Sass/SCSS](https://sass-lang.com/)
- **Typography**: Google Fonts (*Inter*, *Roboto Flex*, *Poppins*)

---

## 🚀 Getting Started

### Prerequisites

Ensure you have **Node.js** (v18+) installed.

### Installation

1. Clone the repository:
   ```bash
   git clone https://github.com/your-username/nexus-product-studio.git
   cd nexus-product-studio
   ```

2. Install dependencies:
   ```bash
   npm install
   ```

3. Run the development server:
   ```bash
   npm run dev
   ```

4. Build for production:
   ```bash
   npm run build
   ```

---

## 📂 Project Structure

```text
nexus-product-studio/
├── public/
│   └── assets/
│       └── slider/        # WebGL slider textures
├── src/
│   ├── assets/            # Videos (Hero & works), fonts
│   ├── components/
│   │   ├── MouseFollower.vue      # Smooth lerp cursor with dynamic hover scaling
│   │   ├── Navbar.vue             # Header navigation bar
│   │   ├── Hero.vue               # Hero section with GSAP entrance animations
│   │   ├── WorksSection.vue       # Works grid with auto-play video cards
│   │   ├── ExperienceSection.vue  # Service breakdown with word reveal animation
│   │   ├── BrandsSection.vue      # Brand list with floating cursor preview
│   │   ├── CarouselSection.vue    # Horizontal auto-scrolling video carousel
│   │   ├── WebGLSlider.vue        # 3D WebGL Radial Ripple Slider component
│   │   ├── FooterSection.vue      # Footer with interactive letter-scaling title
│   │   └── effect/
│   │       └── VariableProximity.vue # Proximity font weight animation component
│   ├── App.vue            # Main application layout & Lenis smooth scroll setup
│   ├── main.js            # Vue app entry point
│   └── style.css          # Global styles & Tailwind CSS imports
├── index.html             # HTML template & font preloads
├── vite.config.js         # Vite configuration
└── package.json           # Project dependencies & scripts
```

---

## 📄 License

This project is created for educational and portfolio demonstration purposes. All rights reserved by **Nexus Product Studio**.

