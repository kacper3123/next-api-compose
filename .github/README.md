<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/neg4n/next-api-compose/development/.github/assets/code_dark.png">
  <source media="(prefers-color-scheme: light)" srcset="https://raw.githubusercontent.com/neg4n/next-api-compose/development/.github/assets/code_light.png">
  <img align="right" width="500px" height="500px" alt="next-api-compose example code theme aware" src="https://raw.githubusercontent.com/neg4n/next-api-compose/development/.github/assets/code_dark.png"/>
</picture>

# Next.js API Compose &middot; [![version](https://badgen.net/npm/v/next-api-compose)](https://www.npmjs.com/package/next-api-compose) [![npm bundle size](https://badgen.net/bundlephobia/minzip/next-api-compose)](https://bundlephobia.com/package/next-api-compose)

## Introduction

This library provides a hassle-free way of composing multiple middleware functions into one [Next.js API Route Handler][next-api-route-handlers]'s method in the **[App Directory][next-app-router-intro]** router.

> [!IMPORTANT]
> The `2.0.0` version of the library supports both [app] and pages directory API utilities. If you're still using Pages Router and you want to migrate from versions below `2.0.0`, please read [migration guide](./MIGRATE_V2.md).

## Features

- [x] 😇 Simple and powerful API
- [x] 🚀 Designed both for Pages Router and App Router
- [x] 🥷 Excellent TypeScript support
- [x] 🧬 Maintaining order of middleware chain
- [x] 📦 No dependencies, small footprint

## Installing and basic usage

Install the package by running:

```sh
npm i next-api-compose -S
# or
yarn add next-api-compose
# or
pnpm i next-api-compose
```

then create an API Route Handler in the **[App Directory][next-app-router-intro]**:

in TypeScript **(recommended)**

```ts
import type { NextRequest } from "next/server";
import { compose } from "next-api-compose";

function someMiddleware(request: NextRequest & { foo: string }) {
  request.foo = "bar";
}

const { GET } = compose({
  GET: [
    [someMiddleware],
    (request /* This is automatically inferred */) => {
      return new Response(request.foo);
      //                         ^ (property) foo: string - autocomplete works here
    },
  ],
});

export { GET };
```

in JavaScript:

```js
import { compose } from "next-api-compose";

function someMiddleware(request) {
  request.foo = "bar";
}

const { GET } = compose({
  GET: [
    [someMiddleware],
    (request) => {
      return new Response(request.foo);
    },
  ],
});

export { GET };
```

## Theory and caveats

1. Unfortunately there is no way to dynamically export named ESModules _(or at least I did not find a way)_ so you have to use `export { GET, POST }` syntax instead of something like `export compose(...)` if you're composing GET and POST methods :(

2. Middleware is executed as specified in the per-method array, so if you want to execute middleware in a specific order, you have to be careful about it. Early returned `new Response()` halts the middleware chain.

## License and contributions

The project is licensed under The MIT License. Thanks for all the contributions! Feel free to open an issue or a pull request.

[next-api-route-handlers]: https://nextjs.org/docs/app/building-your-application/routing/route-handlers
[next-app-router-intro]: https://nextjs.org/docs/app/building-your-application/routing#the-app-router
