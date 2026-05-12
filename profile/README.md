# Sprint 5 – SIMS Project: Organization Hub (First Deployment)

Professorat: Xavi, Joan Iglesias, Maria Merino | Date: 12/05/2026 | Course: DAW 2

## Overview

This is the central repository of our organization for Sprint 5. The main objective of this delivery is to move from the prototypes we previously had to a **fully deployed Alpha version**.

To achieve this, we have set up a modern infrastructure using the following stack:

- **Frontend**: Vue 3, TypeScript, and Tailwind CSS.
- **Backend**: Laravel (PHP).
- **Database**: PostgreSQL.
- **IoT**: Python-based subsystem.

---

## 1. Project Restructuring

Since the groups were reorganized for this sprint, the first thing we did was evaluate which codebase we should keep. In the end, we decided to unify the project using the most complete parts we already had:

* **Sabina's Frontend:** We kept her architecture because it already had a very modular logic structure (based on `composables`), TypeScript typing was very well integrated, and visually it was highly consistent thanks to Tailwind.

* **Joel's Backend:** We selected it as the standard because it already had a very robust implementation of security (Sanctum) and the roles and permissions system using Spatie.

### Multi-Tenant and SPA Architecture

Our application works with a **multi-tenant architecture**. This allows us to centralize client management: each "tenant" (company/client) is logically isolated, while all tenants share the same database infrastructure, making maintenance much easier.

**Using PostgreSQL Schemas:** To achieve this isolation without mixing data, we use native **PostgreSQL schemas**. We have a central schema (`public`) for general management, and then each client has its own dedicated schema (e.g. `tenant_company1`). This way, vehicle, reservation, and user data from one client are inaccessible to others, ensuring maximum security and performance.

The frontend is implemented as a **Single-Page Application (SPA)**. Since SPAs are not indexed efficiently by Google, we plan to create a server-rendered landing page to solve SEO-related issues.

---

## 2. Sprint 5 Technical Decisions (Research)

As required, here we explain why we chose certain key tools for this phase:

### State Management: Why Pinia?

To centralize frontend data and avoid overloading the server with requests, we evaluated Vuex, Redux, and Pinia.

* We discarded Redux because it is too complex and is mainly used in React.
* Vuex used to be the standard, but it forces the use of "mutations", which results in a lot of repetitive code.
* **We chose Pinia** because it is the current official standard for Vue 3. It is much simpler, works perfectly with TypeScript (automatic type inference without extra configuration), and integrates very well with the Composition API we were already using.

### Backend Debugging: Why Laravel Telescope?

To debug application errors, we compared Xdebug, Sentry, and Telescope.

Xdebug is excellent for line-by-line debugging locally, and Sentry is very powerful for monitoring production errors. However, for this Alpha phase, **we chose Laravel Telescope**.

It provides a dashboard directly inside Laravel where we can easily inspect all HTTP requests, database queries (to detect slow queries), and IoT webhooks received by the system.

