# AGENT.md — Guía del proyecto Portfolio

Este archivo existe para que cualquier agente de IA o desarrollador
pueda entender el proyecto completo sin leer todo el código.
Leelo antes de hacer cualquier cambio.

## ¿Qué es este proyecto?

Portfolio personal de Pedro Marquez Quiroz, Full-Stack Developer.
Construido con Astro 6 y Tailwind CSS v4. Una sola página (single-page)
con secciones: Sobre mí, Proyectos, Habilidades, Experiencia y Contacto.
Disponible en dos idiomas: español (/es/) e inglés (/en/).

## Stack técnico

- Framework:  Astro 6
- Estilos:    Tailwind CSS v4 + CSS custom properties
- Iconos:     astro-icon (simple-icons + lucide)
- Tipografía: Inter (sans) + JetBrains Mono (mono) vía Google Fonts
- i18n:       i18n routing nativo de Astro
- Package manager: pnpm

## Estructura de archivos

```
src/
├── components/
│   └── Sidebar.astro        # Sidebar fijo con nav, controles, perfil
├── i18n/
│   ├── es.json               # Traducciones español
│   └── en.json               # Traducciones inglés
├── layouts/
│   └── Layout.astro          # Layout principal con sidebar + main
├── pages/
│   ├── index.astro           # Redirect raíz → /es/
│   └── [lang]/
│       └── index.astro       # Página única para ambos idiomas
└── styles/
    └── global.css            # Sistema de estilos completo
```

## Sistema de estilos — LEER ANTES DE AGREGAR CLASES

**IMPORTANTE: Este proyecto usa Tailwind CSS v4**, que configura todo
en CSS (no usa `tailwind.config.mjs`). La configuración está en
`src/styles/global.css` usando `@theme`, `@custom-variant` y `@plugin`.

NUNCA hardcodear colores en componentes. Todo pasa por variables CSS.

Las variables internas (`--c-*`) están definidas en `:root` (claro) y
`.dark` (oscuro). `@theme` las expone a Tailwind como `--color-*`:

| Clase Tailwind       | Variable CSS interna    | Uso                        |
|----------------------|-------------------------|----------------------------|
| `bg-bg`              | `--c-bg`                | Fondo de página            |
| `bg-surface`         | `--c-surface`           | Fondo de cards y sidebar   |
| `bg-hover`           | `--c-hover`             | Hover de elementos         |
| `border-border`      | `--c-border`            | Bordes generales           |
| `text-text`          | `--c-text`              | Texto principal            |
| `text-muted`         | `--c-muted`             | Texto secundario           |
| `text-dim`           | `--c-dim`               | Texto apagado              |
| `text-accent`        | `--c-accent`            | Cyan — color principal     |
| `text-accent-bright` | `--c-accent-bright`     | Cyan brillante             |
| `text-accent-dim`    | `--c-accent-dim`        | Cyan oscuro                |

Clases de componente reutilizables (definidas en `@layer components` de global.css):
- `.section-label`  → etiqueta de sección estilo terminal (mono, uppercase, cyan)
- `.card`           → card con hover y glow de acento
- `.tech-badge`     → badge de tecnología (React, NestJS, etc.)
- `.nav-link`       → link del sidebar con estado hover
- `.nav-link.active` → link activo en el sidebar
- `.prose-portfolio` → prose para descripciones largas

## Tailwind v4 — Diferencias clave

Este proyecto **NO** usa `tailwind.config.mjs`. En Tailwind v4:
- `@import "tailwindcss"` reemplaza a `@tailwind base/components/utilities`
- `@theme { }` reemplaza a `theme.extend` del config JS
- `@plugin "@tailwindcss/typography"` reemplaza a `plugins: [typography]`
- `@custom-variant dark (...)` reemplaza a `darkMode: 'class'`
- Los estilos se procesan con `@tailwindcss/vite` (plugin de Vite)

## Tipografía

