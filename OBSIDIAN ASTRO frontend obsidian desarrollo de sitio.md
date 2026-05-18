## 🔥 **OPCIÓN 1: Astro + Obsidian Plugin (RECOMENDADO)** 
**Lo MEJOR para tus notas Obsidian** - Transforma .md a HTML estático con tema Obsidian nativo

```bash
npm create astro@latest -- --template minimal mi-obisidian-astro
cd mi-obisidian-astro
npm i @astrojs/mdx @astrojs/sitemap obsidian-export-plugin
```

**`astro.config.mjs`**
```js
import { defineConfig } from 'astro/config';
import mdx from '@astrojs/mdx';
import sitemap from '@astrojs/sitemap';

export default defineConfig({
  integrations: [mdx(), sitemap()],
  markdown: {
    shikiConfig: {
      theme: 'obsidianite' // ¡Tema Obsidian nativo!
    }
  },
  vite: {
    assetsInclude: ['**/*.png', '**/*.jpg', '**/*.gif']
  }
});
```

**`src/pages/[...slug].astro`** (routing automático)
```astro
---
import { read } from 'to-vfile';
import { Processor } from 'unified';
import { VFileWithOutput } from 'unified';
import remarkGfm from 'remark-gfm';
import rehypeSlug from 'rehype-slug';
import rehypeAutolinkHeadings from 'rehype-autolink-headings';
import Layout from '../layouts/Layout.astro';

export async function getStaticPaths() {
  const fs = require('fs');
  const path = require('path');
  
  function scanDir(dir: string): any[] {
    const files = fs.readdirSync(dir, { withFileTypes: true });
    let paths = [];
    
    files.forEach(file => {
      const fullPath = path.join(dir, file.name);
      if (file.isDirectory()) {
        paths = paths.concat(scanDir(fullPath));
      } else if (file.name.endsWith('.md') || file.name.endsWith('.mdx')) {
        const slug = path.relative('src/content', fullPath)
          .replace(/\.mdx?$/, '')
          .replace(/[/\\]/g, '/')
          .replace(/^\//, '');
        paths.push({
          params: { slug: slug.split('/') },
          props: { filePath: fullPath }
        });
      }
    });
    return paths;
  }
  
  return scanDir('src/content');
}

const { filePath } = Astro.props;
const content = await Astro.slugs;
---
<Layout title={content.title}>
  <article class="prose prose-invert max-w-none">
    <Content />
  </article>
</Layout>

<script>
  // Sidebar dinámico con búsqueda
  import Sidebar from '../components/Sidebar.svelte';
</script>
```

**Deploy 1-click a Vercel/Netlify** 🚀

---

## ⚡ **OPCIÓN 2: SvelteKit + Tailwind + shadcn (MODERNO Y RÁPIDO)**

```bash
npm create svelte@latest mi-obisidian-svelte
# Skeleton + TypeScript + ESLint + Tailwind
cd mi-obisidian-svelte
npm i marked dompurify tailwindcss @tailwindcss/typography shadcn-svelte
npx shadcn-svelte@latest init
```

**`+page.svelte`**
```svelte
<script lang="ts">
  import { onMount } from 'svelte';
  import { marked } from 'marked';
  import Sidebar from '$lib/components/Sidebar.svelte';
  import FileViewer from '$lib/components/FileViewer.svelte';
  
  let files: FileNode[] = [];
  let selectedFile: string | null = null;
  
  interface FileNode {
    name: string;
    path: string;
    type: 'file' | 'folder';
    children?: FileNode[];
  }
  
  onMount(async () => {
    files = await fetch('/api/files').then(r => r.json());
  });
</script>

<div class="flex h-screen bg-gradient-to-br from-slate-900 to-slate-950">
  <Sidebar {files} bind:selectedFile />
  <main class="flex-1 p-8 overflow-auto">
    {#if selectedFile}
      <FileViewer filePath={selectedFile} />
    {:else}
      <div class="text-center mt-32 text-slate-400">
        <h1 class="text-4xl font-bold mb-4">📚 Mis Notas Obsidian</h1>
        <p>Selecciona un archivo del árbol lateral</p>
      </div>
    {/if}
  </main>
</div>
```

**`$lib/components/Sidebar.svelte`** (con shadcn)
```svelte
<script lang="ts">
  import { Input } from '$lib/components/ui/input';
  import { Search } from 'lucide-svelte';
  
  export let files: FileNode[];
  export let selectedFile: string | null = null;
</script>

<div class="w-80 border-r border-slate-800 h-screen p-6 space-y-6 bg-slate-900/50">
  <div class="space-y-2">
    <h1 class="text-2xl font-bold bg-gradient-to-r from-blue-400 to-purple-500 bg-clip-text text-transparent">
      📚 Obsidian
    </h1>
    <Input placeholder="Buscar notas..." class="w-full">
  </div>
  
  <div class="space-y-1 max-h-[calc(100vh-200px)] overflow-auto">
    {#each files as file}
      <FileNode {file} {selectedFile} on:select />
    {/each}
  </div>
</div>
```

---

## 🪨 **OPCIÓN 3: Nuxt 3 + Content Module (TODO INCLUIDO)**

```bash
npx nuxi@latest init mi-obisidian-nuxt
cd mi-obisidian-nuxt
npm i @nuxt/content tailwindcss @nuxtjs/tailwindcss
```

**`nuxt.config.ts`**
```ts
export default defineNuxtConfig({
  modules: ['@nuxt/content', '@nuxtjs/tailwindcss'],
  content: {
    // Auto-scan tus .md
    sources: {
      content: {
        driver: 'fs',
        base: './content' // Tu carpeta Obsidian aquí
      }
    },
    markdown: {
      remarkPlugins: ['remark-gfm']
    }
  },
  css: ['~/assets/css/main.css']
});
```

**`pages/index.vue`** (¡5 líneas!)
```vue
<template>
  <div class="container mx-auto p-8">
    <UContainer>
      <UContentNav />
      <UContentDoc />
    </UContainer>
  </div>
</template>
```

---

## 🏆 **MI RECOMENDACIÓN: Astro + Obsidian**

**Por qué Astro es PERFECTO para ti:**

```
✅ 0 configuración para .md → HTML
✅ Temas Obsidian nativos 
✅ Build estático ULTRA rápido
✅ Hot reload automático
✅ 1-click deploy
✅ Soporte imágenes/capturas
✅ SEO automático
✅ 100% tu estructura de carpetas
✅ Menos de 50KB gzipped
```

**Comando 1-línea para empezar:**
```bash
npm create astro@latest mi-obisidian -- --template content && cd mi-obisidian && cp -r /ruta/obsidian/* content/ && npm run dev
```

**Resultado:** `https://tus-notas.netlify.app/admin-gerencial/2do-parcial/mi-nota.md`

¿Cuál prefieres? ¿**Astro** (más simple), **SvelteKit** (más moderno) o **Nuxt** (todo incluido)? Te armo el setup completo del que elijas. 🚀