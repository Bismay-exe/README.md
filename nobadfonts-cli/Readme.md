# NoBadFonts CLI

### Good fonts. No friction. Just ship.

[![npm version](https://img.shields.io/npm/v/nobadfonts?style=flat-square\&color=white\&label=npm)](https://www.npmjs.com/package/nobadfonts)
[![npm downloads](https://img.shields.io/npm/dm/nobadfonts?style=flat-square\&color=white\&label=downloads)](https://www.npmjs.com/package/nobadfonts)
[![License](https://img.shields.io/npm/l/nobadfonts?style=flat-square\&color=white)](./LICENSE)

**Find it. Pull it. Build with it.**

NoBadFonts CLI brings the NoBadFonts font catalog straight to your terminal. Search for typefaces, inspect their details, and bring the ones you love into your project — without the tab-hopping.

```bash
npx nobadfonts add <font>
```

No font-hunting rabbit holes. Just type.

---

## ✳ The type toolkit

| Command    | What it does                               |
| ---------- | ------------------------------------------ |
| `add`      | Bring a font into your project             |
| `download` | Get font files without project integration |
| `select`   | Interactively choose font variants         |
| `search`   | Find fonts by keyword                      |
| `info`     | Inspect a font's details                   |
| `list`     | Explore the catalog                        |

## ⚡ Get started

No global install. No ceremony.

```bash
npx nobadfonts add <font>
```

Want the CLI available everywhere?

```bash
npm install -g nobadfonts
```

Then run it directly:

```bash
nobadfonts add <font>
```

## 🎛 Your font. Your weights.

Need just one weight? Or a specific combination?

Choose the variants your project actually needs.

**Add a single variant**

```bash
npx nobadfonts add <font> --variant bold
```

**Download a single variant**

```bash
npx nobadfonts download <font> --variant regular
```

**Pick multiple variants**

```bash
npx nobadfonts add <font> -v regular -v bold
```

**Use a comma-separated list**

```bash
npx nobadfonts download <font> --variants regular,bold,italic
```

**Not sure which variants are available?**

```bash
npx nobadfonts add <font> --select
```

Interactive selection lets you choose from the variants available for that font.

> No variant flags? The CLI uses all available variants by default.

## 🔎 Find your next typeface

Search the catalog by keyword:

```bash
npx nobadfonts search <keyword>
```

Explore the catalog:

```bash
npx nobadfonts list
```

Found something interesting? Get the details:

```bash
npx nobadfonts info <font>
```

## 🧰 The command shelf

```bash
# Add a font to your project
npx nobadfonts add <font>

# Download font files
npx nobadfonts download <font>

# Add selected variants
npx nobadfonts add <font> -v regular -v bold

# Download selected variants
npx nobadfonts download <font> -v regular -v bold

# Open the interactive variant selector
npx nobadfonts add <font> --select

# Inspect font details
npx nobadfonts info <font>

# Browse the catalog
npx nobadfonts list

# Search for type
npx nobadfonts search <keyword>
```

## 🎨 Every font comes with its own CSS. Automatically.

Whether you `add` a font or simply `download` it, NoBadFonts CLI takes care of the CSS for you.

**Font files downloaded. `fonts.css` generated. No manual `@font-face` setup.**

```bash
# Add a font to your project
npx nobadfonts add <font>

# Or just download it
npx nobadfonts download <font>
```

Both commands automatically generate or update `fonts.css` with the selected font variants — ready for standard CSS and Tailwind CSS workflows.

### 1. One import. That's it.

Import the generated stylesheet into your main CSS file:

```css
@import "./fonts.css";
```

Or link it directly in your HTML:

```html
<link rel="stylesheet" href="./fonts.css" />
```

### 2. Your fonts. Your styles.

**Standard CSS**

```css
.heading {
  font-family: "YourFont", sans-serif;
}
```

**Tailwind CSS**

```html
<h1 class="font-your-font">
  Make something beautiful.
</h1>
```

Your generated `fonts.css` keeps the font declarations in one place, so you can use your downloaded fonts throughout your project.

### No repetitive setup. No manual font-face declarations.

Just pick your type. NoBadFonts handles the rest.

---

## ⌘ Built for the terminal

Whether you're prototyping a landing page, polishing a portfolio, or building your next big thing — NoBadFonts CLI keeps your type workflow close to your code.

Less hunting. More making.

---

## NoBadFonts

**The internet has enough bad fonts.**

Discover your next typeface at [nobadfonts.in](https://nobadfonts.in).

---

<div align="center">

**NoBadFonts CLI** · Good fonts. No friction. Just ship.

Made by [Bismay](https://github.com/Bismay-exe) with 🖤

</div>
