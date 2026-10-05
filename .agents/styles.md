# Sistema de Estilos

## Arquitectura

Este proyecto usa **Tailwind CSS v4** con configuración 100% en CSS.
NO existe `tailwind.config.mjs`. Todo está en `src/styles/global.css`.

### Directivas Tailwind v4 usadas

```css
@import "tailwindcss";              /* Reemplaza @tailwind base/components/utilities */
@plugin "@tailwindcss/typography";  /* Reemplaza plugins: [] del config JS */
@custom-variant dark (...);         /* Reemplaza darkMode: 'class' */
@theme { }                          /* Reemplaza theme.extend del config JS */
```

## Variables CSS

Las variables internas (`--c-*`) se definen en `:root` (light) y `.dark` (dark).
`@theme` las expone a Tailwind como `--color-*` para generar clases utilitarias.

### Tabla de tokens

| Clase Tailwind       | Variable interna      | Uso                        |
|----------------------|-----------------------|----------------------------|
| `bg-bg`              | `--c-bg`              | Fondo de página            |
| `bg-surface`         | `--c-surface`         | Fondo de cards y sidebar   |
| `bg-hover`           | `--c-hover`           | Hover de elementos         |
| `border-border`      | `--c-border`          | Bordes generales           |
| `text-text`          | `--c-text`            | Texto principal            |
| `text-muted`         | `--c-muted`           | Texto secundario           |
| `text-dim`           | `--c-dim`             | Texto apagado              |
| `text-accent`        | `--c-accent`          | Cyan — color principal     |
| `text-accent-bright` | `--c-accent-bright`   | Cyan brillante             |
| `text-accent-dim`    | `--c-accent-dim`      | Cyan oscuro                |

### Valores por tema

| Token        | Light       | Dark        |
|-------------|-------------|-------------|
| bg          | `#f1f5f9`   | `#0b0f1a`   |
| surface     | `#ffffff`   | `#111827`   |
| accent      | `#0891b2`   | `#22d3ee`   |
| text        | `#0f172a`   | `#e2e8f0`   |

## Clases de componente

Definidas en `@layer components` de global.css:

| Clase              | Uso                                            |
|--------------------|------------------------------------------------|
| `.section-label`   | Etiqueta estilo terminal (mono, uppercase, cyan) |
| `.card`            | Card con hover glow de acento                  |
| `.tech-badge`      | Badge de tecnología                            |
| `.nav-link`        | Link del sidebar — incluye estado `.active`    |
| `.sidebar-glow`    | Sombra/glow sutil del sidebar flotante         |
| `.prose-portfolio` | Prose para descripciones largas                |

## Tipografía

- **font-sans** (Inter): títulos, texto general, UI
- **font-mono** (JetBrains Mono): section-labels, nav links, badges, código

Google Fonts se importan al inicio de global.css.

## Dark Mode

La clase `dark` se aplica a `<html>`. Los colores se adaptan
automáticamente porque todas las clases Tailwind referencian
variables CSS que cambian según `:root` vs `.dark`.

**Regla**: si usas clases semánticas (`bg-surface`, `text-muted`),
NO necesitas agregar `dark:` variants. El cambio es automático.
