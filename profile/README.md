# Sprint 4 – SIMS Project: Organization Hub

## Overview

En este sprint, tomamos todo lo aprendido durante el **Sprint 3** y lo aplicamos para construir una aplicación más robusta, escalable y profesional. El objetivo principal es modernizar la infraestructura hacia un ecosistema moderno de stack completo:

- **Frontend**: **Vue**, **TypeScript** y **Tailwind CSS**.
- **Backend**: **Laravel**.
- **Base de Datos**: **PostgreSQL**.

---

## Decisiones de Arquitectura y Estrategia

### La Elección del Frontend (Implementación de Sabina)
Hemos adoptado oficialmente la arquitectura de frontend propuesta por **Sabina**. Esta decisión se tomó para garantizar:
* **Lógica Modular:** Uso de un enfoque basado en `composables` personalizados para la gestión del estado de forma reutilizable.
* **Seguridad de Tipado:** Integración estricta de TypeScript para reducir errores en tiempo de ejecución.
* **Consistencia de UI:** Implementación nativa de Tailwind CSS que facilita la consistencia visual en nuestro entorno.

### La Elección del Backend (Implementación de Joel)
El núcleo del backend desarrollado por **Joel** ha sido seleccionado como el estándar de la organización debido a:
* **Seguridad:** Integración nativa de Laravel Sanctum para la autenticación segura de la API y gestión de tokens.
* **RBAC Avanzado:** Implementación robusta de Spatie para el control granular de permisos y roles.

### Arquitectura Multi-Tenant (SaaS B2B / B2G)
La aplicación utiliza una **arquitectura multi-tenant** para:
- Centralizar la gestión de clientes (tenants).
- Mejorar la escalabilidad.
- Simplificar el mantenimiento y los despliegues.

*Cada tenant está lógicamente aislado mientras comparte la misma infraestructura de base de datos.*

### Single-Page Application (SPA)
El frontend se implementa como una **Single-Page Application (SPA)**. 

#### Implicaciones
Dado que el uso de una SPA hace que la página no sea indexable por Google por defecto, necesitaremos desarrollar una **Landing page** renderizada en el lado del servidor para poder ser indexados correctamente.

---

## Resumen del Tech Stack

# Frontend

# Librerías usadas y por qué las elegimos

Breve resumen de cada dependencia del proyecto y el motivo de su elección.

## Dependencias (runtime)

- vue
  - Para qué sirve: Framework principal (UI, reactividad, Composition API).
  - Por qué: Ecosistema maduro, rendimiento y compatibilidad con TypeScript.

- vue-router
  - Para qué sirve: Enrutado de la SPA.
  - Por qué: Integración nativa con Vue 3 y soporte de rutas anidadas y lazy-loading.

- pinia
  - Para qué sirve: Gestión de estado global.
  - Por qué: API simple, orientada a TypeScript y recomendada por la comunidad Vue.

- axios
  - Para qué sirve: Cliente HTTP para llamadas a API.
  - Por qué: Manejo sencillo de interceptores, timeouts y respuestas; más ergonomía que fetch en casos complejos.

- @tanstack/vue-query
  - Para qué sirve: Gestión y cache de datos remotos (server state).
  - Por qué: Sincronización automática, cache, reintentos y políticas de refetch robustas.

- @tanstack/vue-table
  - Para qué sirve: Helpers y hooks para construir tablas avanzadas.
  - Por qué: Flexibilidad y rendimiento para tablas con paginación, sorting y filtrado.

- @vueuse/core
  - Para qué sirve: Colección de utilidades reactivas reutilizables.
  - Por qué: Evita reinventar hooks comunes (debounce, localStorage, etc.).

- zod
  - Para qué sirve: Validación y parsing de datos en runtime con inferencia de tipos.
  - Por qué: Seguridad al validar payloads y generación de tipos TypeScript a partir de esquemas.

- pinia
  - (ya descrito arriba)

- lucide-vue-next
  - Para qué sirve: Iconos SVG listos para Vue.
  - Por qué: Set ligero y consistente visualmente.

- radix-vue
  - Para qué sirve: Componentes accesibles de bajo nivel (primitives).
  - Por qué: Facilita accesibilidad y control cuando se necesita construir componentes personalizados.

- reka-ui
  - Para qué sirve: Biblioteca de componentes (UI kit) usada en el proyecto.
  - Por qué: Acelerador de desarrollo con componentes reutilizables del equipo/proyecto.

- tailwind-merge
  - Para qué sirve: Mezclar clases Tailwind evitando duplicados/conflictos.
  - Por qué: Útil en utilidades y componentes que combinan clases dinámicamente.

