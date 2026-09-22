# NoBadFonts CLI

### The typeface you want. The terminal you love.

[![npm version](https://img.shields.io/npm/v/nobadfonts?color=CB9EFF\&label=npm)](https://www.npmjs.com/package/nobadfonts)
[![npm downloads](https://img.shields.io/npm/dm/nobadfonts?color=CB9EFF)](https://www.npmjs.com/package/nobadfonts)
[![License](https://img.shields.io/npm/l/nobadfonts)](LICENSE)

**Discover, install, and download fonts from [NoBadFonts](https://nobadfonts.in) — straight from your terminal.**

NoBadFonts CLI makes working with typefaces effortless. Add fonts to your project, download individual variants, explore font metadata, or search the catalog — all without leaving your workflow.

```bash
npx nobadfonts add offbit
```

No hunting through websites. No manually managing font files. Just good type, right where you need it.

---

## ✦ Features

* **One-command font installation** — Add fonts to your project with a single command.
* **Selective variants** — Install or download only the font weights and styles you need.
* **Interactive selection** — Choose variants through an interactive terminal prompt.
* **Automatic CSS generation** — Generate CSS when adding fonts to your project.
* **Direct font downloads** — Download font files without the project integration workflow.
* **Font information** — Inspect available variants and font metadata.
* **Catalog search** — Find typefaces by keyword.
* **Global CLI support** — Install once and use the `nobadfonts` command across projects.

---

## 🚀 Quick Start

### Option 1: Run instantly with npx

No global installation required. Run the CLI directly:

```bash
npx nobadfonts add offbit
```

This adds Offbit to your project using the default installation behavior.

### Option 2: Install globally

If you use NoBadFonts CLI regularly, install it globally:

```bash
npm install -g nobadfonts
```

Then run it from any project directory:

```bash
nobadfonts add offbit
```

That's it. You're ready to bring better typography into your projects.

---

## 📦 Commands

### Add a font

Add a font to your project. By default, all available variants are included.

```bash
npx nobadfonts add offbit
```

Use `add` when you want to integrate a font into your project, including its generated CSS.

### Download a font

Download font files without the project integration workflow.

```bash
npx nobadfonts download offbit
```

By default, all available variants are downloaded.

### Add or download specific variants

Don't need every weight or style? Select only the variants you want.

**Using `--variant`:**

```bash
npx nobadfonts add offbit --variant pixelbold
```

```bash
npx nobadfonts download offbit --variant pixelbold
```

**Using the `-v` shorthand:**

```bash
npx nobadfonts add offbit -v pixelbold -v extradot-pixel
```

```bash
npx nobadfonts download offbit -v pixelbold -v extradot-pixel
```

**Using comma-separated variants:**

```bash
npx nobadfonts add offbit --variants pixelbold,extradot-pixel
```

```bash
npx nobadfonts download offbit --variants pixelbold,extradot-pixel
```

All three formats allow you to specify the variants you need instead of downloading every available variant.

### Interactive variant selection

Not sure which variants are available? Let the CLI guide you.

```bash
npx nobadfonts add offbit --select
```

Or download selected variants:

```bash
npx nobadfonts download offbit --select
```

The interactive terminal prompt lets you choose from the font's available variants.

---

## 🔍 Explore the catalog

### Get font information

Inspect a font and its available metadata:

```bash
npx nobadfonts info offbit
```

### List fonts

Browse fonts available through the CLI:

```bash
npx nobadfonts list
```

### Search fonts

Search the NoBadFonts catalog using a keyword:

```bash
npx nobadfonts search display
```

Try searching for styles, classifications, or other keywords to discover typefaces for your next project.

---

## 🧰 Command Reference

| Command                                   | Description                                |
| ----------------------------------------- | ------------------------------------------ |
| `nobadfonts add <font>`                   | Add all available variants to your project |
| `nobadfonts download <font>`              | Download all available variants            |
| `nobadfonts add <font> -v <variant>`      | Add a specific variant                     |
| `nobadfonts download <font> -v <variant>` | Download a specific variant                |
| `nobadfonts add <font> --select`          | Interactively select variants to add       |
| `nobadfonts download <font> --select`     | Interactively select variants to download  |
| `nobadfonts info <font>`                  | View font information                      |
| `nobadfonts list`                         | List available fonts                       |
| `nobadfonts search <keyword>`             | Search the font catalog                    |

**Variant options**

| Option               | Description                        |
| -------------------- | ---------------------------------- |
| `--variant <name>`   | Specify a variant (repeatable)     |
| `-v <name>`          | Shorthand for `--variant`          |
| `--variants <names>` | Specify comma-separated variants   |
| `--select`           | Open interactive variant selection |

Replace `<font>` with a font slug, such as `offbit`, and `<variant>` with an available variant slug.

---

## 💡 Example Workflow

Imagine you're building a project and want to use Offbit, but only need two variants.

```bash
# Add only the variants you need
npx nobadfonts add offbit -v pixelbold -v extradot-pixel
```

Need the font files without adding them to your project?

```bash
npx nobadfonts download offbit -v pixelbold
```

Want to explore before installing?

```bash
npx nobadfonts search pixel
npx nobadfonts info offbit
```

From discovery to implementation, NoBadFonts CLI keeps typography in your development workflow.

---

## 🌐 Discover more fonts

Explore the full catalog, preview typefaces, and find your next favorite font:

**[nobadfonts.in/fonts](https://nobadfonts.in/fonts)**

---

## 📄 License

MIT © NoBadFonts

See the [LICENSE](LICENSE) file for details.

---

<p align="center">
  Built for developers who give a damn about typography.
  <br />
  <strong><a href="https://nobadfonts.in">NoBadFonts</a></strong>
</p>
