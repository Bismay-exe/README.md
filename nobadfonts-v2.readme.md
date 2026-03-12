---

# NoBadFonts

<p align="center">
  <img src="https://placehold.co/800x200?text=NoBadFonts" alt="NoBadFonts Banner"/>
</p>

<p align="center">
  A curated typography platform for designers and developers.
</p>

<p align="center">
  Discover high-quality fonts • Test typography in real UI contexts • Optimize fonts automatically for the web
</p>

<p align="center">
  <a href="https://nobadfonts.in"><strong>Live Demo</strong></a>
</p>

---

# Overview

Typography is one of the most important aspects of digital design, yet most font platforms are built like **archives instead of tools**.

They suffer from the same problems:

* thousands of low-quality fonts
* poor preview systems
* no realistic design context
* inefficient font formats
* poor developer workflow

Designers end up downloading fonts blindly and testing them manually in their projects.

**NoBadFonts was built to fix this problem.**

Instead of being another massive font database, the platform focuses on:

* **curated typography**
* **real-world preview environments**
* **automatic font optimization**
* **modern developer workflows**

---

# Live Demo

Explore the platform here:

[https://nobadfonts.in](https://nobadfonts.in)

---

# Preview

Dashboard Interface

![Dashboard](https://placehold.co/1200x600)

Font Testing Interface

![Font Tester](https://placehold.co/1200x600)

Font Pairing Tool

![Font Pairing](https://placehold.co/1200x600)

---

# Key Features

## Context-Aware Font Testing

Most font websites show a single preview sentence like:

```
The quick brown fox jumps over the lazy dog
```

This tells you almost nothing about how a font behaves in real interfaces.

NoBadFonts allows fonts to be previewed inside realistic design environments such as:

* UI dashboards
* article layouts
* code editors
* tables and forms
* headlines and body text

This allows designers to evaluate typography **in real usage scenarios**.

---

## Curated Font Library

The platform avoids the typical *“10,000 mediocre fonts”* problem.

Instead, fonts are curated and categorized based on:

* style
* readability
* design use cases
* pairing compatibility

This dramatically improves discovery.

---

## Automatic Font Optimization

Uploading raw fonts usually results in large file sizes that slow down websites.

NoBadFonts automatically converts fonts to **WOFF2**, the most efficient web font format.

Supported uploads:

```
.ttf
.otf
.woff
.woff2
```

When `.ttf` or `.otf` fonts are uploaded:

1. The browser converts them to `.woff2`
2. The optimized version is stored
3. File size is reduced dramatically

Benefits:

* faster websites
* smaller bandwidth usage
* improved performance

---

## Advanced Font Upload Pipeline

The upload system enforces strict quality rules before fonts are published.

Pipeline steps:

1. file validation
2. metadata parsing
3. variant detection
4. WOFF2 optimization
5. storage organization
6. database indexing

Example storage structure:

```
{userId}/{fontSlug}/variants/{variantName}/{filename}
```

This prevents collisions and keeps the system scalable.

---

## Font Pairing Explorer

Typography rarely exists in isolation.

The pairing explorer allows designers to experiment with:

* heading + body combinations
* serif + sans pairings
* editorial typography systems

This helps designers build stronger type hierarchies.

---

## Mobile Application Support

The project is designed to run both as a web application and a native mobile app using **Capacitor**.

Advantages:

* shared web + mobile codebase
* native plugins
* optimized touch interactions

Supported platforms:

* Web
* Android
* iOS (planned)

---

# Tech Stack

Frontend

* React
* TypeScript
* Vite
* Tailwind CSS

Animation

* GSAP
* Framer Motion
* Lenis Smooth Scroll

Backend

* Supabase
* PostgreSQL
* Row Level Security
* Supabase Storage

Infrastructure

* Vercel
* GitHub Actions
* Capacitor

---

# System Architecture

The platform follows a **modern serverless architecture**.

Frontend responsibilities:

* UI rendering
* font preview engine
* font file processing
* client-side WOFF2 conversion

Backend responsibilities:

* authentication
* database storage
* file storage
* access permissions

---

# Database Design

### profiles

| column     | type | description     |
| ---------- | ---- | --------------- |
| id         | uuid | user identifier |
| username   | text | display name    |
| role       | enum | member or admin |
| avatar_url | text | profile image   |

---

### fonts

| column       | type    | description         |
| ------------ | ------- | ------------------- |
| id           | uuid    | primary key         |
| slug         | text    | URL identifier      |
| name         | text    | font name           |
| user_id      | uuid    | owner               |
| tags         | text[]  | classification tags |
| is_published | boolean | visibility status   |

---

### font_variants

| column          | type   | description         |
| --------------- | ------ | ------------------- |
| id              | uuid   | variant identifier  |
| font_id         | uuid   | parent font         |
| variant_name    | text   | e.g. Bold Italic    |
| file_size_woff2 | bigint | optimized file size |
| woff2_url       | text   | storage link        |

---

# Getting Started

## Prerequisites

Before running the project locally you need:

* Node.js ≥ 20
* npm or pnpm
* a Supabase project

---

# Installation

Clone the repository:

```bash
git clone https://github.com/Bismay-exe/nobadfonts.git
```

Install dependencies:

```bash
npm install
```

Start development server:

```bash
npm run dev
```

Open in browser:

```
http://localhost:5173
```

---

# Environment Variables

Create a `.env` file in the root directory.

```
VITE_SUPABASE_URL=your_supabase_url
VITE_SUPABASE_ANON_KEY=your_supabase_anon_key
```

---

# Mobile Build

Sync Capacitor project:

```
npx cap sync
```

Open Android project:

```
npx cap open android
```

---

# Project Structure

```
nobadfonts
│
├── src
│   ├── components
│   ├── pages
│   ├── hooks
│   ├── contexts
│   ├── utils
│   ├── workers
│   └── types
│
├── supabase
├── android
├── scripts
└── nobadfonts-cli
```

---

# Performance Optimizations

The platform includes several performance improvements:

* automatic WOFF2 compression
* lazy-loaded font previews
* optimized React rendering
* minimized network payloads
* client-side font processing

---

# Development Roadmap

Future improvements planned for the platform:

* AI powered font pairing
* Figma plugin
* advanced typography search
* design system export tools
* typography analytics
* community curated collections

---

# Contributing

Contributions are welcome.

1. Fork the repository
2. Create a feature branch
3. Commit changes
4. Push branch
5. Open a pull request

---

# License

MIT License

---

# Author

Bismay

GitHub
[https://github.com/Bismay-exe](https://github.com/Bismay-exe)

---

⭐ If you find this project useful, consider starring the repository.

---