- Títulos y UI general: `font-sans` (Inter)
- Etiquetas de sección, badges, código, nav: `font-mono` (JetBrains Mono)

## Sistema de temas (dark/light)

- La clase `dark` se aplica al elemento `<html>`
- Un script inline con `is:inline` en el `<head>` del Layout lee
  localStorage antes de pintar para evitar flash
- La clave en localStorage es `theme`, valores: `'dark'` | `'light'`
- Por defecto: dark. Si no hay valor guardado, respeta
  `prefers-color-scheme` del sistema
- El botón tiene `id="theme-toggle"`
- Después de cualquier cambio al toggle, llamar `initTheme()`
- Escuchar `astro:after-swap` para reinicializar tras navegación

## Sistema de idiomas (i18n)

- Routing nativo de Astro con `prefixDefaultLocale: true`
- Rutas: `/es/` → español, `/en/` → inglés
- Un único archivo de página: `src/pages/[lang]/index.astro`
- Los JSON de traducciones están en `src/i18n/es.json` y `en.json`
- El Layout y Sidebar reciben `lang` y `t` como props
- El switch de idioma es un `<a href>` simple — sin JavaScript
- NUNCA usar JavaScript para cambiar el idioma en cliente

## Iconos

Usar `astro-icon`. Conjuntos disponibles:
- `simple-icons`: logos de tecnologías (react, nestjs, postgresql, etc.)
- `lucide`: iconos de UI (map-pin, github, linkedin, mail, etc.)

Ejemplo de uso:
```astro
import { Icon } from 'astro-icon/components';
<Icon name="simple-icons:react" class="w-5 h-5 text-accent" />
<Icon name="lucide:map-pin" class="w-4 h-4 text-muted" />
```

## Secciones del index

Cada sección debe tener:
- `id` exacto que coincida con la clave del nav: `about`, `projects`,
  `skills`, `experience`, `contact`
- `min-h-screen` para que el scroll spy funcione correctamente
- padding: `p-8 md:p-16`

El Intersection Observer en `Sidebar.astro` detecta qué sección
está en el viewport (`threshold: 0.4`) y agrega la clase `active`
al link correspondiente del nav.

## Contenido del portfolio

Todo el texto está en los JSON de i18n. NO hardcodear texto
en los componentes. Siempre usar `t.seccion.clave`.

El dueño del portfolio es Pedro Marquez Quiroz:
- Full-Stack Developer con React, NestJS y PostgreSQL
- Interesado en DevOps, cloud (AWS) y flujos CI/CD
- Trabaja con agentes de IA integrados al flujo de desarrollo
- Ubicación: Cochabamba, Bolivia
- Proyectos principales: FastCashier (POS), CarpinPro (plataforma)

## Convenciones

- Componentes: PascalCase → `Sidebar.astro`, `ThemeToggle.astro`
- Variables CSS: kebab-case con prefijo `--c-` (internas) o `--color-` (Tailwind)
- Clases Tailwind: preferir clases semánticas sobre utilitarias crudas
- Scripts de interactividad: siempre escuchar `astro:after-swap`
  para que funcionen tras navegación client-side
- Sin librerías JS externas. Solo Astro + Tailwind + vanilla JS.

## Comandos útiles

```bash
pnpm dev          # desarrollo local
pnpm build        # build de producción
pnpm preview      # preview del build
```

## Lo que falta por implementar (próximos pasos)

- [ ] Sección About completa con bio y stack badges
- [ ] Sección Projects con cards de FastCashier y CarpinPro
- [ ] Sección Skills con categorías y badges
- [ ] Sección Experience con timeline
- [ ] Sección Contact con links y formulario
- [ ] Diseño visual final del Sidebar (avatar, estilos)
- [ ] Responsive / mobile (sidebar como drawer)
- [ ] SEO: meta tags, og:image, sitemap
- [ ] Deploy en Vercel o Netlify