👉 **[At this link you can see screenshots of how we managed logs and requests using Laravel Telescope](https://drive.google.com/drive/folders/1uz6ih19r50wEPR5ZbtAKLgnkmbbpL2Ld?usp=sharing)**

---

## 3. Usability, Testing, and Performance

### Interface Improvements (Steve Krug Philosophy)

We tried to apply Steve Krug's "Don't Make Me Think" concept to make the website more intuitive. Some of the implemented improvements are:

* Improvement 1: We use a bottom bar instead of a sidebar to make the interface more attractive and intuitive for users.
* Improvement 2: We use semantic colors (green/red) so vehicle status can be understood without reading text.
* Improvement 3: Users can perform the same action from different parts of the application.

### Automated Testing and CI/CD

To ensure project quality, we integrated **GitHub Actions**. This workflow acts as our main automated testing system: every time we make changes, the system verifies that the code is correct before deployment is allowed, preventing server-side issues.

### Usability Tests

We asked several people unfamiliar with the application (friends and family) to use it. We assigned them roles and recorded both their screens and voices to identify where they struggled.

* 📄 **[Link to the Usability Report PDF]**
https://drive.google.com/drive/folders/1SP4SLB6FkmM61rTalqUYHAbq_bnad4PO?usp=drive_link

* 🎥 **[Link to the folder containing the test videos]**
https://drive.google.com/drive/folders/1_VtOPgNk3cQiT9LCuPMqPOzwWEA-Whtt?usp=sharing

### Performance

We used Chrome Lighthouse to analyze page speed.

* **Implemented improvements:** *[Add what you optimized here, e.g.: We optimized image sizes and configured lazy-loading to improve loading times].*

---

## 4. Features, Roles, and AI Chatbot

The system distinguishes between several user types:

1. **Superadmin:** Controls the entire platform and all tenants.
2. **Tenant Admin:** Manages the fleet and users within their own company.
3. **Tenant Worker:** Handles maintenance and incidents.
4. **Final User:** The customer who rents the vehicle.

👉 **[In this Google Sheets document you can find the complete feature matrix by role and the shared components](https://docs.google.com/spreadsheets/d/1s2lJcBCdCCeTxu5F6PwWKuIzcNNT5hhBrhVcwPwEjhM/edit?usp=sharing)**

### Help Section (RAG Chatbot)

We decided not to work on the chatbot during this sprint because we wanted to prioritize other aspects of the application that required more development time.

---

## 5. IoT Subsystem Status

To clearly define the current state of the hardware:

* **Selected sensors:** *[GPS sensor, camera, and relay]*

* **Current Status:** *[Camera: The camera is currently capable of taking photos and displaying them in the designated folder.]*
* **Current Status:** *[The relay can be turned on and off from the client-side application. Requests are sent through an ngrok tunnel.]*
* **Current Status:** *[GPS Sensor: Currently, GPS sensor data is managed by storing coordinates in a MongoDB Atlas database. The backend does not directly access the database; instead, it communicates through a dedicated microservice. This microservice acts as an intermediary, querying MongoDB Atlas, retrieving the latest coordinates, and returning them to the backend for processing or visualization.]*

* **Pending:** *[Camera: The remaining task is to allow the camera to scan the QR code generated by the frontend using backend data. Once scanned, the Raspberry Pi will send the code back to the backend to verify the reservation and activate the vehicle.]*
* **Pending:** *[The Raspberry Pi must upload the current relay state to MongoDB in order to later use it in a security procedure that compares MongoDB and PostgreSQL data.]*
* **Pending:** *[GPS Sensor: The next step is to implement the logic for the GPS sensor to automatically send its location (latitude and longitude) every 5 seconds. This data will be sent through the microservice, which will store it in MongoDB Atlas. This will provide near real-time updates of the sensor's location.]*

* **Communication:** The frontend does not interact directly with the hardware. Laravel acts as a bridge by communicating with the subsystem through APIs and webhooks. The goal of this sprint is to make it fully operational so the automotive team can integrate it during the next sprint.

---

## 6. Deployment (First Deployment)

We already have the Alpha version deployed online and publicly accessible.

For deployment, we used a VPS server on **DigitalOcean**. To keep everything organized and scalable, we orchestrated the infrastructure using **Docker Compose**, running services in isolated containers:

* **Container 1:** Database (PostgreSQL).
* **Container 2:** Backend (Laravel).
* **Container 3:** Frontend (Vue 3).

Additionally, our official production domain is already configured. You can access the platform through:

🌐 **https://fleet-ly.me**

*Multitenant Automation:* We configured the system so that when a new tenant is registered, the creation of its **PostgreSQL schema** and corresponding migrations is automatically executed, streamlining the onboarding process for new clients.

📖 **[Link to our Deployment Manual]**

---

## 7. Our Tech Stack (Libraries and Justification)

Here we document in detail the entire stack we use and the reason behind each dependency, which we already consolidated during the previous sprint.

### Frontend

**Main Dependencies (runtime):**

- `vue`: Main framework (UI, reactivity, Composition API). Mature ecosystem, high performance, and TypeScript compatibility.
- `vue-router`: SPA routing. Native integration with Vue 3 and support for nested routes.
- `pinia`: Global state management (explained above).
- `axios`: HTTP client for API calls. Easier handling of interceptors and timeouts compared to *fetch*.
- `@tanstack/vue-query`: Remote data management and caching (server state). Saves a lot of work when synchronizing data and retrying failed requests.
- `@tanstack/vue-table`: Helpers for building advanced tables with pagination and filtering.
- `@vueuse/core`: Collection of reactive utilities (avoids reinventing the wheel for things like localStorage).
- `zod`: Runtime data validation for safer payload handling.
- `lucide-vue-next`: Ready-to-use SVG icons for Vue.
- `radix-vue`: Accessible low-level UI components.
- `reka-ui`: Component library (UI kit) used in the project to speed up development.
- `tailwind-merge` and `clsx`: Utilities to merge and concatenate Tailwind classes without conflicts.
- `tailwindcss-animate`: Animation utility classes.
- `vue-sonner`: Toast/notification system for the interface.
- `vue3-cookies`: Simple session and preference cookie management.
- `class-variance-authority`: Typed CSS class variant definitions.

**DevDependencies (development tools):**

- `vite` / `@vitejs/plugin-vue`: Our bundler. Extremely fast compilation.
- `typescript` / `vue-tsc`: Static typing.
- `tailwindcss` / `@tailwindcss/postcss`: CSS framework.
- `npm-run-all2`: Runs multiple npm scripts simultaneously.
- `vite-plugin-vue-devtools`: Integrates Vue DevTools directly into the local environment.

*In summary: We chose libraries that improve productivity and provide strong typing. For remote data we rely on vue-query, and for the UI we combine Tailwind with radix-vue and reka-ui to create accessible and consistent components.*

### Backend

- **Framework:** Laravel
- **Database:** PostgreSQL

**Main Libraries:**

- **Sanctum:** Handles API token authentication and security.
- **Spatie (laravel-permission):** Allows us to implement a Role-Based Access Control (RBAC) system with granular permissions (e.g. `can.activate.reservation`).
- **nesbot/carbon:** Essential for date handling in PHP. We use it extensively in the `ReservationController` to calculate trip durations (`diffInMinutes`), manage pricing, and handle reservation expiration logic.

---

## 8. Repository Structure and Rules

We separated the code into three repositories within the same organization. Each repository contains its folder structure documented in its own `README.md`.

* 📂 [Frontend Repository](https://github.com/Sprint5-ProjectSIMS-Team3/sims-frontend)

* 📂 [Backend Repository](https://github.com/Sprint5-ProjectSIMS-Team3/sims-backend)

* 📂 [IoT Subsystem Repository](https://github.com/Sprint5-ProjectSIMS-Team3/sims-subsystem)

### Governance

* We added a **Contributor Covenant** to all repositories.
* The project is licensed under the **EUPL** license.

### Code Conventions

We all try to follow these rules to keep the codebase clean:

- **Composables**: `useComposableName.ts`
- **Components**: `PascalCase.vue`
- **Classes**: `PascalCase`
- **Routes**: `kebab-case`
- **Variables and functions**: `camelCase`
- **Language:** All code, filenames, and comments **must be written in English**.

### Workflow (Git Flow)

Check the `CONTRIBUTING.md` file in each repository to see how we manage branches. For commits, we use the following prefixes:

- `Fix: Fixed the users CRUD`
- `Feat: Added users CRUD to the backend`
- `Refactor: Improved authentication logic`
