# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Onelink is a link-in-bio tool where all profile data is encoded as a base64 URL parameter — no database, no backend. Built with Nuxt 3 (Vue 3 Composition API) and Tailwind CSS.

## Commands

```bash
npm run dev      # Start dev server on localhost:3000
npm run build    # Production build
npm run generate # Static site generation
npm run preview  # Preview production build locally
```

No test or lint commands are configured.

## Architecture

### Data Flow

All profile data lives in the URL as a base64 query parameter (`?data=...`). The data object uses single-character keys to minimize URL length:

```
{ n: name, d: description, i: image, f: facebook, t: twitter, ig: instagram,
  gh: github, tg: telegram, l: linkedin, e: email, w: whatsapp, y: youtube,
  ls: [{ l: label, i: iconKey, u: url }] }
```

- `utils/transformer.js` — `encodeData()` / `decodeData()` using `JSON.stringify` + `js-base64`
- `pages/index.vue` — Editor: 3-column layout with form inputs and live phone preview
- `pages/1.vue` — Public render page (template `/1`): reads `?data=`, decodes, renders via template component

### Component Structure (Nuxt auto-imports by directory)

```
AppForm/          Editor form components
  Profile.vue     Name, description, photo URL
  SocialLinks.vue 9 social media fields with icon prefixes
  Links.vue       Custom links list with drag-and-drop (vuedraggable)
  Preview.vue     Phone frame mockup with live preview
  Hr.vue          Visual divider

Templates/        Public profile renderers
  Simple.vue      Renders avatar, name, bio, social icons, custom links

Base/             Reusable building blocks
  FormSection.vue Title/description wrapper for form sections
  Loading.vue     SVG spinner

ExternalLink.vue  Individual link item (icon + label + URL)
```

### Key Libraries

- **Nuxt 3** with `<script setup>` Composition API
- **Tailwind CSS** via `@nuxtjs/tailwindcss` module
- **nuxt-icon** — Iconify icons from https://icones.js.org (Phosphor, Material Design Icons, etc.)
- **@vueuse/nuxt** — VueUse composables
- **vuedraggable** — Drag-and-drop for custom links
- **@headlessui/vue** — Headless UI components
- **js-base64** — Base64 encode/decode for URL data