- tailwindcss-animate
  - Para qué sirve: Clases de animación listas para usar con Tailwind.
  - Por qué: Consistencia y facilidad para animaciones cotidianas.

- vue-sonner
  - Para qué sirve: Toasters/notifications para Vue.
  - Por qué: Integración simple y estilizada para notificaciones UX.

- vue3-cookies
  - Para qué sirve: API para leer/escribir cookies en Vue 3.
  - Por qué: Manejo sencillo de cookies (sesión, token, preferencias).

- class-variance-authority
  - Para qué sirve: Definir variantes de clases CSS de forma tipada.
  - Por qué: Facilita construir componentes con variantes (size, color) de forma segura.

- clsx
  - Para qué sirve: Concatenar clases condicionalmente.
  - Por qué: Sintaxis ligera y eficiente para clases dinámicas.

## DevDependencies (herramientas de desarrollo)

- vite / @vitejs/plugin-vue
  - Para qué sirve: Bundler/dev server rápido para Vue 3.
  - Por qué: Experiencia de desarrollo muy rápida y build optimizado.

- typescript / vue-tsc
  - Para qué sirve: Tipado estático y verificación de tipos para Vue.
  - Por qué: Mayor seguridad y detección temprana de errores.

- tailwindcss / @tailwindcss/postcss / tailwindcss-animate (dev)
  - Para qué sirve: Framework CSS utilitario y pipeline PostCSS.
  - Por qué: Velocidad para crear UI consistentes y responsive.

- npm-run-all2
  - Para qué sirve: Ejecutar múltiples scripts npm en paralelo/serie.
  - Por qué: Orquestar pasos de build y type-check de forma simple.

- @tsconfig/node24, @vue/tsconfig, @types/node
  - Para qué sirve: Presets y tipos para TypeScript.
  - Por qué: Configuración consistente y tipos de Node para herramientas.

- vite-plugin-vue-devtools
  - Para qué sirve: Integración con devtools extendidos en desarrollo.
  - Por qué: Mejora la depuración de componentes en local.

## Notas finales
- Se eligieron librerías que favorecen productividad, tipado y experiencia de desarrollador (Vite, TS, Pinia).
- Para manejo de datos remotos y cache se prefirió @tanstack/vue-query por sus garantías y lógica out-of-the-box.
- Para UI se combinan utilidades (Tailwind, clsx, class-variance-authority) con primitives accesibles (radix-vue) y un kit propio (reka-ui) para acelerar el desarrollo consistente.

Si quieres, creo un README más breve por módulo (auth, tickets, users) explicando qué librerías usa cada uno.

### Backend
- **Laravel**

#### Librerías
- **Sanctum:** Añade tokens y se encarga de la seguridad y autenticación de Laravel.
- **Spatie (laravel-permission):** Forma fácil y robusta de gestionar roles y permisos. Implementa un sistema de Control de Acceso Basado en Roles (RBAC) que nos permite asignar roles (Admin, Client, Maintenance) y permisos granulares (ej. `can.activate.reservation`). 
- **nesbot/carbon:** Extensión para el manejo de fechas y horas en PHP. Crucial en el `ReservationController` para operaciones sensibles al tiempo, como calcular la duración exacta de los viajes (`diffInMinutes`) para el coste final, y gestionar la caducidad de reservas.

### Base de Datos
- **PostgreSQL**

---

## Repositorios del Proyecto

El código está dividido en dos repositorios principales. Consulta el `README.md` de cada uno para ver su **estructura de directorios** específica:

* [**Repositorio Frontend **](./ruta-al-repo-frontend) 
* [**Repositorio Backend **](./ruta-al-repo-backend) 

---

## Convenciones de Código Globales

Para garantizar la consistencia y legibilidad en todo el proyecto, se deben seguir las siguientes convenciones:

- **Composables**: `useComposableName.ts`
- **Componentes**: `PascalCase.vue`
- **Clases**: `PascalCase`
- **Rutas**: `kebab-case`
- **Variables, funciones y otros**: `camelCase`

### Reglas Generales
- Todos los comentarios, nombres de archivos y el contenido de los mismos **deben estar escritos en inglés**.
- Por favor, mantén el código limpio, legible y bien estructurado.

---

## Flujo de Trabajo y Commits

Consulta el archivo `CONTRIBUTING.md` en cada repositorio para ver el **procedimiento detallado de ramas (Git Flow)**. 

### Mensajes de Commit
Usa mensajes de commit concisos y significativos con los siguientes prefijos:

- **Fix**: `Fix: Fixed the users CRUD`
- **Feat**: `Feat: Added users CRUD to the backend`
- **Refactor**: `Refactor: Improved authentication logic`
