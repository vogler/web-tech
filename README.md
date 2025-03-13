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
  - https://chakra-ui.com/ - few form components, https://v2.chakra-ui.com/getting-started/comparison
- https://github.com/shadcn-ui/ui - copies code of components, can diff, but not nice that it always copies and not just for components you want to change
- https://www.reddit.com/r/reactjs/comments/vtgbai/comparison_of_ui_libraries_for_react/
  - https://github.com/themesberg/flowbite - based on Tailwind
  - https://github.com/saadeghi/daisyui - based on Tailwind
- https://github.com/ariakit/ariakit - recommended for high quality components: https://ariakit.org/components
- Table/DataGrid:
  - https://github.com/ag-grid/ag-grid - {React, Angular, Vue, JS}, styling, many features, charts, nice docs/examples
  - https://github.com/TanStack/table - {React, Vue, Solid, Svelte, JS}, no styling, docs not as nice
- Icons:
  - https://github.com/iconify/iconify unifies 150+ icon sets with 200k icons; https://icon-sets.iconify.design/
    - alternative explore/search for iconify icons: https://icones.js.org
  - nice set of ~6k icons: https://tabler.io/icons

### Reactive UI
- https://github.com/facebook/react/ - works, biggest community, but slow due to virtual DOM diffing, React Compiler can add memoization, but not supported well yet (in Next.js only via Webpack)
  - state management: go for jotai
    - https://zustand.docs.pmnd.rs/getting-started/comparison
    - top-down single store (like Redux): https://github.com/pmndrs/zustand 
    - bottom-up composition of atoms (like Recoil but w/o the ugly strings): https://github.com/pmndrs/jotai
    - old: https://github.com/facebookexperimental/Recoil - more flexible state management needed for dynamic dependencies, but not nice to use due to boilerplate
  - [React I Love You, But You're Bringing Me Down](https://marmelab.com/blog/2022/09/20/react-i-love-you.html)
- https://github.com/solidjs/solid - faster due to fine grained dependencies -> no virtual DOM, but requires a bit more discipline, seems to solve many of React's problems, nice docs. However, can't just use all of the React libraries:
  - https://www.solidjs.com/guides/faq#is-there-react-compat-or-some-way-to-use-my-react-libraries-in-solid
  - https://www.solidjs.com/guides/faq#does-solid-have-a-nextjs-or-material-components-like-library-i-can-use
  - There are also projects that have versions for {React, Vue, Solid, Svelte, JS}: https://github.com/TanStack/table

### Mobile
- go for Progressive Web App with responsive layout if possible; if not go for Expo
- some things only work with Chrome: https://caniuse.com/web-bluetooth
- web-hybrid: go for ionic/react if there's already a web app: webview + access to native SDK: https://github.com/ionic-team/ionic-framework - old: Capacitor, Cordova
  - https://ionic.io/resources/articles/ionic-vs-react-native-a-comparison-guide
- hybrid-native: go for React Native via Expo for mobile-first apps
  - https://expo.dev build for iOS/Android/Web and submit to app stores
  - file-based routing like Next.js: https://docs.expo.dev/router/introduction/
  - why bother with Flutter?
- https://onestack.dev web + React Native via Vite plugin
  - Sync engine: https://zero.rocicorp.dev
- https://tamagui.dev UI for web + React Native with optional compiler, but missing things like menu
  - https://tamagui.dev/docs/intro/why-a-compiler

### Forms/validation
- https://github.com/react-hook-form/react-hook-form - form state management and validation
  - https://github.com/jaredpalmer/formik is too heavy/verbose
- https://github.com/colinhacks/zod - schema validation with static type inference
  - https://github.com/colinhacks/zod#comparison - better than Joi, Yup, etc.
  - https://github.com/react-hook-form/resolvers#zod
- https://github.com/arktypeio/arktype - like zod, but uses static/dynamic parser for string literals instead of method chaining
  - [ArkType Vs Zod](https://gist.github.com/ssalbdivad/d60d876ab6486adc97e38e3f6916e93f), [reddit](https://www.reddit.com/r/node/comments/11l35tg/introducing_arktype_typescript_you_can_take_to/)

### (Type-safe) client-server requests
- https://github.com/trpc/trpc - client imports router type from server; [blog post](https://colinhacks.com/essays/painless-typesafety)

### Frontend frameworks for existing backend
- https://github.com/marmelab/react-admin - React/MUI for REST+GraphQL backends, ListGuesser to render tables based on type of data

### Server: render views, serve static files, API
- https://github.com/vercel/next.js/ - deals with SSR, auth, but requires React and overall opinionated, don't like the verbose/inflexible file-based routing
- https://github.com/expressjs/express - simple and widespread, but bare-bones and old
- https://github.com/koajs/koa - a bit more modern than express

### Database abstraction, ORM, query builder
- https://github.com/prisma/prisma - Nicely typed query builder and migrations, but does not support unions. Issues:
  - [Support for a Union type](https://github.com/prisma/prisma/issues/2505#issuecomment-785229500)
    - maybe will be solved by [ZenStack](https://zenstack.dev/blog/polymorphism) at some point
  - [Option brand the model name into data](https://github.com/prisma/prisma/issues/5315)
- https://github.com/edgedb/edgedb - Replacement for Prisma? Also has TypeScript Query Builder, but in addition supports unions and inheritance.
- https://github.com/kysely-org/kysely - TS-only query builder, just checks queries against supplied types for tables, migration only via supplied functions -> not as convenient as Prisma
  - https://github.com/WiseLibs/better-sqlite3 - recommended for sqlite
- https://github.com/gajus/slonik - type-safe query builder for PostgreSQL, but just uses template strings for SQL (so, no auto-complete?)
- [YT: BF: I tried 8 different Postgres ORMs](https://www.youtube.com/watch?v=4QN1BzxF8wM)
  - pg https://github.com/brianc/node-postgres
  - postgres.js https://github.com/porsager/postgres
  - knex https://github.com/knex/knex
  - kysely https://github.com/kysely-org/kysely
  - sequelize https://github.com/sequelize/sequelize
  - typeorm https://github.com/typeorm/typeorm
  - prisma https://github.com/prisma/prisma
  - drizzle https://github.com/drizzle-team/drizzle-orm
- https://github.com/typeorm/typeorm - schema by annotating classes, tried in https://github.com/vogler/syncmine and had issues with basic key constraints: [primary](https://github.com/typeorm/typeorm/issues/3238), [unique](https://github.com/typeorm/typeorm/issues/4122)
- https://github.com/drizzle-team/drizzle-orm - type-safe query builder with schema definition similar to Zod (infers type from value), supports many DBs

### Sync, local-first, CRDT
- see CRDT.md; local-first software: https://www.inkandswitch.com/local-first/
- https://electric-sql.com realtime sync for Postgres
- https://onestack.dev Sync engine: https://zero.rocicorp.dev
- https://github.com/toeverything/octobase local-first DB used by Affine

### BaaS (self-hosted)
- Database, API, permissions, dashboard
  - https://github.com/strapi/strapi - headless CMS with nice content-type builder, REST+GraphQL
  - https://github.com/directus/directus - REST+GraphQL for any existing SQL database, dashboard to manage data
  - https://github.com/surrealdb/surrealdb - realtime document-graph database with REST, custom QL, optional schema, GraphQL planned, dashboard planned
  - https://github.com/amplication/amplication - REST+GraphQL, MySQL
  - https://github.com/zenstackhq/zenstack - extends Prisma schema language with access control, plugins to generate tRPC router, Zod, React hooks
- ... and auth - open-source Firebase alternatives
  - https://github.com/supabase/supabase - supposedly hard to self-host; realtime REST+GraphQL on Postgres, files, dashboard
  - https://github.com/appwrite/appwrite - easy self-host via Docker, PHP, NoSQL API for MariaDB, dashboard
  - https://github.com/pocketbase/pocketbase - Go backend in one file, SQLite with realtime REST

### Reverse proxy / load balancing
- https://github.com/NickMRamirez/Proxy-Benchmarks - fast to slow: HAProxy, NGINX, Envoy, Traefik, Caddy
- https://github.com/caddyserver/caddy - Go web server with automatic HTTPS (Let's Encrypt), HTTP/3, reverse proxy, load balancing, caching, nicer to use but slower than nginx/haproxy/traefik, https://caddyserver.com - `caddy file-server --domain example.com` `caddy reverse-proxy --from example.com --to localhost:9000`
- https://github.com/NginxProxyManager/nginx-proxy-manager - web interface to manage proxy hosts for nginx
