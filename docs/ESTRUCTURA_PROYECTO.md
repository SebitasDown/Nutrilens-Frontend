# Estructura del proyecto NutriLens (Clean Code)

Este documento describe la organización de carpetas de la aplicación y **qué debe ir en cada una**, para mantener un código limpio y escalable en Angular.

---

## Vista general

```
src/app/
├── core/           # Núcleo de la app (singleton, una sola vez)
├── shared/         # Elementos reutilizables en toda la app
├── features/       # Módulos por funcionalidad (lazy load)
├── layouts/        # Estructuras de página (header, sidebar, etc.)
├── models/         # Interfaces, tipos y DTOs
├── app.ts          # Componente raíz
├── app.routes.ts   # Rutas principales
└── app.config.ts   # Configuración global
```

---

## 📁 `core/`

**Qué es:** Todo lo que se usa **una sola vez** en la aplicación y es esencial para que la app funcione.

**Qué poner aquí:**
- Servicios singleton (auth, API, storage, notificaciones).
- Guards de rutas (protección de rutas, roles).
- Interceptors HTTP (token, errores, loading).
- Constantes globales (URLs de API, timeouts, config).

**Qué NO poner:** Componentes con vista, cosas que se reutilizan en varias pantallas (eso va en `shared/` o `features/`).

| Subcarpeta        | Contenido |
|-------------------|-----------|
| `core/services/` | Servicios con `providedIn: 'root'`. Ej: `AuthService`, `ApiService`. |
| `core/guards/`    | Guards para el router. Ej: `AuthGuard`, `RoleGuard`. |
| `core/interceptors/` | Interceptors de `HttpClient`. Ej: `AuthInterceptor`, `ErrorInterceptor`. |
| `core/constants/` | Constantes (API_URL, endpoints, config fija). |

**Regla:** Si es lógica global o de infraestructura → `core/`.

---

## 📁 `shared/`

**Qué es:** Código **reutilizable** en varias partes de la app (varias features o layouts).

**Qué poner aquí:**
- Componentes UI genéricos (botones, inputs, cards, modales, spinners).
- Pipes (formato de fecha, truncar texto, etc.).
- Directivas (click outside, debounce, etc.).
- Utilidades puras (helpers, validadores, formateadores).

**Qué NO poner:** Lógica de negocio de una feature concreta (eso va en `features/`).

| Subcarpeta              | Contenido |
|-------------------------|-----------|
| `shared/components/`    | Componentes reutilizables. Preferir `standalone: true`. |
| `shared/pipes/`         | Pipes para templates. |
| `shared/directives/`    | Directivas estructurales o de atributo. |
| `shared/utils/`         | Funciones puras sin dependencias de Angular. |

**Regla:** Si lo usas en más de una feature o en layouts → `shared/`.

---

## 📁 `features/`

**Qué es:** Cada **funcionalidad** de la app en su propia carpeta (por dominio o por pantalla).

**Qué poner aquí:**
- Una carpeta por feature, por ejemplo: `auth/`, `dashboard/`, `nutricion/`, `perfil/`.
- Dentro de cada feature: páginas, componentes específicos, servicios y rutas de esa feature.

**Estructura sugerida por feature:**

```
features/
├── auth/
│   ├── components/     # Componentes solo de auth (login form, register form)
│   ├── pages/          # Páginas (login, register, recuperar contraseña)
│   ├── auth.routes.ts  # Rutas de la feature
│   └── index.ts        # Barrel (re-exportar rutas, etc.)
├── dashboard/
│   ├── components/
│   ├── pages/
│   ├── dashboard.routes.ts
│   └── index.ts
└── nutricion/
    ├── components/
    ├── pages/
    ├── nutricion.routes.ts
    └── index.ts
```

**Qué NO poner:** Componentes que sirven para toda la app (esos van en `shared/`).

**Regla:** Si es una parte de la aplicación con sus propias pantallas y flujo → una carpeta en `features/`. Las rutas de cada feature se cargan en `app.routes.ts` (idealmente con lazy load).

---

## 📁 `layouts/`

**Qué es:** Componentes que definen la **estructura visual** de las páginas (donde va el header, sidebar, contenido, footer).

**Qué poner aquí:**
- Layout principal (con header, menú, contenido).
- Layout de auth (pantalla de login/registro sin menú).
- Cualquier variante de “esqueleto” de página.

**Ejemplos:** `MainLayoutComponent`, `AuthLayoutComponent`, `PublicLayoutComponent`.

**Regla:** Si define la estructura de la página (shell) y se usa envolviendo rutas → `layouts/`.

---

## 📁 `models/`

**Qué es:** Definiciones de **tipos**: interfaces, tipos TypeScript y DTOs que representan datos de dominio o contratos de API.

**Qué poner aquí:**
- Interfaces de entidades: `User`, `Meal`, `NutritionReport`.
- DTOs de request/response de la API.
- Tipos compartidos (enums, unions) que usen varias partes de la app.

**Qué NO poner:** Lógica (eso va en services o utils). Solo definiciones de forma de datos.

**Regla:** Si describe “qué forma tienen los datos” y se usa en varios sitios → `models/` (y opcionalmente re-exportar en `models/index.ts`).

---

## Cómo usar las rutas

En `app.routes.ts` se definen las rutas principales y se cargan las features con **lazy load**:

```ts
export const routes: Routes = [
  { path: '', redirectTo: 'dashboard', pathMatch: 'full' },
  { path: 'auth', loadChildren: () => import('./features/auth/auth.routes').then(m => m.AUTH_ROUTES) },
  { path: 'dashboard', loadChildren: () => import('./features/dashboard/dashboard.routes').then(m => m.DASHBOARD_ROUTES) },
  { path: '**', redirectTo: 'dashboard' },
];
```

Cada feature exporta sus rutas en un archivo `*.routes.ts` (por ejemplo `auth.routes.ts`).

---

## Resumen rápido

| Carpeta     | Uso |
|------------|-----|
| **core**   | Servicios globales, guards, interceptors, constantes. Una vez en la app. |
| **shared** | Componentes, pipes, directivas y utils reutilizables. |
| **features** | Una carpeta por funcionalidad (auth, dashboard, etc.) con sus páginas y rutas. |
| **layouts** | Componentes que definen la estructura de la página (header, sidebar, etc.). |
| **models**  | Interfaces, tipos y DTOs. Sin lógica. |

Siguiendo esta estructura se mantiene el proyecto ordenado, predecible y fácil de escalar (clean code en Angular).
