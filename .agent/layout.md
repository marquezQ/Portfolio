# Layout y Sidebar

## Layout (`src/layouts/Layout.astro`)

El Layout es el esqueleto de la aplicación. Recibe `lang` y `t` como props.

### Responsabilidades

1. **`<head>`**: charset, viewport, meta description, title, hreflang, favicon
2. **Script anti-flash**: `<script is:inline>` que lee localStorage antes del render
3. **Sidebar**: renderiza `<Sidebar>` con props `lang` y `t`
4. **Main**: `<main>` donde se inyecta el `<slot />`

### Estructura HTML

```html
<html lang={lang} class="dark">
  <head>...</head>
  <body>
    <!-- Backdrop del Drawer (móvil) -->
    <div id="sidebar-backdrop" class="fixed inset-0 z-40 ..."></div>

    <div class="flex h-screen overflow-hidden">
      <!-- Contenedor del sidebar (Drawer en móvil / Sticky en desktop) -->
      <div id="sidebar-wrapper" class="fixed inset-y-0 left-0 z-50 w-76 -translate-x-full lg:translate-x-0 lg:sticky lg:top-0 lg:h-screen lg:w-80 ...">
        <Sidebar />
      </div>
      <!-- Columna de contenido principal -->
      <main class="flex-1 flex flex-col h-screen min-w-0">
        <!-- Breadcrumb / Header (Limpio, sin fondo ni bordes, alineado con el top del sidebar card) -->
        <header class="shrink-0 flex justify-between px-8 pt-8 pb-5 md:px-16 lg:pt-8 lg:pb-6...">
          <span id="breadcrumb-text">PORTFOLIO OS - SOBRE MÍ</span>
          <span>V1.0.0</span>
        </header>
        <!-- Contenedor con scroll interno justificado a la izquierda -->
        <div class="flex-1 overflow-y-auto w-full px-8 md:px-16 pb-12">
          <div class="max-w-4xl">
            <slot />
          </div>
        </div>
      </main>
    </div>
  </body>
</html>
```

## Responsive y Drawer Móvil
- **Pantallas grandes (`>= lg`)**: El sidebar se comporta de manera `sticky` en su columna de ancho `w-80` reservando su espacio.
- **Pantallas pequeñas (`< lg`)**: El sidebar se oculta por defecto (`-translate-x-full`) convirtiéndose en un drawer lateral deslizante. El botón de sandwich integrado en el breadcrumb despliega el sidebar, y un backdrop oscuro difuminado bloquea el fondo. Al hacer clic en el backdrop o en un enlace del nav, el drawer se cierra automáticamente.

## Breadcrumbs Dinámicos y Scroll Limpio
El breadcrumb actúa como un indicador transparente flotante con el formato `PORTFOLIO OS - [PATH]` en el extremo izquierdo y `V1.0.0` en el extremo derecho. Está alineado verticalmente con el inicio del sidebar card (`pt-8`).
Para evitar que el texto de las secciones se solape con el texto del breadcrumb al scrollear (debido a la ausencia de fondo/difuminado en la cabecera), el scroll de la página se realiza de manera interna en el contenedor de las secciones (`overflow-y-auto`). De esta forma, el contenido desaparece físicamente al alcanzar el límite inferior de la cabecera, logrando un scroll sumamente limpio y profesional.

## Sidebar flotante (`src/components/Sidebar.astro`)

### Diseño visual

El sidebar es un **card con aspecto flotante** gracias al padding de su columna contenedora, con esquinas redondeadas y comportamiento `sticky`.

```
sticky top-4 z-50 flex h-[calc(100vh-2rem)] flex-col
rounded-2xl bg-surface border border-border sidebar-glow
```

### Secciones del sidebar (de arriba a abajo)

1. **Perfil**: avatar circular + nombre (primer nombre + apellido) + "FULL-STACK ENGINEER"
2. **Controles**: fila horizontal con switch de idioma (ES/EN) y toggle de tema (🌙/☀️)
3. **Navegación**: links con iconos lucide por sección, estado `.active` con borde derecho cyan

### Nav items con iconos

Cada link de navegación tiene un ícono lucide específico:

| Sección    | Ícono              |
|------------|---------------------|
| about      | `lucide:user`       |
| projects   | `lucide:code-2`     |
| skills     | `lucide:monitor`    |
| experience | `lucide:clock-3`    |
| contact    | `lucide:mail`       |

### Scroll spy

Un `IntersectionObserver` (`threshold: 0.4`) observa todas las
`section[id]` y aplica la clase `active` al `.nav-link` cuyo
`data-section` coincide con el `id` de la sección visible.

### Switch de idioma

Un `<a href>` simple que navega a `/${otherLang}/`.
Sin JavaScript, sin estado reactivo.

### Toggle de tema

Botón con `id="theme-toggle"`. Al hacer click:
1. Alterna clase `dark` en `<html>`
2. Guarda en `localStorage`
3. Actualiza ícono (🌙 ↔ ☀️)
