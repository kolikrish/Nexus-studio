# Julien Pianetti Portfolio — Vue 3 & Vite

A pixel-perfect, highly responsive interactive portfolio website inspired by [Julien Pianetti](https://julienpianetti.com), built with **Vue 3**, **Vite**, **Tailwind CSS v4**, and **SCSS**.

![Vue 3](https://img.shields.io/badge/Vue.js-3.5-4FC08D?style=flat&logo=vuedotjs&logoColor=white)
![Vite](https://img.shields.io/badge/Vite-8.1-646CFF?style=flat&logo=vite&logoColor=white)
![Tailwind CSS](https://img.shields.io/badge/Tailwind_CSS-v4-38B2AC?style=flat&logo=tailwindcss&logoColor=white)

---

## 🌟 GitHub Repository Info

- **Repository Name**: `julien-pianetti-vue`
- **Short Description**:
  > A modern, interactive portfolio website replica of Julien Pianetti built with Vue 3, Vite, Tailwind CSS, and SCSS. Features dynamic video cards with hover/scroll playback, variable font proximity animations, cursor-tracking brand previews, and auto-scrolling motion carousels.

---

## ✨ Features

- 🎯 **Hero Section**: Includes a custom **Variable Font Proximity** effect on the main headline (*Selected works\**), where letter thickness dynamically shifts based on cursor proximity, paired with an embedded HTML5 WebM video player.
- 📹 **Selected Works Grid**: Responsive 3-column grid displaying project cards (`Pullin`, `Vision(s)`, `Voyages E.Leclerc`, etc.). Videos load paused as static thumbnails and play automatically when hovered or scrolled onto.
- 💼 **Experience Section**: Clean 3-column layout breaking down Digital Design, Webflow, and Motion Design services, accompanied by custom inline SVG icons.
- 🏷️ **Interactive Brands Section**: Full typography list of 37 brand partners. Hovering over a brand highlights that word while dimming all other brands to light gray, simultaneously opening a cursor-following floating image preview.
- 🎠 **Infinite Auto-Scrolling Carousel**: Drag-to-scroll & auto-scrolling horizontal marquee displaying video cards from `src/assets/works/`.
- ⚪ **Minimalist White Theme Footer**: Features a massive *"Let's Collaborate!"* title, quick menu navigation, active work status badge, and smooth "BACK TO TOP" button.

---

## 🛠️ Tech Stack

- **Framework**: [Vue 3](https://vuejs.org/) (`<script setup>` SFCs)
- **Build Tool**: [Vite](https://vite.dev/)
- **Styling**: [Tailwind CSS v4](https://tailwindcss.com/) & [Sass/SCSS](https://sass-lang.com/)
- **Typography**: Google Fonts (*Inter*, *Roboto Flex*, *Poppins*)
- **Media**: WebM & MP4 Video integration

---

## 🚀 Getting Started

### Prerequisites

Ensure you have **Node.js** (v18+) installed.

### Installation

1. Clone the repository:
   ```bash
   git clone https://github.com/your-username/julien-pianetti-vue.git
   cd julien-pianetti-vue
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
julien-pianetti-vue/
├── public/                # Static assets & icons
├── src/
│   ├── assets/            # Video files (Hero-video.webm & works/*.mp4), fonts
│   ├── components/
│   │   ├── Navbar.vue             # Header navigation bar
│   │   ├── Hero.vue               # Hero section with proximity headline & video
│   │   ├── WorksSection.vue       # Works grid with hover/scroll video playback
│   │   ├── ExperienceSection.vue  # 3-column service grid with SVGs
│   │   ├── BrandsSection.vue      # Interactive brand list with cursor preview
│   │   ├── CarouselSection.vue    # Horizontal auto-scrolling video carousel
│   │   ├── FooterSection.vue      # Minimalist white theme footer
│   │   └── effect/
│   │       └── VariableProximity.vue # Proximity font weight animation component
│   ├── App.vue            # Main application layout
│   ├── main.js            # Vue app initialization
│   └── style.css          # Global CSS & Tailwind imports
├── index.html             # HTML template & Google Font links
├── vite.config.js         # Vite configuration
└── package.json           # Project dependencies & scripts
```

---

## 📄 License

This project is created for educational and portfolio demonstration purposes. Design inspiration belongs to [Julien Pianetti](https://julienpianetti.com).
