# NoBadFonts

## Demo

Check out the live demo:
**[https://nobadfonts.in](https://nobadfonts.in)**

---

# NoBadFonts

I created this project to build a **high-quality typography discovery platform** for designers and developers.

Most font websites overwhelm users with thousands of low-quality fonts and poor preview tools.
**NoBadFonts solves this problem by focusing on curated typography and realistic font testing environments.**

Instead of generic previews like *“The quick brown fox”*, this platform allows fonts to be tested in **real UI, editorial, and coding contexts**.

## Key Focus Areas

* **Primary goal I achieved:** A modern typography platform with realistic preview environments and optimized font delivery.
* **Unique value I provide:** Context-aware font testing, automatic WOFF2 optimization, and a curated library instead of a massive unfiltered catalog.

---

# NoBadFonts

NoBadFonts is a **curated library of typefaces built for designers who care about how things feel, not just how they look.**

It combines:

* curated fonts
* real-world preview environments
* automated font optimization
* a full web + mobile ecosystem

---

# Table of Contents

* Project Overview
* Features
* Tech Stack
* Installation
* Usage
* Architecture
* Project Structure
* Roadmap
* Contributing
* License

---

# Project Overview

Typography is one of the most important parts of digital design, but existing font platforms have serious problems:

* Too many low-quality fonts
* Poor preview systems
* No real design context
* Inefficient font formats
* No workflow integration

**NoBadFonts fixes this by focusing on quality, context, and performance.**

The platform allows designers to:

* discover curated fonts
* preview them in real UI environments
* upload fonts
* manage variants
* generate optimized WOFF2 assets
* test font pairings

---

# Features I Implemented

## Context-Aware Font Tester

Traditional font preview tools show a single sentence.

I built a **context aware font tester** that simulates real design environments.

Users can preview fonts inside:

* UI dashboards
* editorial layouts
* code editors
* tables and forms

This helps designers understand how fonts behave in real interfaces.

---

## Advanced Font Upload Pipeline

The upload system automatically processes fonts before publishing.

The pipeline performs:

1. File validation
2. Metadata enforcement
3. Font fingerprinting
4. Variant detection
5. WOFF2 optimization
6. Storage organization

Example storage structure:

```
{userId}/{fontSlug}/variants/{variantName}/{filename}
```

This prevents collisions and keeps the system scalable.

---

## Automatic WOFF2 Optimization

Large font files slow down websites.

To solve this I integrated **Google's WOFF2 compression via WebAssembly**.

If a user uploads `.ttf` or `.otf`:

1. The browser converts it to `.woff2`
2. The optimized version is uploaded
3. File size is drastically reduced

Benefits:

* faster websites
* lower bandwidth
* better performance

---

## Algorithmic Font Description Generator

Most font websites use generic descriptions.

I built an **algorithmic description generator** that analyzes metadata tags and produces human-readable descriptions.

Example:

Input tags:

```
["sans", "geometric", "modern", "bold"]
```

Generated description:

```
A geometric sans-serif with a modern character.
Its bold weight makes it ideal for headlines and branding.
```

---

## Font Pairing System

Typography rarely uses one font.

The platform includes a **font pairing explorer** that allows designers to experiment with combinations.

---

## Native Mobile Support

The project uses **Capacitor** to create a native mobile ecosystem.

Benefits:

* shared web + mobile codebase
* native plugins
* touch optimized interactions

Supported platforms:

* Web
* Android
* iOS (future)

---

# My Tech Stack

## Frontend

* React 19
* TypeScript
* Vite
* Tailwind CSS v4
* Framer Motion
* GSAP
* Lenis smooth scrolling

---

## Backend

* Supabase
* PostgreSQL
* Row Level Security
* Supabase Storage
* Edge Functions (planned)

---

## Infrastructure

* Vercel deployment
* GitHub Actions
* Capacitor mobile builds

---

# Screenshots

Dashboard Preview

![Dashboard](https://placehold.co/800x400)

Font Tester

![Font Tester](https://placehold.co/800x400)

Font Pairing Tool

![Font Pairing](https://placehold.co/800x400)

---

# Live Demo

Try the platform here:

[https://nobadfonts.in](https://nobadfonts.in)

---

# Getting Started

## Prerequisites

* Node.js 20+
* npm or pnpm
* Supabase project

---

# Installation

```bash
git clone https://github.com/Bismay-exe/nobadfonts.git
cd nobadfonts

npm install
```

---

# Environment Variables

Create a `.env` file:

```
VITE_SUPABASE_URL=your_supabase_url
VITE_SUPABASE_ANON_KEY=your_supabase_key
```

---

# Run Development Server

```
npm run dev
```

Open:

```
http://localhost:5173
```

---

# Build for Production

```
npm run build
```

---

# Mobile Build (Capacitor)

```
npx cap sync
npx cap open android
```

---

# Project Architecture

Frontend handles:

* UI
* previews
* font conversion
* upload pipeline

Backend handles:

* authentication
* database
* storage
* permissions

---

# Database Schema

### profiles

| Column     | Type | Description    |
| ---------- | ---- | -------------- |
| id         | uuid | User ID        |
| username   | text | Display name   |
| role       | enum | member / admin |
| avatar_url | text | Profile image  |

---

### fonts

| Column       | Type   | Description    |
| ------------ | ------ | -------------- |
| id           | uuid   | Primary key    |
| slug         | text   | URL identifier |
| name         | text   | Font name      |
| user_id      | uuid   | Owner          |
| tags         | text[] | Search tags    |
| is_published | bool   | Visibility     |

---

### font_variants

| Column          | Type   | Description      |
| --------------- | ------ | ---------------- |
| id              | uuid   | Variant ID       |
| font_id         | uuid   | Font reference   |
| variant_name    | text   | e.g. Bold Italic |
| file_size_woff2 | bigint | File size        |
| woff2_url       | text   | CDN link         |

---

# Project Structure

```
nobadfonts
│
├── src
│   ├── components
│   ├── contexts
│   ├── hooks
│   ├── pages
│   ├── utils
│   ├── workers
│   └── types
│
├── supabase
├── android
├── scripts
├── nobadfonts-cli
```

---

# CLI Tool

The project also includes a **CLI companion tool**.

Install:

```
npm install nobadfonts-cli
```

Example:

```
npx nobadfonts-cli add helvetica
```

---

# My Optimizations

I improved performance by:

* converting fonts to WOFF2 automatically
* reducing network bandwidth
* lazy loading font previews
* optimizing rendering using React 19 features

---

# Known Issues

* Some rare font files fail WOFF2 conversion
* Mobile gesture interactions still being improved
* Font pairing recommendation system still basic

---

# Lessons I Learned

Building this project taught me:

**Architecture**

How to design a scalable full-stack application using React + Supabase.

**Performance**

How to optimize asset delivery using WOFF2 and browser-side processing.

**Product Thinking**

Why curated content is more valuable than large unfiltered databases.

---

# Development Roadmap

Future plans:

* AI font pairing recommendations
* Figma plugin
* advanced typography analytics
* font licensing marketplace
* community curated collections

---

# Contributing

Contributions are welcome.

Steps:

1. Fork repository
2. Create feature branch
3. Commit changes
4. Push branch
5. Open pull request

---

# License

MIT License

---

# Contact

Maintainer: **Bismay**
GitHub:
[https://github.com/Bismay-exe/nobadfonts](https://github.com/Bismay-exe/nobadfonts)

---

# Acknowledgements

Thanks to the open source community behind:

* React
* Vite
* Supabase
* TailwindCSS
* WOFF2

---

If you like this project, please **⭐ star the repository**.

---

## Brutally Honest Advice

Your **actual project is impressive**, but you are **bad at presenting it**.

The engineering here (WASM font conversion, upload pipeline, context previews) is **far above most portfolio projects**, yet your repo doesn't communicate that.

What you should do next:

1. Add **GIF demos**
2. Add **real screenshots**
3. Write **a landing README hero section**
4. Add **architecture diagrams**

Otherwise people will think it's just another font website.

---

If you want, I can also write a **much better README that looks like a top GitHub project** (the kind that gets stars fast).
