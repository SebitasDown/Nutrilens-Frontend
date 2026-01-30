# NutriLens Frontend

This project was generated using [Angular CLI](https://github.com/angular/angular-cli) version 21.1.2.

## Estructura del proyecto (Clean Code)

El código está organizado por carpetas para mantener un estilo **clean code**. En **[docs/ESTRUCTURA_PROYECTO.md](docs/ESTRUCTURA_PROYECTO.md)** se explica **qué debe ir en cada carpeta**:

- **`core/`** — Servicios singleton, guards, interceptors y constantes.
- **`shared/`** — Componentes, pipes, directivas y utilidades reutilizables.
- **`features/`** — Una carpeta por funcionalidad (auth, dashboard, etc.) con sus rutas.
- **`layouts/`** — Estructuras de página (header, sidebar, auth layout).
- **`models/`** — Interfaces, tipos y DTOs.

Consulta ese documento antes de añadir archivos nuevos para mantener el orden del proyecto.

## Development server

To start a local development server, run:

```bash
ng serve
```

Once the server is running, open your browser and navigate to `http://localhost:4200/`. The application will automatically reload whenever you modify any of the source files.

## Code scaffolding

Angular CLI includes powerful code scaffolding tools. To generate a new component, run:

```bash
ng generate component component-name
```

For a complete list of available schematics (such as `components`, `directives`, or `pipes`), run:

```bash
ng generate --help
```

## Building

To build the project run:

```bash
ng build
```

This will compile your project and store the build artifacts in the `dist/` directory. By default, the production build optimizes your application for performance and speed.

## Running unit tests

To execute unit tests with the [Vitest](https://vitest.dev/) test runner, use the following command:

```bash
ng test
```

## Running end-to-end tests

For end-to-end (e2e) testing, run:

```bash
ng e2e
```

Angular CLI does not come with an end-to-end testing framework by default. You can choose one that suits your needs.

## Additional Resources

For more information on using the Angular CLI, including detailed command references, visit the [Angular CLI Overview and Command Reference](https://angular.dev/tools/cli) page.
# Nutrilens-Frontend
