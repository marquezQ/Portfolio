# Arquitectura de Componentes

## Principio: secciones desacopladas

Cada sección del portfolio es un componente independiente en
`src/components/sections/`. La página `index.astro` simplemente
los compone en orden:

```astro
<Layout lang={lang} t={t}>
  <About t={t} />
  <Projects t={t} />
  <Skills t={t} />
  <Experience t={t} />
  <Contact t={t} />
</Layout>
```

### Beneficios

- **Aislamiento**: cada sección se puede editar sin tocar las demás
- **Reusabilidad**: se pueden importar en otras páginas si fuera necesario
- **Mantenimiento**: archivos pequeños y enfocados
- **Orden**: reordenar secciones es mover una línea de HTML

## Componentes existentes

### Layout y estructura

| Componente       | Ruta                        | Props         |
|------------------|-----------------------------|---------------|
| `Layout`         | `src/layouts/Layout.astro`  | `lang`, `t`   |
| `Sidebar`        | `src/components/Sidebar.astro` | `lang`, `t` |

### Secciones

| Componente     | Ruta                                         | Props | id         |
|----------------|----------------------------------------------|-------|------------|
| `About`        | `src/components/sections/About.astro`        | `t`   | `about`    |
| `Projects`     | `src/components/sections/Projects.astro`     | `t`   | `projects` |
| `Skills`       | `src/components/sections/Skills.astro`       | `t`   | `skills`   |
| `Experience`   | `src/components/sections/Experience.astro`   | `t`   | `experience` |
| `Contact`      | `src/components/sections/Contact.astro`      | `t`   | `contact`  |

## Contrato de props

Todos los componentes de sección reciben un único prop `t`:
```typescript
interface Props {
  t: Record<string, any>;
}
```

`t` es el objeto JSON de traducciones completo. Cada sección
accede a sus claves: `t.hero.*`, `t.projects.*`, `t.skills.*`, etc.

## Reglas para crear nuevas secciones

1. Crear archivo en `src/components/sections/NuevaSeccion.astro`
2. El `<section>` debe tener un `id` único que coincida con la clave en `t.nav`
3. Usar `min-h-[70vh]` o `min-h-screen` y espaciado vertical (`py-12 md:py-20`) para mantener alineación y permitir al scroll spy funcionar
4. Importar y agregar en `src/pages/[lang]/index.astro`
5. Agregar la clave del nav en ambos JSONs de i18n
6. Agregar el item en `navItems` de `Sidebar.astro` con su ícono lucide
7. El título de la sección se muestra dinámicamente en el breadcrumb superior, evitando repetir títulos estilo terminal.

## Detalles de Secciones

### About (`src/components/sections/About.astro`)
Presenta el perfil profesional del usuario. Contiene:
- Pill animado con el rol/título actual.
- Nombre y biografía extraídos de las traducciones.
- Grid de badges tecnológicas interactivas generadas a partir de `t.hero.stack`.
- Botón principal de descarga de CV con icono Lucide.
- Enlaces de redes sociales (GitHub y LinkedIn) representados por sus logos correspondientes de Simple Icons.

### Skills (`src/components/sections/Skills.astro`)
Diseñada visualmente como monitores de entorno de sistema (System Monitor) estilo Linux, para las habilidades técnicas:
- **Estructura Dinámica**: Agrupa las categorías del JSON (Frontend, Backend, Bases de Datos, DevOps) en 3 entornos principales (`frontend_env`, `backend_env`, `devops_env`).
- **Arquitectura UI (System Monitor)**: Utiliza un "header sutil" con iconos de infraestructura (`lucide:layout`, `server`, `cloud`) y un indicador de estado animado (Online). El cuerpo emula una terminal con un prompt claro (`ls -la ./FRONTEND`).
- **Mapeo Inteligente de Íconos**: Incluye un diccionario estático `iconMap` que toma los strings del JSON ("React", "Docker") y los asocia automáticamente con sus logos de la base de datos de `simple-icons` a través de Astro Icon, renderizando SVGs puros en tiempo de compilación.
- **Responsive Grid Mágico**: Implementa un patrón responsivo avanzado para maximizar el uso de espacio:
  - Teléfonos (`<sm`): Tarjetas 100% ancho, lista en 1 columna (`grid-cols-1`).
  - Tabletas/Laptops (`sm` a `lg`): Tarjetas 100% ancho (apiladas verticalmente), pero la lista de habilidades se expande a 2 o 3 columnas (`sm:grid-cols-2 md:grid-cols-3`) para aprovechar el enorme espacio horizontal y evitar vacíos.
  - Desktop Grande (`xl`): Las tarjetas se colocan una al lado de la otra en 3 columnas (`xl:grid-cols-3`). Para evitar que las habilidades internas se aplasten, la lista vuelve inteligentemente a 1 columna (`xl:grid-cols-1`).
