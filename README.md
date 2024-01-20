# Angular and Fastify (with `Codeium` AI) course outline.

## Software requirements
- Node.js version 20.x or higher
- Visual Studio Code (`Angular Language Service`, `Prettier`, `Tailwind CSS IntelliSense`, `Docker`, `Codeium`)
- Docker
- Git

# Angular

## Basic Angular
- `Angular` CLI
- Create project
- Project structure
- `Tailwind` and `daisyUI` integration
- Using `daisyUI` themes
- Add font
- Data binding and Event
- Pages, Routing and Layout
- Query/Link parameters (Static/Dynamic)
- Create custom components
- Control flow (`@if`, `@for`, `@switch`)
- Pipe
- Style/Class
## Form
- Create `daisyUI` form
- Reactive Form/Form Group (Input/Checkbox/Radio/Select)
- Form validation
- Submit/Reset
## Http Service
- Using `DummyJSON`
- `Insomnia` REST client (basic CRUD pattern and Authen)
- Using HTTP Client service
- Authentication interceptors
- `GET`/`POST`/`PUT`/`DELETE` method
- Pagination
- Deployment (Docker/GitLab)

# Fastify
- Install Fastify CLI
- Create project
- Basic routing
- Adding basic plugins (`@fastify/cors`, `@fastify/formbody`, `@fastify/rate-limit`)
- Route parameters and validation
- Database connection with `knex` (MySQL version 5.6 or higher)
  - `knex` Query builder
  - `knex` Transaction
- Basic CRUD services
- Authentication with JWT token (`@fastify/jwt`)
- Route permission (`fastify-guard`)
- Deployment (`Docker`/`GitLab`)

# Angular and Fastify
- Create app layout
- Import database schema
- Login system
  - `daisyUI` login form
  - `Fastify` login system (`jwt` + `bcrypt`)
  - `Angular` route guard
  - Logout
- Add/Update users (`daisyUI` + `Fastify`)
- Pagination
- Change password
- Remove user
- Search user
- Deployment (`Docker`/`GitLab`)
