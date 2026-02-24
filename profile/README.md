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

##  Tecnologías base

- **Vue 3 + TypeScript** → Framework progresivo con tipado fuerte, ideal para construir interfaces reactivas y escalables.  
- **Tailwind CSS** → Framework de utilidades CSS que permite crear diseños consistentes y modernos de forma ágil.

---

##  Librerías principales

- **vue3-toastify** → Manejo de notificaciones mediante un *composable* `useToast`, que estandariza el aspecto y comportamiento de los mensajes.  
- **@headlessui/vue** → Componentes accesibles y sin estilos predefinidos (*headless*), fáciles de personalizar con Tailwind.  
- **@heroicons/vue** → Conjunto de íconos SVG personalizables y optimizados para Vue.  
- **Leaflet (leaflet.js + leaflet.css)** → Librería ligera para mapas interactivos; usada para renderizar el mapa principal, gestionar marcadores dinámicos y controlar interacciones (zoom, pan).  
- **OpenStreetMap (Tile Provider)** → Fuente de datos de mapas abiertos utilizada como capa base de Leaflet, evitando la dependencia de servicios propietarios como Google Maps.

---

## Dependencias adicionales

Estas dependencias complementan el ecosistema de Vue y mejoran la arquitectura, la gestión de datos y la interfaz de usuario:

###  Gestión de estado y datos
- `pinia`  
- `@tanstack/vue-query`

### Ruteo y composición
- `vue-router`  
- `@vueuse/core`

###  Validación y utilidades
- `zod`  
- `clsx`  
- `class-variance-authority`  
- `tailwind-merge`

###  UI y componentes reutilizables
- `radix-vue`  
- `reka-ui`  
- `lucide-vue-next`  
- `vue-sonner`  
- `tailwindcss-animate`

###  HTTP y cookies
- `axios`  
- `vue3-cookies`

###  Tablas dinámicas
- `@tanstack/vue-table`

---

## Resumen

Este stack permite un desarrollo **ágil, modular y mantenible**, con:
- Interfaces reactivas y personalizables  
- Manejo eficiente de datos y estado  
- Validaciones robustas  
- Integración fluida con TailwindCSS y librerías UI modernas  
- Notificaciones y mapas interactivos en tiempo real

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
