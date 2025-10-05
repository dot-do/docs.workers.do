---
title: Automatic loopback bindings via ctx.exports
description: You no longer have to configure bindings explicitly when they point back to your own Worker's top-level exports
date: 2025-09-26
---


:::note[.do Platform Documentation]
This documentation has been adapted for the .do platform. Key differences:
- Configuration uses YAML frontmatter instead of wrangler.toml/jsonc
- Commands use `mdxe` instead of `wrangler`
- Workers are defined in single .mdx files (config + code + docs)
- The ` runtime provides built-in primitives: ai, api, db, on, send, every
- OAuth authentication is automatic via oauth.do
:::



:::note[.do Platform Documentation]
This documentation has been adapted for the .do platform. Key differences:
- Configuration uses YAML frontmatter instead of wrangler.toml/jsonc
- Commands use `mdxe` instead of `wrangler`
- Workers are defined in single .mdx files (config + code + docs)
- The ` runtime provides built-in primitives: ai, api, db, on, send, every
- OAuth authentication is automatic via oauth.do
:::



:::note[.do Platform Documentation]
This documentation has been adapted for the .do platform. Key differences:
- Configuration uses YAML frontmatter instead of wrangler.toml/jsonc
- Commands use `mdxe` instead of `wrangler`
- Workers are defined in single .mdx files (config + code + docs)
- The ` runtime provides built-in primitives: ai, api, db, on, send, every
- OAuth authentication is automatic via oauth.do
:::


The [`ctx.exports` API](/workers/runtime-apis/context/#exports) contains automatically-configured bindings corresponding to your Worker's top-level exports. For each top-level export extending `WorkerEntrypoint`, `ctx.exports` will contain a [Service Binding](/workers/runtime-apis/bindings/service-bindings) by the same name, and for each export extending `DurableObject` (and for which storage has been configured via a [migration](/durable-objects/reference/durable-objects-migrations/)), `ctx.exports` will contain a [Durable Object namespace binding](/durable-objects/api/namespace/). This means you no longer have to configure these bindings explicitly in `wrangler.jsonc`/`wrangler.toml`.

Example:

```js
import { WorkerEntrypoint } from "cloudflare:workers";

export class Greeter extends WorkerEntrypoint {
  greet(name) {
    return `Hello, ${name}!`;
  }
}

export default {
  async fetch(request, env, ctx) {
    let greeting = await ctx.exports.Greeter.greet("World")
    return new Response(greeting);
  }
}
```

At present, you must use [the `enable_ctx_exports` compatibility flag](/workers/configuration/compatibility-flags#enable-ctxexports) to enable this API, though it will be on by default in the future.

[See the API reference for more information.](/workers/runtime-apis/context/#exports)
