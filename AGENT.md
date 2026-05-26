# AGENT.md — Guía del proyecto Portfolio

Este archivo existe para que cualquier agente de IA o desarrollador
pueda entender el proyecto completo sin leer todo el código.
Leelo antes de hacer cualquier cambio.

> Para contexto detallado por tema, ver los archivos en `.agent/`.

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
│   ├── Sidebar.astro              # Sidebar flotante con nav y controles
│   └── sections/                  # Componentes de sección desacoplados
│       ├── About.astro
│       ├── Projects.astro
│       ├── Skills.astro
│       ├── Experience.astro
│       └── Contact.astro
├── i18n/
│   ├── es.json                    # Traducciones español
│   └── en.json                    # Traducciones inglés
├── layouts/
│   └── Layout.astro               # Layout principal (head + sidebar + main)
├── pages/
│   ├── index.astro                # Redirect raíz → /es/
│   └── [lang]/
│       └── index.astro            # Página única — compone las secciones
└── styles/
    └── global.css                 # Sistema de estilos completo
```

## Documentación extendida (.agent/)

Contexto detallado organizado por tema:

| Archivo                          | Contenido                              |
|----------------------------------|----------------------------------------|
| `.agent/styles.md`               | Sistema de estilos, variables CSS, clases de componente |
| `.agent/layout.md`               | Layout, sidebar flotante, scroll spy   |
| `.agent/components.md`           | Arquitectura de componentes, props     |

## Reglas rápidas

1. **Colores**: NUNCA hardcodear → usar `bg-surface`, `text-muted`, `text-accent`, etc.
2. **Texto**: NUNCA hardcodear → siempre `t.seccion.clave` del JSON de i18n
3. **Idioma**: cambio vía `<a href>` — NUNCA JavaScript en cliente
4. **Tema**: clase `dark` en `<html>` + variables CSS hacen el toggle automático
5. **Iconos**: `astro-icon` con conjuntos `simple-icons` y `lucide`
6. **Scripts**: siempre escuchar `astro:after-swap` para reinicializar tras navegación
7. **Secciones**: cada sección es un componente en `src/components/sections/`
8. **No ejecutar builds ni tests automáticos en el navegador**: Al finalizar tareas o turnos, no es necesario hacer `pnpm build` ni lanzar subagentes de navegador para verificar visualmente. El usuario realizará la verificación de forma manual.

## Convenciones

- Componentes: PascalCase → `Sidebar.astro`, `About.astro`
- Variables CSS internas: `--c-*` (ej: `--c-accent`)
- Variables Tailwind: `--color-*` (generadas por `@theme`)
- Scripts de interactividad: escuchar `astro:after-swap`
- Sin librerías JS externas. Solo Astro + Tailwind + vanilla JS.

## Comandos útiles

```bash
pnpm dev          # desarrollo local
pnpm build        # build de producción
pnpm preview      # preview del build
```

## Lo que falta por implementar

- [ ] Sección About completa con bio y stack badges
- [ ] Sección Projects con cards de FastCashier y CarpinPro
- [ ] Sección Skills con categorías y badges
- [ ] Sección Experience con timeline
- [ ] Sección Contact con links y formulario
- [x] Sidebar flotante con estilo card
- [x] Componentes de sección desacoplados
- [ ] Responsive / mobile (sidebar como drawer)
- [ ] SEO: meta tags, og:image, sitemap
- [ ] Deploy en Vercel o Netlify
