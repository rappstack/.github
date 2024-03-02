# @rappstack/dev

# rappstack

rappstack (reactive app stack) is an opinionated stack that emphasizes:

* reactive programming
* general purpose contexts
* tiny browser bundles
* performance

The tech stack includes:

* [bunjs](https://bun.sh/)
* [Elysiajs](https://elysiajs.com/)
* [SQLite](https://www.sqlite.org/index.html)
* [DrizzleORM](https://orm.drizzle.team/)
* [tailwindcss](https://tailwindcss.com/)
* [ctx-core](https://github.com/ctx-core/ctx-core)
* [rmemo](https://github.com/ctx-core/rmemo)
* [relement](https://github.com/relementjs/relementjs)
* [rebuildjs](https://github.com/rebuildjs/rebuildjs)
* [relysjs](https://github.com/relysjs/relysjs)
* [hyop](https://github.com/hyopjs/hyop)

## Databases

[Sqlite](https://www.sqlite.org/index.html) used to make the deployments of many apps easier to maintain. SQLite embeds into the process. A separate database service is not needed. Support for other databases can happen as needed. The Drizzle schemas are currently set on SQLite.

## Deployment Environments

I'm using docker-compose on a VPS to keep costs of services...ahem Redis...down. Edge computing services are attractive, but some of the costs are too high right right now. It's less work to ssh into a box to maintain an app. Since a VPS matches the development environment requiring minimal dependency additions.

The apps I am deveoping do not have heavy usage. Scale & dynamic geographic latency reduction is not needed. A simple CDN works. If needed, I will add Edge computing. I would switch to the [libsql](https://github.com/tursodatabase/libsql) [Drizzle adapter](https://orm.drizzle.team/docs/get-started-sqlite#turso). To support [Turso](https://turso.tech/) or a similar edge database scheme. The current Sqlite library is [bun:sqlite](https://bun.sh/docs/api/sqlite).

## MPA First

rappstack emphasizes MPA first development. SPA support also works but the majority of the apps that I write are best served with an MPA architecture. Adding client-side router will happen as needed.

## Contexts

[ctx-core/be](https://github.com/ctx-core/be) adds general purpose context support.

## Browser Bundle Sizes

rappstack uses relysjs (which uses rebuildjs + ElysiaJS). Relysjs' app server modules export middleware in *.server.ts. A optional matching *.browser.ts module supports browser code. An explicit `index.browser.ts` enables tight control over the browser javascript. hyop provides hydration as hypermedia. For browser rendering, relementjs is tiny...a bit small than VanJS. I have found that with hyop, browser rendering is not necessary most of the time. I can use Vanilla Javascript to hydrate the SSR DOM elements.

## So...what about reactive apps?

rmemo (reactive memo), is tiny a tiny reactive state library. Smaller than nanostores & TenCent's reactive-signal. Reactive memos cover all needs for a powerful reactive programming paradigm. With the `memo_`, `sig_`, `memosig_`, `lock_memosig_`, & ctx-related tree-shakable functions. The reactive memo pattern handles:

- async
- subscriptions
- events, animation orchestration
- build orchestration
- deployment orchestration

Some apps using rappstack include:

- https://github.com/btakita/briantakita.me-dev
- https://github.com/btakita/brookebrodack-dev
- https://github.com/btakita/herbaliciousbliss-site

## You mentioned fast?

BunJS & ElysiaJS are both fast. With a small browser JS payload rappstack is not going to be a performance bottleneck for apps. With JS payloads starting at 0 B, a perfect lighthouse score is easy to achieve.

## Does it support Vite?

No. I originally tried integrating with Vite, but Vite is a bit too complicated for my tiny mind. So I gave up & wrote rebuildjs. Rebuildjs facilitates predictable development. The builds are fast enough & the workflow is not interrupted. Esbuild does most of the heavy lifting. Rebuildjs has reactive app, middleware, & request state using rmemo & ctx-core.

An interesting feature of relysjs is the ephemeral `stall_app`. `stall__app` "stalls" the request until a rebuild is complete & the app restarts. Even waiting for build tasks such as tailwindcss purge.  This allows a page refresh to happen immediately without `Unable to connect` error.

## What about HMR?

I don't really use HMR b/c (ctrl+r/cmd+r) is a pretty quick keystroke. Sometimes I want to keep the old page open b/c I'm in the middle of debugging & HMR has messed with the workflow. I'm not against HMR if there's a good solution to these workflow gotchas, but it has not been a priority yet. HMR support will come later as needed.

## Projects

* https://github.com/btakita/briantakita.me-dev
* https://github.com/btakita/brookebrodack-dev
* https://github.com/cogov/cogov-dev

rappstack (reactive app stack) development repo with submodules to all @rappstack repos

## Why no npm packages?

@rappstack/* packages are used as git submodules in a bun monorepo. This is to get some apps off the ground & deal with api churn. I'm developing several websites & apps using these submodules. Once the api churn subsides, then I will release npm packages.

## Naming Conventions/System

[Tag Vector](https://briantakita.me/posts/tag-vector-0-introduction) is the naming convention.
