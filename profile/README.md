# rappstack

rappstack (reactive app stack) is a semi-opinionated stack that emphasizes:

* reactive programming
* general purpose contexts
* tiny browser bundles
* server side performance

The tech stack includes:

* [bunjs](https://bun.sh/)
* [Elysiajs](https://elysiajs.com/)
* [ctx-core](https://github.com/ctx-core/ctx-core)
* [rmemo](https://github.com/ctx-core/rmemo)
* [relement](https://github.com/relementjs/relementjs)
* [rebuildjs](https://github.com/rebuildjs/rebuildjs)
* [relysjs](https://github.com/relysjs/relysjs)
* [tailwindcss](https://tailwindcss.com/)
* [Prisma?](https://www.prisma.io/)

## Databases

Postgres & Redis will be used. Probably a document database such as MongoDB or RethinkDB. Possibly a vector database such as Weaviate.

## Deployment Environments

I'm using docker-compose on a VPS to keep costs of services...ahem Redis...down. The new-fangled edge computing services are attractive, but the costs are too high right now. So edge computing is a consideration but not what I'm using right now. Some of the exports will work in a cloud or edge computing, but the stack is targeting a VPS environment at this time.

## MPA First

rappstack emphasies MPA first. SPAs are supported, but the majority of the apps that I write are best served with an MPA architecture. A client-side router will be needed when I or someone else who uses this stack needs one.

## Naming Conventions/System

[Tag Vector](https://briantakita.me/posts/tag-vector-0-introduction) is the naming convention.

## Contexts

ctx-core/be is used for contexts.

## Browser Bundle Sizes

rappstack uses relysjs (which uses rebuildjs + ElysiaJS). Relysjs' app modules are based on middleware. For each middleware module, a `index.server.ts` & `index.browser.ts` file can be created. Having an expicit `index.browser.ts` allows control over the javascript that is sent to the browser. Relementjs provides the `hy__bind` html attribute & `hy__bind` function, which allows the browser code to bind dynamic behavior to the rendered DOM. While relementjs is tiny (~ the size of VanJS), while scaling large & small depending on what is needed, relementjs does not need to be loaded in the browser.

## So...what about reactive apps?

rmemo (reactive memo), is a tiny (smaller than nanostores & TenCent's reactive-signal) reactive memo library. I don't mean to be tautological in the explanation, but reactive memos are all that is needed for a powerful reactive programming paradigm. Sure, there is `memo_`, `sig_`, `memosig_`, `lock_memosig_`, & ctx-related tree-shakable functions, but essentially, the reactive memo pattern handles async, subscriptions, events, animation orchestration, build orchestration, deployment orchestration, etc. I don't have docs yet, but there is code in the apps that use rappstack.

## You mentioned fast?

BunJS & ElysiaJS are both fast. Coupled with a small browser JS payload (starting with the default of 0 bytes), rappstack is probably not going to be a performance bottleneck.

## Does it support Vite?

No. I originally tried integrating with Vite, but Vite is a bit too complicated for my tiny mind. So I gave up & wrote rebuildjs. Rebuildjs enables predictable development where the builds are fast enough & the workflow is not interrupted. Esbuild does most of the heavy lifting. Rmemo & ctx-core are used. I understand the domain api. Relysjs has an ephemeral stall_app, which "stalls" request until the build is complete, including tailwindcss purge, & the updated server is started. This allows a page refresh to happen immediately without `Unable to connect` or 404 or whatever...

## What about HMR?

I don't really use HMR b/c (ctrl+r/cmd+r) is a pretty quick keystroke. Sometimes I want to keep the old page open b/c I'm in the middle of debugging & HMR has messed with the workflow. I'm not against HMR if there's a good solution to these workflow gotchas, but it has not been a priority yet. HMR support will probably come later. 

## Projects

* https://github.com/btakita/briantakita.me-dev
* https://github.com/btakita/brookebrodack-dev
* https://github.com/cogov/cogov-dev
