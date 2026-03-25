# Sprint 5 – SIMS Project: Organization Hub (First Deployment)



*(Taula de capçalera: Professorat: Xavi , Joan Iglesias, Maria Merino | Data: 23/03/2026 | Curs: DAW 2)*



## Overview



Este es el repositorio central de nuestra organización para el Sprint 5. El objetivo principal de esta entrega es pasar de los prototipos que teníamos a una **versión Alfa completa y desplegada**. 



Para conseguirlo, hemos montado una infraestructura moderna con el siguiente stack:

- **Frontend**: Vue 3, TypeScript y Tailwind CSS.

- **Backend**: Laravel (PHP).

- **Base de Datos**: PostgreSQL.

- **IoT**: Subsistema basado en Python.



---



## 1. Reestructuración del Proyecto



Como en este sprint se han rehecho los grupos, lo primero que hicimos fue evaluar qué código base aprovechar. Al final decidimos unificar el proyecto usando las partes más completas que teníamos:



* **El Frontend de Sabina:** Nos hemos quedado con su arquitectura porque ya tenía una lógica muy modular (basada en `composables`), el tipado con TypeScript estaba muy bien integrado y visualmente era muy consistente gracias a Tailwind.

* **El Backend de Joel:** Lo hemos elegido como estándar porque ya tenía implementado de forma muy robusta todo el tema de seguridad (Sanctum) y el sistema de roles y permisos con Spatie.



### Arquitectura Multi-Tenant y SPA

Nuestra aplicación funciona con una **arquitectura multi-tenant**. Esto nos permite centralizar la gestión de clientes: cada "tenant" (empresa/cliente) está aislado lógicamente, pero todos comparten la misma infraestructura de base de datos, lo que nos facilita mucho el mantenimiento.



**El uso de Esquemas en PostgreSQL:** Para lograr este aislamiento sin mezclar los datos, utilizamos **esquemas (schemas) nativos de PostgreSQL**. Tenemos un esquema central (public) para la gestión general, y luego cada cliente tiene su propio esquema dedicado (ej. `tenant_empresa1`). De esta forma, los datos de los vehículos, reservas y usuarios de un cliente son inaccesibles para el resto, garantizando máxima seguridad y rendimiento.



El frontend se implementa como una **Single-Page Application (SPA)**. Como las SPA tienen el problema de que Google no las indexa bien, tenemos previsto hacer una Landing page renderizada desde el servidor para solucionar el tema del SEO.



---



## 2. Decisiones Técnicas del Sprint 5 (Investigación)



Tal y como se pedía en los requisitos, aquí explicamos por qué hemos elegido ciertas herramientas clave para esta fase:



### Gestión de estado (State Management): ¿Por qué Pinia?

Para centralizar los datos en el frontend y no freír el servidor a peticiones, estuvimos mirando Vuex, Redux y Pinia. 

* Redux lo descartamos porque es demasiado complejo y se usa más en React. 

* Vuex era el estándar antes, pero te obliga a usar "mutaciones", lo que hace que escribas mucho código repetitivo. 

* **Nos quedamos con Pinia** porque es el estándar oficial actual para Vue 3. Es mucho más simple, funciona perfecto con TypeScript (nos autocompleta los tipos sin configurar nada) y encaja genial con la Composition API que ya estábamos usando.



### Depuración en el Backend: ¿Por qué Laravel Telescope?

Para depurar los errores de la aplicación estuvimos comparando Xdebug, Sentry y Telescope. 

Xdebug está muy bien para ir línea por línea en local, y Sentry es brutal para ver errores de usuarios en producción. Sin embargo, para esta fase Alfa, **hemos elegido Laravel Telescope**. Nos da un panel dentro del propio Laravel donde podemos ver de un vistazo todas las peticiones HTTP, las consultas a la base de datos (para ver si alguna va lenta) y los Webhooks que nos llegan del IoT.



