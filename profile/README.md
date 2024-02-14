# @rappstack/dev

# rappstack

rappstack (reactive app stack) is a semi-opinionated stack that emphasizes:

* reactive programming
* general purpose contexts
* tiny browser bundles
* performance

The tech stack includes:

* [bunjs](https://bun.sh/)
* [Elysiajs](https://elysiajs.com/)
* [SQLite](https://www.sqlite.org/index.html)
* [DrizzleORM](https://orm.drizzle.team/)
* [Auth.js](https://authjs.dev/)
* [tailwindcss](https://tailwindcss.com/)
* [ctx-core](https://github.com/ctx-core/ctx-core)
* [rmemo](https://github.com/ctx-core/rmemo)
* [relement](https://github.com/relementjs/relementjs)
* [rebuildjs](https://github.com/rebuildjs/rebuildjs)
* [relysjs](https://github.com/relysjs/relysjs)
* [hyop](https://github.com/hyopjs/hyop)

## Databases

[Sqlite](https://www.sqlite.org/index.html) used to make the deployments of many apps easier to maintain. Since SQLite is embedded, a separate database service is not needed. Other databases can be supported as needed, but the Drizzle schemas are currently set on SQLite.

## Deployment Environments

I'm using docker-compose on a VPS to keep costs of services...ahem Redis...down. The new-fangled edge computing services are attractive, but the costs are too high right now. It's less work to ssh into a box to maintain an app as it more closely matches the development environment & minimal additional software is needed. None of the apps are heavily used, so scale & dynamic geographic latency reduction is not needed..a simple CDN works. If needed, Edge computing will be added as needed. The [libsql](https://github.com/tursodatabase/libsql) [Drizzle adapter](https://orm.drizzle.team/docs/get-started-sqlite#turso) can be used instead of [bun:sqlite](https://bun.sh/docs/api/sqlite) to support [Turso](https://turso.tech/).

## MPA First

rappstack emphasies MPA first development. SPAs are supported, but the majority of the apps that I write are best served with an MPA architecture. A client-side router can be added as needed.

## Contexts

[ctx-core/be](https://github.com/ctx-core/be) is used for contexts.

## Browser Bundle Sizes

rappstack uses relysjs (which uses rebuildjs + ElysiaJS). Relysjs' app modules are based on middleware. For each middleware module, a `index.server.ts` & `index.browser.ts` file can be created. Having an expicit `index.browser.ts` allows control over the javascript that is sent to the browser. Relementjs provides the `hy__bind` html attribute & `hy__bind` function, which allows the browser code to bind dynamic behavior to the rendered DOM. While relementjs is tiny (~ the size of VanJS), while scaling large & small depending on what is needed, relementjs does not need to be loaded in the browser.

## So...what about reactive apps?

rmemo (reactive memo), is a tiny (smaller than nanostores & TenCent's reactive-signal) reactive memo library. I don't mean to be tautological in the explanation, but reactive memos are all that is needed for a powerful reactive programming paradigm. Sure, there is `memo_`, `sig_`, `memosig_`, `lock_memosig_`, & ctx-related tree-shakable functions, but essentially, the reactive memo pattern handles async, subscriptions, events, animation orchestration, build orchestration, deployment orchestration, etc. I don't have docs yet, but there is code in the apps that use rappstack.

## You mentioned fast?

BunJS & ElysiaJS are both fast. Coupled with a small browser JS payload (starting with the default of 0 bytes), rappstack is probably not going to be a performance bottleneck.

## Does it support Vite?

No. I originally tried integrating with Vite, but Vite is a bit too complicated for my tiny mind. So I gave up & wrote rebuildjs. Rebuildjs enables predictable development where the builds are fast enough & the workflow is not interrupted. Esbuild does most of the heavy lifting. Rmemo & ctx-core are used. I understand the domain api. Relysjs has an ephemeral stall_app, which "stalls" the request until the build is complete, including tailwindcss purge, & the updated server is started. This allows a page refresh to happen immediately without `Unable to connect` or 404 or whatever...

## What about HMR?

I don't really use HMR b/c (ctrl+r/cmd+r) is a pretty quick keystroke. Sometimes I want to keep the old page open b/c I'm in the middle of debugging & HMR has messed with the workflow. I'm not against HMR if there's a good solution to these workflow gotchas, but it has not been a priority yet. HMR support will probably come later.

## Projects

* https://github.com/btakita/briantakita.me-dev
* https://github.com/btakita/brookebrodack-dev
* https://github.com/cogov/cogov-dev

rappstack (reactive app stack) development repo with submodules to all @rappstack repos

## Why no npm packages?

@rappstack/* packages are currently used as git submodules in a bun monorepo. This is to get some apps off the ground & deal with api churn. I'm developing several websites & apps using these submodules. Once the api churn subsides, then I will release npm packages.

## Naming Conventions/System

[Tag Vector](https://briantakita.me/posts/tag-vector-0-introduction) is the naming convention.
