# Proyecto base Astro (SSG) para hosting compartido con cPanel

## 1. Objetivo del documento

Este archivo define **paso a paso** cómo inicializar, estructurar y colaborar en un proyecto **Astro estático**, pensado para desplegarse en `public_html` de un dominio principal (NO subdominio), usando:

- Astro (SSG)
- Tailwind CSS v4
- Animaciones simples, modernas y mantenibles
- SwiperJS para sliders y carruseles
- Librería de iconos compatible con Astro
- Flujo de trabajo colaborativo con Git (2 devs frontend + Dev Sr)

Está pensado para **devs que usan Astro por primera vez**.

---

## 2. Stack tecnológico aprobado

- **Framework:** Astro (Static Site Generation)
- **CSS base:** Tailwind CSS v4
- **CSS custom:** archivos CSS propios + variables de identidad corporativa
- **Animaciones:**
  - CSS (Tailwind + custom)
  - `@astrojs/view-transitions`
  - `motion` (opcional, para casos puntuales)

- **Sliders / Carruseles:** SwiperJS
- **Iconos:** Lucide Icons (`lucide-astro`)
- **Deploy:** Build local → subir contenido de `/dist` a `public_html`

---

## 3. Inicialización del proyecto (Dev Sr)

```bash
npm create astro@latest empresa-web
cd empresa-web
npm install
npm run dev
```

### Configuración Astro (`astro.config.mjs`)

```js
import { defineConfig } from "astro/config";

export default defineConfig({
  output: "static",
});
```

> ⚠️ Nunca usar SSR en este proyecto.

---

## 4. Estructura de carpetas propuesta

```
/src
 ├── components
 │   ├── layout
 │   ├── ui
 │   ├── navigation
 │   ├── sliders
 │   └── sections
 ├── layouts
 │   └── BaseLayout.astro
 ├── pages
 │   ├── index.astro
 │   └── servicios
 │       └── [slug].astro
 ├── styles
 │   ├── global.css
 │   ├── variables.css
 │   └── animations.css
 ├── assets
 │   ├── images
 │   └── logos
 └── data
     ├── services.ts
     └── faqs.ts
```

---

## 5. Tailwind CSS v4 (Integración oficial Astro)

### Instalación (forma correcta y recomendada)

```bash
npx astro add tailwind
```

Esta integración:

- Configura Tailwind automáticamente
- Usa Vite internamente (gestionado por Astro)
- Evita configuraciones manuales de PostCSS
- Es la vía más estable para proyectos Astro

### CSS global

Astro creará o reutilizará un archivo global (ejemplo: `src/styles/tailwind.css`).
Debe contener como mínimo:

```css
@import "tailwindcss";
```

> En Tailwind v4 ya **no se usan** `@tailwind base/components/utilities`.

### Importación en el layout base

En `src/layouts/BaseLayout.astro`:

```astro
---
import "../styles/tailwind.css";
---
```

Esto garantiza que Tailwind esté disponible globalmente en todo el proyecto.

> ⚠️ No instalar Tailwind manualmente con Vite ni PostCSS. Cualquier cambio debe ser aprobado por el Dev Sr.

---

## 6. Animaciones – Propuesta oficial

### Nivel 1 (base – obligatorio)

- CSS animations + transitions
- Tailwind utilities (`transition`, `duration`, `ease`)

### Nivel 2 (secciones clave)

- `@astrojs/view-transitions`

```bash
npx astro add view-transitions
```

Uso:

```astro
<html transition:animate>
```

### Nivel 3 (opcional)

- `motion` para micro-interacciones complejas

👉 **Regla:** Animaciones sutiles, no excesivas.

---

## 7. Iconos

### Librería elegida: Lucide

```bash
npm install lucide-astro
```

Uso:

```astro
import { Phone, Mail, Clock } from 'lucide-astro';
```

---

## 8. SwiperJS

```bash
npm install swiper
```

Se usará para:

- Slider principal (10 slides por servicio)
- Carrusel de logos (hover / touch pause)
- Prueba social por servicio

Cada Swiper irá encapsulado en `/components/sliders`.

---

## 9. Página Home – Secciones

### a) Topbar

- Redes sociales (Facebook, LinkedIn, X, YouTube)
- Teléfono (`tel:`)
- Correo (`mailto:`)
- Horarios
- Todo con iconos

### b) Navbar

- Menú principal
- Dropdowns por categoría

### c) Slider principal (Swiper)

- Imagen background cover
- Isologo + línea vertical
- Título
- Descripción (promoción / descuento)
- CTA primario / secundario

### d) Acerca de [empresa]

- Imagen animada (30s)
- Badge +10 años
- Texto descriptivo
- Beneficios con iconos

### e) Carrusel de logos

- Autoplay
- Pause on hover / touch

### f) Soluciones integrales

- 10 cards
- Imagen + título + descripción
- Enlace + CTA

### g) ¿Por qué elegirnos?

- Imagen animada (antes / después)
- Beneficios clave

---

## 10. Páginas de servicio (`/servicios/[slug].astro`)

Incluyen:

- Header con breadcrumb
- Prueba social (logos)
- Certificaciones y alianzas
- Paso a paso del servicio
- Garantías
- FAQ (máx 5)
- Módulo integrador de servicios (interactivo)

El módulo integrador actualizará contenido dinámicamente (estado local en Astro + JS).

---

## 11. Delegación de tareas

### Sr - MSANCHEZ

- Inicializar proyecto [done]
- Configuración Astro [done]
- Arquitectura de carpetas [done]
- Revisión de PRs [pending]

### Dev - LSILVA (50%)

- Tailwind base + variables [pending]
- Topbar
- Navbar + dropdowns
- Slider principal
- Carrusel de logos
- Animaciones base

### Dev - ADORANTES (50%)

- Animaciones con Tailwind [pending]
- Sección Acerca de
- Soluciones integrales (cards)
- ¿Por qué elegirnos?
- Plantilla de páginas de servicio
- FAQ + garantías

---

## 12. Git workflow

- `main` → producción
- `develop` → integración
- `feature/*` → trabajo individual

Ejemplo:

```
feature/topbar
feature/home-slider
```

### Convención de commits

- `init:` inicialización
- `feat:` nueva funcionalidad
- `fix:` corrección
- `style:` estilos
- `doc:` documentación
- `chore:` mantenimiento

Ejemplo:

```
feat: add home slider swiper
style: adjust navbar spacing
```

---

## 13. Deploy a cPanel

```bash
npm run build
```

Subir **contenido de `/dist`** a:

```
public_html/
```

---

## 14. Cierre

Este documento define el **estándar oficial del proyecto**.
Cualquier cambio de stack o arquitectura debe ser aprobado por el Dev Sr.