👉 **[En este enlace se pueden ver capturas de como gestionabamos los errores en el logs y las peticiones con laravel telescope](https://drive.google.com/drive/folders/1uz6ih19r50wEPR5ZbtAKLgnkmbbpL2Ld?usp=sharing)**



---



## 3. Usabilidad, Tests y Rendimiento



### Mejoras de Interfaz (Filosofía de Steve Krug)

Hemos intentado aplicar el concepto de "No me hagas pensar" de Steve Krug para que la web sea más intuitiva. Algunos de los cambios que hemos metido son:

* Mejora 1: Usamos una bottombar en vez de una sidebar para que sea mas atractivo e intuitivo para el usuario*

* Mejora 2: Usamos colores semánticos (verde/rojo) para que el estado de los vehículos se entienda sin leer*

* Mejora 3: El usuario puede hacer una misma cosa desde diferentes partes de la aplicacion



### Tests Automatizados y CI/CD

Para asegurar la calidad del proyecto, hemos integrado **GitHub Actions**. Este flujo actúa como nuestro test automatizado principal: cada vez que hacemos un cambio, el sistema verifica que todo el código esté correcto antes de permitir el despliegue, evitando errores en el servidor.



### Tests de Usabilidad

Le hemos pedido a varias personas que no conocían la aplicación (amigos y familiares) que intenten usarla. Les hemos asignado roles y hemos grabado la pantalla y sus voces para ver dónde se atascaban.

* 📄 **[Enlace al Informe de Usabilidad en PDF]**

* 🎥 **[Enlace a la carpeta con los vídeos de las pruebas]**
https://drive.google.com/drive/folders/1_VtOPgNk3cQiT9LCuPMqPOzwWEA-Whtt?usp=sharing


### Rendimiento

Hemos pasado el "Lighthouse" de Chrome para analizar la velocidad de la página. 

* **Mejoras aplicadas:** *[Añadir qué habéis tocado, ej: Hemos optimizado el peso de las imágenes y configurado lazy-loading para mejorar el tiempo de carga].*



---



## 4. Funcionalidades, Roles y Chatbot IA



El sistema distingue entre varios tipos de usuarios:

1. **Superadmin:** Controla toda la plataforma y los Tenants.

2. **Tenant Admin:** Gestiona la flota y los usuarios de su propia empresa.

3. **Tenant Worker:** Se encarga del mantenimiento y las incidencias.

4. **Final User:** El cliente que alquila el vehículo.



👉 **[En este Google Sheets tenéis la matriz completa con todas las funcionalidades por rol y los componentes que compartimos](https://docs.google.com/spreadsheets/d/1s2lJcBCdCCeTxu5F6PwWKuIzcNNT5hhBrhVcwPwEjhM/edit?usp=sharing)**



### Sección de Ayuda (Chatbot RAG)

Hemos decidido no tocar el chatbot en este sprint porque queremos priorizar otros aspectos de la applicación que nos han requerido mas tiempo.



---



## 5. Estado del Subsistema IoT



Para dejar claro en qué punto estamos con el hardware:

* **Estado Actual:** *[Enlace al diagrama de diseño y al modelo de datos JSON]*

* **Sensores elegidos:** *[Listar sensores, ej: Hemos decidido tirar adelante con el módulo GPS para la posición]*

* **Actuador ON/OFF:** Lo hemos implementado con *[explicar brevemente el relé o la placa]* que recibe las órdenes.

* **Comunicación:** El frontend no toca el hardware directamente. Laravel hace de puente comunicándose con el subsistema mediante API y Webhooks. El objetivo de este sprint es dejarlo 100% operativo para que los de automoción puedan integrarlo en el siguiente sprint.



---



## 6. Despliegue (First Deployment)



Ya tenemos la versión Alfa subida a internet y accesible públicamente. 



Para el despliegue hemos utilizado un servidor VPS en **DigitalOcean**. Para mantener todo organizado y escalable, hemos orquestado la infraestructura usando **Docker Compose**, levantando los servicios en contenedores aislados:

* **Contenedor 1:** Base de datos (PostgreSQL).

* **Contenedor 2:** Backend (Laravel).

* **Contenedor 3:** Frontend (Vue 3).



Además, ya tenemos configurado nuestro dominio oficial en producción. Podéis acceder a la plataforma a través de:

🌐 **https://voltiacar.live**



*Automatización Multitenant:* Hemos configurado el sistema para que, al registrar un nuevo tenant, se ejecute automáticamente la creación de su **esquema en PostgreSQL** y sus migraciones correspondientes, agilizando al máximo la entrada de nuevos clientes.



📖 **[Enlace a nuestro Manual de Despliegue]**



---



## 7. Nuestro Tech Stack (Librerías y Justificación)



Aquí dejamos documentado al detalle todo el stack que usamos y el porqué de cada dependencia, algo que ya consolidamos en el sprint anterior.



### Frontend



**Dependencias principales (runtime):**

- `vue`: Framework principal (UI, reactividad, Composition API). Ecosistema maduro, rendimiento y compatibilidad con TypeScript.

- `vue-router`: Enrutado de la SPA. Integración nativa con Vue 3 y soporte de rutas anidadas.

- `pinia`: Gestión de estado global (explicado arriba).

- `axios`: Cliente HTTP para llamadas a la API. Nos facilita el manejo de interceptores y timeouts respecto a *fetch*.

- `@tanstack/vue-query`: Gestión y caché de datos remotos (server state). Nos ahorra mucho trabajo sincronizando datos y haciendo reintentos.

- `@tanstack/vue-table`: Helpers para construir tablas avanzadas con paginación y filtros.

- `@vueuse/core`: Colección de utilidades reactivas (para no reinventar la rueda con cosas como localStorage).

- `zod`: Validación de datos en runtime para ir seguros con los payloads.

- `lucide-vue-next`: Iconos SVG listos para Vue.

- `radix-vue`: Componentes accesibles de bajo nivel.

- `reka-ui`: Biblioteca de componentes (UI kit) que usamos en el proyecto para ir más rápido.

- `tailwind-merge` y `clsx`: Para mezclar y concatenar clases de Tailwind sin que haya conflictos.

- `tailwindcss-animate`: Clases de animación.

- `vue-sonner`: Para los toasters/notificaciones de la interfaz.

- `vue3-cookies`: Manejo sencillo de cookies de sesión/preferencias.

- `class-variance-authority`: Para definir variantes de clases CSS de forma tipada.



**DevDependencies (herramientas de desarrollo):**

- `vite` / `@vitejs/plugin-vue`: Nuestro bundler. Compila rapidísimo.

- `typescript` / `vue-tsc`: Tipado estático.

- `tailwindcss` / `@tailwindcss/postcss`: Framework CSS.

- `npm-run-all2`: Para ejecutar varios scripts de npm a la vez.

- `vite-plugin-vue-devtools`: Nos integra las devtools directamente en el entorno local.



*En resumen: Elegimos librerías que nos den productividad y un buen tipado. Para los datos remotos nos fiamos de vue-query, y para la UI combinamos Tailwind con radix-vue y reka-ui para tener componentes accesibles y consistentes.*



### Backend

- **Framework:** Laravel

- **Base de Datos:** PostgreSQL



**Librerías principales:**

- **Sanctum:** Nos gestiona los tokens y la seguridad de la API.

- **Spatie (laravel-permission):** Nos permite montar el sistema de Control de Acceso Basado en Roles (RBAC) con permisos granulares (ej. `can.activate.reservation`). 

- **nesbot/carbon:** Básico para el manejo de fechas en PHP. Lo usamos a tope en el `ReservationController` para calcular la duración exacta de los viajes (`diffInMinutes`), cobrar lo que toca y gestionar cuándo caducan las reservas.



---



## 8. Estructura de Repositorios y Reglas



Tenemos el código separado en tres repositorios dentro de esta misma organización. En el `README.md` de cada uno está la estructura de carpetas:



* 📂 [Repositorio Frontend](https://github.com/Sprint5-ProjectSIMS-Team3/sims-frontend) 

* 📂 [Repositorio Backend](https://github.com/Sprint5-ProjectSIMS-Team3/sims-backend) 

* 📂 [Repositorio Subsistema IoT](https://github.com/Sprint5-ProjectSIMS-Team3/sims-subsystem)



### Gobernanza

* Hemos añadido un **Contributor Covenant** en todos los repositorios.

* El proyecto está bajo la licencia **EUPL**.



### Convenciones de Código

Todos intentamos seguir estas reglas para mantener el código limpio:

- **Composables**: `useComposableName.ts`

- **Componentes**: `PascalCase.vue`

- **Clases**: `PascalCase`

- **Rutas**: `kebab-case`

- **Variables y funciones**: `camelCase`

- **Idioma:** Todo el código, nombres de archivos y comentarios **deben estar en inglés**.



### Flujo de Trabajo (Git Flow)

Consultad el `CONTRIBUTING.md` de cada repo para ver cómo gestionamos las ramas. Para los commits, usamos estos prefijos:

- `Fix: Fixed the users CRUD`

- `Feat: Added users CRUD to the backend`

- `Refactor: Improved authentication logic` esto?
