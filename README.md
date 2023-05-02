# web-tech

This repository is for trying out libraries/frameworks/tools for web development in order to evaluate what I like best.
Since tech *stack* is often a misnomer, I'll just call it tech.

Activity is good, but it seems there's a lot of noise, projects being hyped for some time and then slowly dying.
It's not motivating to waste time on research and setup just because some other framework becomes trendy.

The included repos use different technologies for a similar web app - some todo/task/time tracker.

- [tasktime-2019](https://github.com/vogler/tasktime-2019) - Svelte (reactive UI), Firebase (auth, hosted real-time document store), Bulma (CSS)
- [tasktime-2021](https://github.com/vogler/tasktime-2021) - Snowpack (build, dev), React (reactive UI), Chakra (UI component lib), Prisma (typed DB query builder), grant (auth)

The idea of https://todomvc.com is similar, but it's outdated and does not include backends.

## General opinions

- functional > imperative, static > dynamic typing, immutable > mutable
- language: since JavaScript is likely needed for frontend programming, it makes sense to use some language that at least has a transpiler to JavaScript
  - so, Ruby, Python, PHP etc. are out, also because TypeScript is nicer anyway (type system, interop, 'isomorphic' (misnomer again) code, i.e. share code between frontend & backend) and NodeJS has a good JIT compiler
  - in general just simpler to use the same language for everything
  - there are some Functional Reactive Programming projects in Haskell and OCaml, but haven't checked those out since [Elm](https://elm-lang.org) and [Opa](https://github.com/MLstate/opalang) which was way ahead of its time, but somehow never took off
- TypeScript > JavaScript:
  - Coming from OCaml, it's annoying that TS has no variant types, pattern matching and no full type inference.
  - Type erasure and lack of good meta programming like [ppx](https://ocamlverse.github.io/content/metaprogramming.html) makes it hard to fix things in external libs.
  - Unsound type system hasn't been an issue so far.
- views defined in the same language like [jsx](https://reactjs.org/docs/introducing-jsx.html)/[tsx](https://www.typescriptlang.org/docs/handbook/jsx.html) instead of some new template language like in [Svelte](https://svelte.dev/examples/if-blocks)
- [React](https://reactjs.org/) > [Angular](https://angular.io/) follows from the above
- UI component library > [Tailwind CSS](https://tailwindcss.com/)
- Query builder like [Prisma](https://github.com/prisma/prisma) > ORM like [TypeORM](https://github.com/typeorm/typeorm)
- open source, local > closed, cloud only, vendor lock-in; PostgreSQL > Firebase, AWS
- bundler/build system needs to be fast and have HMR/auto-update
- package manager: Python tooling is pure chaos ([PDM](https://dev.to/frostming/a-review-pipenv-vs-poetry-vs-pdm-39b4) > [Poetry](https://python-poetry.org/) > [Pipenv](https://remastr.com/blog/pip-pipenv-poetry-comparison) > [venv](https://towardsdatascience.com/poetry-to-complement-virtualenv-44088cc78fd1) > pip), while [npm](https://docs.npmjs.com/cli/v8/commands) just works and has everything in one tool.

## Notes on tech - frontend to backend
### UI components
- [YT: Fireship: 7 ways to deal with CSS UI libraries](https://www.youtube.com/watch?v=ouncVBiye_M):
  - https://mantine.dev/ - date/time picker, multi select, autocomplete, color input, notifications, quill.js rich text editor, file drag and drop
  - https://mui.com/ - looks like Android, autocomplete, date/time picker, multi select (via chip array), data grid with filtering/sorting/grouping/export, timeline, tree view
  - https://ant.design/components/overview/ - autocomplete, date/time picker, tree select, cascade select
  - https://rebassjs.org/ - few components
  - https://tamagui.dev/ - few components, supports React Native
  - https://chakra-ui.com/ - few form components
- Table/DataGrid:
  - https://github.com/ag-grid/ag-grid - {React, Angular, Vue, JS}, styling, many features, charts, nice docs/examples
  - https://github.com/TanStack/table - {React, Vue, Solid, Svelte, JS}, no styling, docs not as nice

### Reactive UI
- https://github.com/facebook/react/ - works, biggest community, but slow due to virtual DOM diffing
  - https://github.com/facebookexperimental/Recoil - more flexible state management needed for dynamic dependencies, but not nice to use due to boilerplate
  - [React I Love You, But You're Bringing Me Down](https://marmelab.com/blog/2022/09/20/react-i-love-you.html)
- https://github.com/solidjs/solid - faster due to fine grained dependencies -> no virtual DOM, but requires a bit more discipline, seems to solve many of React's problems, nice docs. However, can't just use all of the React libraries:
  - https://www.solidjs.com/guides/faq#is-there-react-compat-or-some-way-to-use-my-react-libraries-in-solid
  - https://www.solidjs.com/guides/faq#does-solid-have-a-nextjs-or-material-components-like-library-i-can-use
  - There are also projects that have versions for {React, Vue, Solid, Svelte, JS}: https://github.com/TanStack/table

### Forms/validation
- https://github.com/react-hook-form/react-hook-form - form state management and validation
  - https://github.com/jaredpalmer/formik is too heavy/verbose
- https://github.com/colinhacks/zod - schema validation with static type inference
  - https://github.com/colinhacks/zod#comparison
  - https://github.com/react-hook-form/resolvers#zod

### (Type-safe) client-server requests
- https://github.com/trpc/trpc - client imports router type from server

### Frontend frameworks for existing backend
- https://github.com/marmelab/react-admin - React/MUI for REST+GraphQL backends, ListGuesser to render tables based on type of data

### Server: render views, serve static files, API
- https://github.com/vercel/next.js/ - deals with SSR, auth, but requires React and overall opinionated, don't like the verbose/inflexible file-based routing
- https://github.com/expressjs/express - simple and widespread, but bare-bones and old

### Database abstraction, ORM, query builder
- https://github.com/prisma/prisma - Nicely typed query builder and migrations, but does not support unions. Issues:
  - [Support for a Union type](https://github.com/prisma/prisma/issues/2505#issuecomment-785229500)
  - [Option brand the model name into data](https://github.com/prisma/prisma/issues/5315)
- https://github.com/edgedb/edgedb - Replacement for Prisma? Also has TypeScript Query Builder, but in addition supports unions and inheritance.

### BaaS (self-hosted)
- Database, API, permissions, dashboard
  - https://github.com/directus/directus - REST+GraphQL for any existing SQL database, dashboard to manage data
  - https://github.com/surrealdb/surrealdb - realtime document-graph database with REST, custom QL, optional schema, GraphQL planned, dashboard planned
  - https://github.com/amplication/amplication - REST+GraphQL, MySQL
- ... and auth - open-source Firebase alternatives
  - https://github.com/supabase/supabase - supposedly hard to self-host; realtime REST+GraphQL on Postgres, files, dashboard
  - https://github.com/appwrite/appwrite - easy self-host via Docker, PHP, NoSQL API for MariaDB, dashboard
  - https://github.com/pocketbase/pocketbase - Go backend in one file, SQLite with realtime REST

### Reverse proxy / load balancing
- https://github.com/NickMRamirez/Proxy-Benchmarks - fast to slow: HAProxy, NGINX, Envoy, Traefik, Caddy
- https://github.com/caddyserver/caddy - Go web server with automatic HTTPS (Let's Encrypt), HTTP/3, reverse proxy, load balancing, caching, nicer to use but slower than nginx/haproxy/traefik, https://caddyserver.com - `caddy file-server --domain example.com` `caddy reverse-proxy --from example.com --to localhost:9000`
- https://github.com/NginxProxyManager/nginx-proxy-manager - web interface to manage proxy hosts for nginx
