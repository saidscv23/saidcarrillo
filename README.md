# 🚀 Portafolio Personal - Said Carrillo

¡Bienvenido al código fuente de mi portafolio web personal! Este proyecto es una presentación interactiva, rápida y moderna de mi perfil profesional, proyectos y habilidades como Desarrollador Full Stack. 

## 📝 Descripción General

Este proyecto consiste en una **Single Page Application (SPA) / Landing Page** rápida y optimizada, diseñada para destacar mis habilidades, mi experiencia tecnológica, y los proyectos en los que he trabajado. Está diseñada priorizando el rendimiento, la accesibilidad y una estética moderna (con un modo oscuro por defecto y un modo claro alternativo).

## 🛠 Tecnologías Utilizadas

- **Astro**: El framework principal elegido por su extrema velocidad y envío mínimo (o nulo) de JavaScript al cliente por defecto.
- **Tailwind CSS (v4)**: Framework utilitario avanzado. Integrado a través de Vite, permite estilos rápidos, diseño puramente responsivo y variables CSS nativas.
- **JavaScript (Vanilla)**: Utilizado puntualmente (sin frameworks pesados como React o Vue) para:
  - Observador de elementos (`IntersectionObserver`) que ejecuta animaciones de "fade-in" al scrollear.
  - Alternancia dinámica de modo oscuro a claro.
- **SVG en línea**: Para los íconos (como el del sol y la luna, o logos tecnológicos), asegurando que carguen al instante y puedan ser estilizados con Tailwind.

---

## 📦 Instalación y Ejecución Local

Para clonar y correr este proyecto de manera local, necesitas [Node.js](https://nodejs.org/) instalado en tu máquina.

1. **Clona el repositorio:**
   ```bash
   git clone https://github.com/saidscv23/saidcarrillo.git
   cd saidcarrillo
   ```

2. **Instala las dependencias:**
   ```bash
   npm install
   ```
   *(Si utilizas pnpm, `pnpm install`, o yarn, `yarn install`)*

3. **Ejecuta el servidor de desarrollo:**
   ```bash
   npm run dev
   ```

4. **Visita el proyecto:**  
   Abre tu navegador en `http://localhost:4321/` para ver el portafolio en acción.

---

## 📂 Estructura de Carpetas y Archivos

El ecosistema de carpetas sigue las convenciones predeterminadas de **Astro**, promoviendo un entorno escalable y ordenado.

```text
/
├── public/                 # Contiene recursos estáticos (favicon.svg, imágenes base, ico). No se procesan en el build.
├── src/                    # Aquí vive el código fuente procesado por Astro.
│   ├── components/         # Componentes UI reutilizables e independientes.
│   ├── layouts/            # Componentes de envoltura (layout principal, HTML shell).
│   ├── pages/              # Define las rutas principales del sitio (index.astro, cv.astro).
│   └── styles/             # Archivos CSS globales y configuración de variables CSS (global.css).
├── astro.config.mjs        # Archivo de configuración global del framework Astro (plugins como Tailwind).
├── package.json            # Dependencias del ecosistema Node y scripts.
├── tsconfig.json           # Configuración de TypeScript y aliasing de Astro.
└── README.md               # Este archivo.
```

- `src/pages/`: Cada archivo `.astro` aquí se convierte automáticamente en una ruta. Ejemplo: `index.astro` es la raíz (`/`).
- `src/components/`: Piezas modulares e independientes del rompecabezas UI (Hero, Header, etc.).
- `src/layouts/`: Plantillas que aseguran que componentes como las etiquetas `<head>`, fuentes y `<body>` sean consistentes entre múltiples páginas.
- `src/styles/`: Centraliza hojas de estilo en bruto. En este proyecto se destaca `global.css` como el gestor principal del tema y animaciones.

---

## 🧩 Uso de Componentes Principales

- **`Header.astro`**: Contiene la navegación superior. Mantiene una posición `fixed` con efecto *glassmorphism* (`backdrop-blur`). Incluye los anclajes de navegación hacia secciones e integra el botón de cambio de tema (toggle claro/oscuro).
- **`Hero.astro`**: La primera gran sección visible. Suele tener animaciones escalonadas y una llamada a la acción principal fuerte.
- **`Skills.astro` / `TechStack.astro`**: Renderizan las parrillas o carruseles (marquees) con las habilidades y las tecnologías correspondientes.
- **`Projects.astro`**: La galería de proyectos recientes, en formato de tarjeta (card) visualmente atractiva.
- **`Contact.astro`**: Provee correo, links de contacto directos, y redes sociales para entablar comunicación.
- **`Layout.astro`**: Componente primordial que recibe propiedades `title` y `description` (útiles para SEO), ensambla el HTML y define el script global contra el parpadeo de tema (FOUC).

---

## 🌓 Alternancia de Modo Claro / Oscuro

El sistema de tema oscuro/claro está construido eficientemente sin librerías externas para evitar parpadeos no deseados de estilo durante la carga de página (FOUC - *Flash Of Unstyled Content*).

**¿Cómo funciona?**

1. **Pre-renderizado en `Layout.astro`:**  
   En la etiqueta `<head>`, un script bloqueante e *inline* lee el valor `theme` almacenado del `localStorage` o desde la preferencia del sistema operativo del usuario `(prefers-color-scheme)`. Dependiendo de este, añade (o no) una clase `.light` a la etiqueta `<html>`.

2. **Vars de CSS en `global.css`:**  
   En modo oscuro (default), los colores funcionan directo desde las paletas de Tailwind. Cuando Astro adjunta la clase `.light` el CSS sobrescribe las principales variables `color-gray-*`, invirtiendo negros por blancos y modificando acentos como el `color-cyan-*`.

3. **Lógica de Toggling (`Header.astro`):**  
   Al hacer clic sobre el botón en el menú, un pequeño manejador de eventos en el cliente:
   - Añade temporalmente una clase `.theme-transition` al HTML para suavizar los colores (no instantáneo).
   - Verifica si existe `.light`. Si lo tiene, vuelve a estado Oscuro; si no, lo pone Claro.
   - Actualiza el estado dentro de `localStorage` para la próxima vez que el usuario regrese.

---

## 💡 Buenas Prácticas y Arquitectura Recomendada en Astro

Si vas a tomar este proyecto como base, mantén presentes los siguientes principios arquitectónicos de Astro:

- **Arquitectura de Islas (Island Architecture):** Procura mantener los componentes estáticos y solo añadir JavaScript a componentes que estrictamente necesiten reactividad. Usa directivas de Astro como `client:idle`, `client:load` o `client:visible` sólo cuando tengas un componente de framework (React, Svelte, etc.) que requiera hidratar JavaScript interactivo.
- **Uso de Scoped CSS:** Dentro de tus componentes `.astro`, puedes añadir `<style>` y afectará únicamente a ese componente, previniendo choques globales (CSS encapsulado).
- **Semántica HTML & Accesibilidad (a11y):** Los botones interactivos de íconos, como el del cambio de tema, deben tener un atributo `aria-label` para su lectura por sistemas de asistencia. 
- **Límites de Componentes:** Mantén los componentes funcionales, simples y orientados a la vista. Trata de mantener un componente haciendo exactamente una cosa.

¡Disfruta el código! Si encuentras algún bug o tienes una sugerencia, siéntete libre de notificarlo.
