# CHANGELOG

The version headers in this history reflect the versions of Apollo Server itself.  Versions of other packages (e.g. which are not actual HTTP integrations; packages not prefixed with `apollo-server`) may use different versions.  For more details, check the publish commit for that version in the Git history, or check the individual CHANGELOGs for specific packages which are maintained separately:

- [__CHANGELOG for `@apollo/gateway`__](https://github.com/apollographql/apollo-server/blob/master/packages/apollo-gateway/CHANGELOG.md)
- [__CHANGELOG for `@apollo/federation`__](https://github.com/apollographql/apollo-server/blob/master/packages/apollo-federation/CHANGELOG.md)

### vNEXT

> The changes noted within this `vNEXT` section have not been released yet.  New PRs and commits which introduce changes should include an entry in this `vNEXT` section as part of their development.  When a release is being prepared, a new header will be (manually) created below and the the appropriate changes within that release will be moved into the new section.

- `apollo-server-core`: Update GraphQL Playground to latest version to remove a rogue curly-brace appearing in the top-right corner of the interface under certain conditions. [PR #3702](https://github.com/apollographql/apollo-server/pull/3702) [Playground PR](https://github.com/apollographql/graphql-playground/pull/21)

### v2.10.0

- `apollo-server-express`: Support `CorsOptionsDelegate` type on `cors` parameter to `applyMiddleware`, to align with the supported type of the underlying [`cors`](https://npm.im/cors) middleware [itself](https://github.com/DefinitelyTyped/DefinitelyTyped/blob/31483b781ac30f98bdf4d40a517e921f2fc2ce37/types/cors/index.d.ts#L32). [#3613](https://github.com/apollographql/apollo-server/pull/3613)
- `apollo-server-core`: Allow asynchronous initialization of datasources: the `initialize` method on datasources may now return a Promise, which will be settled before any resolvers are called. [#3639](https://github.com/apollographql/apollo-server/pull/3639)
- `apollo-server-core`: experimental: Allow configuration of the parsed/validated document store by introducing an `experimental_approximateDocumentStoreMiB` property to the `ApolloServer` constructor options which overrides the default cache size of 30MiB. [#3755](https://github.com/apollographql/apollo-server/pull/3755)

### v2.9.16

> [See complete versioning details.](https://github.com/apollographql/apollo-server/commit/4d1a75e318897c335674c7ee046c0baec7df4a9b)

- `apollo-server-core`: Update apollo-tooling dependencies, resolve TS build error (missing types for node-fetch) [#3662](https://github.com/apollographql/apollo-server/pull/3662)

### v2.9.15

> [See complete versioning details.](https://github.com/apollographql/apollo-server/commit/0743d6b2f1737758cf09e80d2086917772bc00c9)

- `apollo-engine-reporting`: Fix regression introduced by [#3614](https://github.com/apollographql/apollo-server/pull/3614) which caused `PersistedQueryNotFoundError`, `PersistedQueryNotSupportedError` and `InvalidGraphQLRequestError` errors to be triggered before the `requestDidStart` handler triggered `treeBuilder`'s `startTiming` method. This fix preserves the existing behavior by special-casing these specific errors.  [#3638](https://github.com/apollographql/apollo-server/pull/3638) fixes [#3627](https://github.com/apollographql/apollo-server/issues/3627)
- `apollo-server-cloud-functions`: Transmit CORS headers on `OPTIONS` request. [#3557](https://github.com/apollographql/apollo-server/pull/3557)
- `apollo-server-caching`: De-compose options interface for `KeyValueCache.prototype.set` to accommodate better TSDoc annotations for its properties (e.g. to specify that `ttl` is defined in _seconds_). [#3619](https://github.com/apollographql/apollo-server/pull/3619)
- `apollo-server-core`, `apollo-server-caching`: Introduce a `ttl` property, specified in seconds, on the options for automated persisted queries (APQ) which applies specific TTL settings to the cache `set`s during APQ registration.  Previously, all APQ cache records were set to 300 seconds.  Additionally, this adds support (to the underlying `apollo-server-caching` mechanisms) for a time-to-live (TTL) value of `null` which, when supported by the cache implementation, skips the assignment of a TTL value altogether.  This allows the cache's controller to determine when eviction happens (e.g. cache forever, and purge least recently used when the cache is full), which may be desireable for network cache stores (e.g. Memcached, Redis). [#3623](https://github.com/apollographql/apollo-server/pull/3623)
- `apollo-server-core`: Upgrade TS to 3.7.3 [#3618](https://github.com/apollographql/apollo-server/pull/3618)

### v2.9.14

> [See complete versioning details.](https://github.com/apollographql/apollo-server/commit/ad5eac5ea1741142122e4cb8fd34a9748be31e89)

- `apollo-server-core`: Ensure that plugin's `didEncounterErrors` hooks are invoked for known automated persisted query (APQ) errors. [#3614](https://github.com/apollographql/apollo-server/pull/3614)
- `apollo-server-plugin-base`: Move `TContext` generic from `requestDidStart` method to `ApolloServerPlugin` Interface. [#3525](https://github.com/apollographql/apollo-server/pull/3525)

### v2.9.13

> [See complete versioning details.](https://github.com/apollographql/apollo-server/commit/a0a60e73e04e913d388de8324f7d17e4406deea2)

- `@apollo/gateway`: Add `@types/node-fetch` as a regular dependency to avoid missing dependency for TypeScript consumers. [#3546](https://github.com/apollographql/apollo-server/pull/3546) fixes [#3471](https://github.com/apollographql/apollo-server/issues/3471)
- `apollo-engine-reporting`: Declare acceptable `graphql` versions ranges in `peerDependencies` rather than allowing it to occur implicitly (and less ideally) via its consumers (e.g. most `apollo-server-*` packages). [#3496](https://github.com/apollographql/apollo-server/pull/3496)

### v2.9.12

- Reinstate [#3530](https://github.com/apollographql/apollo-server/pull/3530) via [#3539](https://github.com/apollographql/apollo-server/pull/3539) - after a patch release of the `@apollo/protobufjs` fork, the build issue for consumers should be resolved.

### v2.9.11

- Revert [#3530](https://github.com/apollographql/apollo-server/pull/3530) via [#3535](https://github.com/apollographql/apollo-server/pull/3535)- the introduction of the `@apollo/protobufjs` fork is causing TS errors in consumer projects. Reverting this change for now, and will reintroduce it after the issue is resolved within the forked package.

### v2.9.10

> [See complete versioning details.](https://github.com/apollographql/apollo-server/commit/2a4c654986a158aaccf947ee56a4bfc48a3173c7)

- `apollo-engine-reporting`: Swap usage of `protobufjs` for a newly published fork located at [`@apollo/protobufjs`](https://npm.im/@apollo/protobufjs). This is to account for the [relative uncertainty](https://github.com/protobufjs/protobuf.js/issues/1199) into the continued on-going maintenance of the official `protobuf.js` project. This should immediately resolve a bug that affected `Long` types in `apollo-engine-reporting` and other non-Apollo projects that rely on `protobuf.js`'s `Long` type. [#3530](https://github.com/apollographql/apollo-server/pull/3530)

### v2.9.9

> [See complete versioning details.](https://github.com/apollographql/apollo-server/commit/93002737d53dd9a50b473ab9cef14849b3e539aa)

- `apollo-server-core`: Don't try parsing `variables` and `extensions` as JSON if they are defined but empty strings. [#3501](https://github.com/apollographql/apollo-server/pull/3501)
- `apollo-server-lambda`: Introduce `onHealthCheck` on `createHandler` in the same fashion as implemented in other integrations. [#3458](https://github.com/apollographql/apollo-server/pull/3458)
- `apollo-server-core`: Use `graphql`'s `isSchema` to more defensively check the user-specified schema's type at runtime and prevent unexpected errors. [#3462](https://github.com/apollographql/apollo-server/pull/3462)

### v2.9.8

> [See complete versioning details.](https://github.com/apollographql/apollo-server/commit/3cdde1b7a71ace6411fbacf82a1a61bf737444a6)

- `apollo-server-core`: Provide accurate type for `formatResponse` rather than generic `Function` type. [#3431](https://github.com/apollographql/apollo-server/pull/3431)
- `apollo-server-core`: Pass complete request context to `formatResponse`, rather than just `context`. [#3431](https://github.com/apollographql/apollo-server/pull/3431)

### v2.9.7

> [See complete versioning details.](https://github.com/apollographql/apollo-server/commit/5d94e986f04457ec17114791ee6db3ece4213dd8)

- `apollo-server-errors`: Fix `ApolloError` bug and `GraphQLError` spec compliance [#3408](https://github.com/apollographql/apollo-server/pull/3408)

### v2.9.6

> [See complete versioning details.](https://github.com/apollographql/apollo-server/commit/fc7462ec5f8604bd6cba99aa9a377a9b8e045566)

- `@apollo/gateway`, `@apollo/federation`, `apollo-engine-reporting`: Update `apollo-graphql` dependency to bring in [`apollo-tooling`'s #1551](https://github.com/apollographql/apollo-tooling/pull/1551) which resolve runtime errors when its source is minified.  While this fixes a particular minification bug when Apollo Server packages are minified, we _do not_ recommend minification of server code in most cases. [#3387](https://github.com/apollographql/apollo-server/pull/3387) fixes [#3335](https://github.com/apollographql/apollo-server/issues/3335)
- `apollo-server-koa`: Correctly declare dependency on `koa-compose`. [#3356](https://github.com/apollographql/apollo-server/pull/3356)
- `apollo-server-core`: Preserve any `extensions` that have been placed on the response when pre-execution errors occur. [#3394](https://github.com/apollographql/apollo-server/pull/3394)

### v2.9.3

> [See complete versioning details.](https://github.com/apollographql/apollo-server/commit/a1fbf95fc01739d5cbaa59919149bb85c563fdaa)

- `apollo-server-express`: Add direct dependency on `express` to allow for usage of `express.Router` for `getMiddleware` functionality (from [#2435](https://github.com/apollographql/apollo-server/pull/2435)).  Previously, unlike other server integration packages, `apollo-server-express` did not directly need `express` as a dependency since it only relied on `express` for TypeScript typings. [#3239](https://github.com/apollographql/apollo-server/pull/3239) fixes [#3238](https://github.com/apollographql/apollo-server/issues/3238)
- `apollo-server-lambda`: Add `@types/aws-lambda` as a direct dependency to `apollo-server-express` to allow usage of its typings without needing to separately install it. [#3242](https://github.com/apollographql/apollo-server/pull/3242) fixes [#2351](https://github.com/apollographql/apollo-server/issue/2351)

### v2.9.2

> [See complete versioning details.](https://github.com/apollographql/apollo-server/commit/92ea402a90bf9817c9b887707abbd77dcf5edcb4)

- `apollo-server-koa`: **Drop support for Node.js v6 within the Apollo Server Koa integration in order to update `koa-bodyparser` dependency from `v3.0.0` to `v4.2.1`.** [#3229](https://github.com/apollographql/apollo-server/pull/3229) fixes [#3050](https://github.com/apollographql/apollo-server/issues/3050)
- `apollo-server-express`: Use explicit return type for new `getMiddleware` method. [#3230](https://github.com/apollographql/apollo-server/pull/3230) (hopefully) fixes [#3222](https://github.com/apollographql/apollo-server/issues/3222)

### v2.9.1

> [See complete versioning details.](https://github.com/apollographql/apollo-server/commit/029c8dca3af812ee70589cdb6de749df3d2843d8)

- `apollo-server-core`: Make `formatError` available to subscriptions in the same spirit as the existing `formatResponse`. [#2942](https://github.com/apollographql/apollo-server/pull/2942)
- `apollo-engine-reporting`: The behavior of the `engine.maxAttempts` parameter previously did not match its documentation. It is documented as being the max number of attempts *including* the initial attempt, but until this release it was actually the number of retries *excluding* the initial attempt. The behavior has been changed to match the documentation (and the literal reading of the option name). [#3218](https://github.com/apollographql/apollo-server/pull/3218)
- `apollo-engine-reporting`: When sending the report fails with a server-side 5xx error, include the full error from the server in the logs. [#3218](https://github.com/apollographql/apollo-server/pull/3218)
- `apollo-server-core`: Fix regression which prevented the resizing of the schema panel in GraphQL Playground. [#3224](https://github.com/apollographql/apollo-server/pull/3224) and [upstream](https://github.com/apollographql/graphql-playground/pull/19)

### v2.9.0

> [See complete versioning details.](https://github.com/apollographql/apollo-server/commit/6037f6e80fdaa53b50b99ae94d93c724c382c23c)

- `apollo-server-express`, `apollo-server-koa`: A new `getMiddleware` method has been introduced, which accepts the same parameters as `applyMiddleware` with the exception of the `app` property.  This allows implementors to obtain the middleware directly and "`use`" it within an existing `app`.  In the near-term, this should ease some of the pain points with the previous technique.  Longer-term, we are exploring what we consider to be a much more natural approach by introducing an "HTTP transport" in Apollo Server 3.x.  See [this proposal issue](https://github.com/apollographql/apollo-server/issues/3184) for more information.  [#2435](https://github.com/apollographql/apollo-server/pull/2435)
- `@apollo/federation`: `buildFederatedSchema`'s `typeDefs` parameter now accepts arrays of `DocumentNode`s (i.e. type definitions wrapped in `gql`) and `resolvers` to make the migration from a single service into a federated service easier for teams previously utilizing this pattern. [#3188](https://github.com/apollographql/apollo-server/pull/3188)

### v2.8.2

> [See complete versioning details.](https://github.com/apollographql/apollo-server/commit/99f78c6782bce170186ba6ef311182a8c9f281b7)

- `apollo-server-koa`: Update dependency koa to v2.8.1. [PR #3175](https://github.com/apollographql/apollo-server/pull/3175)
- `apollo-server-express`: Update types exported by the ASE package. [PR #3173](https://github.com/apollographql/apollo-server/pull/3175) [PR #3172](https://github.com/apollographql/apollo-server/pull/3172)

### v2.8.1

> [See complete versioning details.](https://github.com/apollographql/apollo-server/commit/84d80eba10d87663dab60af4a1cd46bccf30513f)

- `apollo-engine-reporting`: Fix reporting errors which have non-array `path` fields (eg, non-GraphQLError errors). [PR #3112](https://github.com/apollographql/apollo-server/pull/3112)
- `apollo-engine-reporting`: Add missing `apollo-server-caching` dependency. [PR #3054](https://github.com/apollographql/apollo-server/pull/3054)
- `apollo-server-hapi`: Revert switch from `accept` and `boom` which took place in v2.8.0. [PR #3089](https://github.com/apollographql/apollo-server/pull/3089)
- `@apollo/gateway`: Change the `setInterval` timer, which is used to continuously check for updates to a federated graph from the Apollo Graph Manager, to be an `unref`'d timer.  Without this change, the server wouldn't terminate properly once polling had started since the event-loop would continue to have unprocessed events on it. [PR #3105](https://github.com/apollographql/apollo-server/pull/3105)
- Switch to using community `@types/graphql-upload` types.
- `apollo-server-fastify`: Change the typing of the HTTP `response` from `OutgoingMessage` to `ServerResponse`. [Commit](https://github.com/apollographql/apollo-server/commit/7638f643fa0445f5f8151ef884da779d85fb954c)
- `apollo-server-hapi`: Pass the `raw` request and response objects to `graphql-upload`s `processRequest` method to align on the same TypeScript types. [Commit](https://github.com/apollographql/apollo-server/commit/8e49b288a6aecd0e134637e64ef4ed751aa8d304)

### v2.8.0

> [See complete versioning details.](https://github.com/apollographql/apollo-server/commit/ddeb71f8d6a0f3c91646aa0c7c99d2003b5bf73f)

- `@apollo/federation`: Add support for "value types", which are type definitions which live on multiple services' types, inputs, unions or interfaces.  These common types must be identical by name, kind and field across all services. [PR #3063](https://github.com/apollographql/apollo-server/pull/3063)
- `apollo-server-express`: Use the Express `send` method, rather than calling `net.Socket.prototype.end`. [PR #2842](https://github.com/apollographql/apollo-server/pull/2842)
- `apollo-server-hapi`: Update internal dependencies to use scoped packages `@hapi/accept` and `@hapi/boom`, in place of `accept` and `boom` respectively. [PR #3089](https://github.com/apollographql/apollo-server/pull/3089)

### v2.7.2

> [See complete versioning details.](https://github.com/apollographql/apollo-server/commit/d0b33f20ba4731c071d6fd8cfaeca1a1f3d83e4b)

- `apollo-engine-reporting`: Fix reporting errors from backend. (The support for federated metrics introduced in v2.7.0 did not properly handle GraphQL errors from the backend; all users of federated metrics should upgrade to this version.) [PR #3056](https://github.com/apollographql/apollo-server/pull/3056) [Issue #3052](https://github.com/apollographql/apollo-server/issues/3052)
- `apollo-engine-reporting`: Clean up `SIGINT` and `SIGTERM` handlers when `EngineReportingAgent` is stopped; fixes 'Possible EventEmitter memory leak detected' log. [PR #3090](https://github.com/apollographql/apollo-server/pull/3090)

### v2.7.1

> [See complete versioning details.](https://github.com/apollographql/apollo-server/commit/2f87e4af9a6f1e3c8f4c51b4f77860bd3150c8c6)

- `apollo-engine-reporting`: If an error is thrown by a custom variable transform function passed into the reporting option `sendVariableValues: { transform: ... }`, all variable values will be replaced with the string `[PREDICATE_FUNCTION_ERROR]`.
- `apollo-server-express`: Typing fix for the `connection` property, which was missing from the `ExpressContext` interface.  [PR #2959](https://github.com/apollographql/apollo-server/pull/2959)
- `@apollo/gateway`: Ensure execution of correct document within multi-operation documents by including the `operationName` in the cache key used when caching query plans used in federated execution. [PR #3084](https://github.com/apollographql/apollo-server/pull/3084)

### v2.7.0

> [See complete versioning details.](https://github.com/apollographql/apollo-server/commit/1d44f3d4756d43123eb01bf293e65f4a3c2e64c8)

- `apollo-engine-reporting`: **Behavior change**: By default, send no GraphQL variable values to Apollo's servers instead of sending all variable values. Adding the new EngineReportingOption `sendVariableValues` to send some or all variable values, possibly after transforming them. This replaces the `privateVariables` option, which is now deprecated. [PR #2931](https://github.com/apollographql/apollo-server/pull/2931)

  To maintain the previous behavior of transmitting **all** GraphQL variable values, unfiltered, to Apollo Engine, configure `engine`.`sendVariableValues` as follows:

  ```js
  engine: {
    sendVariableValues: { all: true }
  }
  ```
- `apollo-engine-reporting`: **Behavior change**: By default, send no GraphQL request headers and values to Apollo's servers instead of sending all. Adding the new EngineReportingOption `sendHeaders` to send some or all header values. This replaces the `privateHeaders` option, which is now deprecated. [PR #2931](https://github.com/apollographql/apollo-server/pull/2931)

   To maintain the previous behavior of transmitting  **all** GraphQL request headers and values, configure `engine`.`sendHeaders` as following:
     ```js
     engine: {
       sendHeaders: { all: true }
     }
     ```
- `apollo-engine-reporting`: **Behavior change**: If the error returned from the `engine.rewriteError` hook has an `extensions` property, that property will be used instead of the original error's extensions. Document that changes to most other `GraphQLError` fields by `engine.rewriteError` are ignored. [PR #2932](https://github.com/apollographql/apollo-server/pull/2932)
- `apollo-engine-reporting`: **Behavior change**: The `engine.maskErrorDetails` option, deprecated by `engine.rewriteError` in v2.5.0, now behaves a bit more like the new option: while all error messages will be redacted, they will still show up on the appropriate nodes in a trace. [PR #2932](https://github.com/apollographql/apollo-server/pull/2932)
- `apollo-server-core`, `@apollo/gateway`: **Introduced managed federation support**.  For more information on managed federation, see [the blog post](https://blog.apollographql.com/announcing-managed-federation-265c9f0bc88e) or jump to the [documentation for managed federation](https://www.apollographql.com/docs/platform/federation/).
- `@apollo/gateway@0.7.1`: Don't print a warning about an unspecified "graph variant" (previously, and in many ways still, known as "schema tag") every few seconds.  We do highly recommend specifying one when using the Apollo Platform features though! [PR #3043](https://github.com/apollographql/apollo-server/pull/3043)
- `graphql-playground`: Update to resolve incorrect background color on tabs when using the `light` theme. [PR #2989](https://github.com/apollographql/apollo-server/pull/2989) [Issue #2979](https://github.com/apollographql/apollo-server/issues/2979)
- `graphql-playground`: Fix "Query Planner" and "Tracing" panels which were off the edge of the viewport.
- `apollo-server-plugin-base`: Fix `GraphQLRequestListener` type definitions to allow `return void`. [PR #2368](https://github.com/apollographql/apollo-server/pull/2368)

### v2.6.7

> [See complete versioning details.](https://github.com/apollographql/apollo-server/commit/183de5f112324def375a45c239955e1bf1608fae)

- `apollo-server-core`: Guard against undefined property access in `isDirectiveDefined` which resulted in "Cannot read property 'some' of undefined" error. [PR #2924](https://github.com/apollographql/apollo-server/pull/2924) [Issue #2921](https://github.com/apollographql/apollo-server/issues/2921)

### v2.6.6

> [See complete versioning details.](https://github.com/apollographql/apollo-server/commit/26db63cbd5adf54b07a5b67c0e0fbff8e61c79aa)

- `apollo-server-core`: Avoid duplicate `cacheControl` directives being added via `isDirectiveDefined`, re-landing the implementation reverted in v2.6.1 which first surfaced in v2.6.0. [PR #2762](https://github.com/apollographql/apollo-server/pull/2762) [Reversion PR #2754](https://github.com/apollographql/apollo-server/pull/2754) [Original PR #2428](https://github.com/apollographql/apollo-server/pull/2428)
- `apollo-server-testing`: Add TypeScript types for `apollo-server-testing` client. [PR #2871](https://github.com/apollographql/apollo-server/pull/2871)
- `apollo-server-plugin-response-cache`: Fix undefined property access attempt which occurred when an incomplete operation was received. [PR #2792](https://github.com/apollographql/apollo-server/pull/2792) [Issue #2745](https://github.com/apollographql/apollo-server/issues/2745)

### v2.6.5

> [See complete versioning details.](https://github.com/apollographql/apollo-server/commit/a2b2a0d8f013826d08433129a69834035e04f1d5)

- `apollo-engine-reporting`: Simplify the technique for capturing `operationName`. [PR #2899](https://github.com/apollographql/apollo-server/pull/2899)
- `apollo-server-core`: Fix regression in 2.6.0 which caused `engine: false` not to disable Engine when the `ENGINE_API_KEY` environment variable was set. [PR #2850](https://github.com/apollographql/apollo-server/pull/2850)
- `@apollo/federation`: Introduced a `README.md`. [PR #2883](https://github.com/apollographql/apollo-server/pull/2883)
- `@apollo/gateway`: Introduced a `README.md`. [PR #2883](https://github.com/apollographql/apollo-server/pull/2883)

### v2.6.4

> [See complete versioning details.](https://github.com/apollographql/apollo-server/commit/596e2f20e090d2f860d238058118d860a72b3be4)

- `@apollo/gateway`: Pass `context` through to the `graphql` command in `LocalGraphQLDataSource`'s `process` method. [PR #2821](https://github.com/apollographql/apollo-server/pull/2821)
- `@apollo/gateway`: Fix gateway not sending needed variables for subqueries not at the root level. [PR #2867](https://github.com/apollographql/apollo-server/pull/2867)
- `@apollo/federation`: Allow matching enums/scalars in separate services and validate that enums have matching values. [PR #2829](https://github.com/apollographql/apollo-server/pull/2829).
- `@apollo/federation`: Strip `@external` fields from interface extensions. [PR #2848](https://github.com/apollographql/apollo-server/pull/2848)
- `@apollo/federation`: Add support for list type keys in federation. [PR #2841](https://github.com/apollographql/apollo-server/pull/2841)
- `@apollo/federation`: Deduplicate variable definitions for sub-queries. [PR #2840](https://github.com/apollographql/apollo-server/pull/2840)

### v2.6.3

> [See complete versioning details.](https://github.com/apollographql/apollo-server/commit/bdf634d4884774fa81fb22475aa4bd8178025762)

- `apollo-engine-reporting`: Set `forbiddenOperation` and `registeredOperation` later in the request lifecycle. [PR #2828](https://github.com/apollographql/apollo-server/pull/2828)
- `apollo-server-core`: Add `queryHash` to `GraphQLExecutor` for federation. [PR #2822](https://github.com/apollographql/apollo-server/pull/2822)
- `@apollo/federation`: Preserve descriptions from SDL of federated services. [PR #2830](https://github.com/apollographql/apollo-server/pull/2830)

### v2.6.2

- `apollo-engine-reporting-protobuf`: Update protobuf to include `forbiddenOperations` and `registeredOperations`. [PR #2768](https://github.com/apollographql/apollo-server/pull/2768)
- `apollo-server-core`: Add `forbiddenOperation` and `registeredOperation` to `GraphQLRequestMetrics` type. [PR #2768](https://github.com/apollographql/apollo-server/pull/2768)
- `apollo-engine-reporting`: Set `forbiddenOperation` and `registeredOperation` on trace if the field is true on `requestContext.metrics`. [PR #2768](https://github.com/apollographql/apollo-server/pull/2768)
- `apollo-server-lambda`: Remove `Object.fromEntries` usage. [PR #2787](https://github.com/apollographql/apollo-server/pull/2787)

### v2.6.1

- Revert: Don't add `cacheControl` directive if one has already been defined. Presently, although the TypeScript don't suggest it, passing a `String` as `typeDefs` to `ApolloServer` is supported and this would be a breaking change for non-TypeScript users. [PR #2428](https://github.com/apollographql/apollo-server/pull/2428)

### v2.6.0

- `apollo-server-core`: Introduce new `didEncounterErrors` life-cycle hook which has access to unformatted `errors` property on the `requestContext`, which is the first positional parameter that this new request life-cycle receives.  [PR #2719](https://github.com/apollographql/apollo-server/pull/2719)
- `apollo-server-core`: Allow request pipeline life-cycle hooks (i.e. plugins) to modify the response's `http.status` code (an integer) in the event of an error.  When combined with the new `didEncounterErrors` life-cycle hook (see above), this will allow modifying the HTTP status code in the event of an error.  [PR #2714](https://github.com/apollographql/apollo-server/pull/2714)
- `apollo-server-lambda`: Set `callbackWaitsForEmptyEventLoop` to `false` for `OPTIONS` requests to return as soon as the `callback` is triggered instead of waiting for the event loop to empty. [PR #2638](https://github.com/apollographql/apollo-server/pull/2638)
- `apollo-server`: Support `onHealthCheck` in the `ApolloServer` constructor in the same way as `cors` is supported.  This contrasts with the `-express`, `-hapi`, etc. variations which accept this parameter via their `applyMiddleware` methods and will remain as-is.  [PR #2672](https://github.com/apollographql/apollo-server/pull/2672)
- core: Expose SHA-512 hex hash digest of the Engine API key to plugins, when available, as `engine.apiKeyHash`. [PR #2685](https://github.com/apollographql/apollo-server/pull/2685) [PR #2736](https://github.com/apollographql/apollo-server/pull/2736)
- `apollo-datasource-rest`: If another `Content-type` is already set on the response, don't overwrite it with `application/json`, allowing the user's initial `Content-type` to prevail. [PR #2520](https://github.com/apollographql/apollo-server/issues/2035)
- Don't add `cacheControl` directive if one has already been defined. [PR #2428](https://github.com/apollographql/apollo-server/pull/2428)
- `apollo-cache-control`: Do not respond with `Cache-control` headers if the HTTP response contains `errors`. [PR #2715](https://github.com/apollographql/apollo-server/pull/2715)
- `apollo-server-core`: Skip loading `util.promisify` polyfill in Node.js engines >= 8.0 [PR #2278](https://github.com/apollographql/apollo-server/pull/2278)
- `apollo-server-core`: Lazy load `subscriptions-transport-ws` in core [PR #2278](https://github.com/apollographql/apollo-server/pull/2278)
- `apollo-server-cache-redis`: **BREAKING FOR USERS OF `apollo-server-cache-redis`** (This is a package that must be updated separately but shares the same `CHANGELOG.md` with Apollo Server itself.)  A new **major** version of this package has been published and updated to support Redis Standalone, Cluster and Sentinel modes.  This is a breaking change since it is now based on [`ioredis`](https://github.com/luin/ioredis) instead of [`node_redis`](https://github.com/NodeRedis/node_redis).  Although this update is compatible with the most common uses of `apollo-server-cache-redis`, please check the [options supported by `ioredis`](https://github.com/luin/ioredis/blob/master/API.md#new-redisport-host-options) while updating to this version.  The constructor options are passed directly from `RedisCache` to the new Redis adapter.  The pre-1.0 versions should continue to work with Apollo Server without modification. [PR #1770](https://github.com/apollographql/apollo-server/pull/1770)

### v2.5.1

- Upgrade GraphQL Playground to the latest upstream release.  This release also includes a new "Query Plan" panel for displaying the query planning results when running the Apollo Gateway.

### v2.5.0

#### New

- New plugin package `apollo-server-plugin-response-cache` implementing a full query response cache based on `apollo-cache-control` hints. The implementation added a few hooks and context fields; see the PR for details. There is a slight change to `cacheControl` object: previously, `cacheControl.stripFormattedExtensions` defaulted to false if you did not provide a `cacheControl` option object, but defaulted to true if you provided (eg) `cacheControl: {defaultMaxAge: 10}`. Now `stripFormattedExtensions` defaults to false unless explicitly provided as `true`, or if you use the legacy boolean `cacheControl: true`. For more information, [read the documentation](https://www.apollographql.com/docs/apollo-server/features/caching).  [PR #2437](https://github.com/apollographql/apollo-server/pull/2437)
- Add `rewriteError` option to `EngineReportingOptions` (i.e. the `engine` property of the `ApolloServer` constructor).  When defined as a `function`, it will receive an `err` property as its first argument which can be used to manipulate (e.g. redaction) an error prior to sending it to Apollo Engine by modifying, e.g., its `message` property.  The error can also be suppressed from reporting entirely by returning an explicit `null` value.  For more information, [read the documentation](https://www.apollographql.com/docs/apollo-server/features/errors#for-apollo-engine-reporting) and the [`EngineReportingOptions` API reference](https://www.apollographql.com/docs/apollo-server/api/apollo-server#enginereportingoptions). `maskErrorDetails` is now deprecated. [PR #1639](https://github.com/apollographql/apollo-server/pull/1639)
- `apollo-server-azure-functions`: Support `@azure/functions` to enable Apollo Server [Typescript development in Azure Functions](https://azure.microsoft.com/en-us/blog/improving-the-typescript-support-in-azure-functions/). [PR #2487](https://github.com/apollographql/apollo-server/pull/2487)
- Allow `GraphQLRequestListener` callbacks in plugins to depend on `this`. [PR #2470](https://github.com/apollographql/apollo-server/pull/2470)
- `apollo-server-testing`: Add `variables` and `operationName` to `Query` and `Mutation` types. [PR #2307](https://github.com/apollographql/apollo-server/pull/2307) [Issue #2172](https://github.com/apollographql/apollo-server/issue/2172)

#### Bug fixes

- Add `cache-control: no-cache` header to both `PersistedQueryNotSupportedError` and `PersistedQueryNotFoundError` responses as these should never be cached. [PR #2452](https://github.com/apollographql/apollo-server/pull/2452)
- `apollo-datasource-rest`: Don't attempt to parse "204 No Content" responses as JSON. [PR #2446](https://github.com/apollographql/apollo-server/pull/2446)
- `apollo-server-express`: Fix Playground URL when Apollo Server is mounted inside of another Express app by utilizing `req.originalUrl`. [PR #2451](https://github.com/apollographql/apollo-server/pull/2451)
- `apollo-datasource-rest`: Correctly allow a TTL value of `0` to represent "not-cacheable". [PR #2588](https://github.com/apollographql/apollo-server/pull/2588)
- `apollo-datasource-rest`: Fix `Invalid argument` in IE11, when `this.headers` is `undefined`. [PR #2607](https://github.com/apollographql/apollo-server/pull/2607)

### v2.4.8

- No functional changes in this version.  The patch version has been bumped to fix the `README.md` displayed on the [npm package for `apollo-server`](https://npm.im/apollo-server) as a result of a broken publish.  Apologies for the additional noise!

### v2.4.7

- Fix typings which incorrectly included `cors` as part of the constructor options for `apollo-server-express` (it should be defined via `applyMiddleware`) but, conversely, inadvertently omitted the perfectly valid `cors` option from the `apollo-server` constructor (where `applyMiddleware` is not used/available). [PR #2373](https://github.com/apollographql/apollo-server/pull/2373) [Issue #1882](https://github.com/apollographql/apollo-server/issues/1882)

### v2.4.6

- Allow Node.js-like runtimes to identify as Node.js as well. [PR #2357](https://github.com/apollographql/apollo-server/pull/2357) [Issue #2356](https://github.com/apollographql/apollo-server/issue/2356)

### v2.4.5

- `apollo-server-express`: Export `ExpressContext` [PR #2352](https://github.com/apollographql/apollo-server/pull/2352)

### v2.4.4

- Fix typing for ContextFunction incorrectly requiring the context object the function produces to match the parameters of the function [PR #2350](https://github.com/apollographql/apollo-server/pull/2350)

### v2.4.3

- `apollo-server-lambda`: Fix typings which triggered "Module has no default export" errors. [PR #2230](https://github.com/apollographql/apollo-server/pull/2230)
- `apollo-server-koa`: Support OPTIONS requests [PR #2288](https://github.com/apollographql/apollo-server/pull/2288)
- Add `req` and `res` typings to the `ContextFunction` argument for apollo-server and apollo-server-express. Update `ContextFunction` return type to allow returning a value syncronously. [PR #2330](https://github.com/apollographql/apollo-server/pull/2330)
- Type the `formatError` function to accept an GraphQLError as an argument and return a GraphQLFormattedError [PR #2343](https://github.com/apollographql/apollo-server/pull/2343)

### v2.4.2

- `apollo-server-fastify` is now on Apollo Server and lives within the `apollo-server` repository.  This is being introduced in a _patch_ version, however it's a _major_ version bump from the last time `apollo-server-fastify` was published under `1.0.2`.  [PR #1971](https://github.com/apollostack/apollo-server/pull/1971)
- Move `apollo-graphql` package to the `apollo-tooling` repository [PR #2316](https://github.com/apollographql/apollo-server/pull/2316)

### v2.4.1

- Fix inaccurate total duration in apollo-tracing [PR #2298](https://github.com/apollographql/apollo-server/pull/2298)
- Avoid importing entire `crypto` dependency tree if not in Node.js. [PR #2304](https://github.com/apollographql/apollo-server/pull/2304)
- Allow passing `parseOptions` to `ApolloServerBase` constructor. [PR #2289](https://github.com/apollographql/apollo-server/pull/2289)
- Rename `azureFunctions.d.ts` to `azureFunctions.ts`. [PR #2287](https://github.com/apollographql/apollo-server/pull/2287)
- Require `apollo-engine-reporting` only if `EngineReportingAgent` used. [PR #2305](https://github.com/apollographql/apollo-server/pull/2305)

### v2.4.0

- Implement an in-memory cache store to save parsed and validated documents and provide performance benefits for repeat executions of the same document. [PR #2111](https://github.com/apollographql/apollo-server/pull/2111) (`>=2.4.0-alpha.0`)
- Fix: Serialize arrays as JSON on fetch in `RESTDataSource`. [PR #2219](https://github.com/apollographql/apollo-server/pull/2219)
- Fix: The `privateHeaders` configuration for `apollo-engine-reporting` now allows headers to be specified using any case and lower-cases them prior to comparison. [PR #2276](https://github.com/apollographql/apollo-server/pull/2276)
- Fix broken `apollo-server-azure-functions` TypeScript definitions. [PR #2287](https://github.com/apollographql/apollo-server/pull/2287)

### v2.3.3

- `apollo-server` (only): Stop double-invocation of `serverWillStart` life-cycle event.  (More specific integrations - e.g. Express, Koa, Hapi, etc. - were unaffected.) [PR #2239](https://github.com/apollographql/apollo-server/pull/2239)
- Avoid traversing `graphql-upload` module tree in run-time environments which aren't Node.js. [PR #2235](https://github.com/apollographql/apollo-server/pull/2235)

### v2.3.2

- Switch from `json-stable-stringify` to `fast-json-stable-stringify`. [PR #2065](https://github.com/apollographql/apollo-server/pull/2065)
- Fix cache hints of `maxAge: 0` to mean "uncachable". [#2197](https://github.com/apollographql/apollo-server/pull/2197)
- Apply `defaultMaxAge` to scalar fields on the root object. [#2210](https://github.com/apollographql/apollo-server/pull/2210)
- Don't write to the persisted query cache until execution will begin. [PR #2227](https://github.com/apollographql/apollo-server/pull/2227)

- `apollo-server-azure-functions`: Added Azure Functions documentation and deployment examples [PR #2131](https://github.com/apollographql/apollo-server/pull/2131),
[Issue #2092](https://github.com/apollographql/apollo-server/issues/2092)

### v2.3.1

- Provide types for `graphql-upload` in a location where they can be accessed by TypeScript consumers of `apollo-server` packages. [ccf935f9](https://github.com/apollographql/apollo-server/commit/ccf935f9) [Issue #2092](https://github.com/apollographql/apollo-server/issues/2092)

### v2.3.0

- **BREAKING FOR NODE.JS <= 8.5.0 ONLY**: To continue using Apollo Server 2.x in versions of Node.js prior to v8.5.0, file uploads must be disabled by setting `uploads: false` on the `ApolloServer` constructor options.  Without explicitly disabling file-uploads, the server will `throw` at launch (with instructions and a link to our documentation).

  This early deprecation is due to changes in the third-party `graphql-upload` package which Apollo Server utilizes to implement out-of-the-box file upload functionality.  While, in general, Apollo Server 2.x aims to support all Node.js versions which were under an LTS policy at the time of its release, we felt this required an exception.  By `throw`-ing when `uploads` is not explicitly set to `false`, we aim to make it clear immediately (rather than surprisingly) that this deprecation has taken effect.

  While Node.js 6.x is covered by a [Long Term Support agreement by the Node.js Foundation](https://github.com/nodejs/Release#release-schedule) until April 2019, there are substantial performance (e.g. [V8](https://v8.dev/) improvements) and language changes (e.g. "modern" ECMAScript support) offered by newer Node.js engines (e.g. 8.x, 10.x).  We encourage _all users_ of Apollo Server to update to newer LTS versions of Node.js prior to the "end-of-life" dates for their current server version.

  **We intend to drop support for Node.js 6.x in the next major version of Apollo Server.**

  For more information, see [PR #2054](https://github.com/apollographql/apollo-server/pull/2054) and [our documentation](https://www.apollographql.com/docs/apollo-server/v2/migration-file-uploads.html).

### v2.2.7

- `apollo-engine-reporting`: When multiple instances of `apollo-engine-reporting` are loaded (an uncommon edge case), ensure that `encodedTraces` are handled only once rather than once per loaded instance. [PR #2040](https://github.com/apollographql/apollo-server/pull/2040)

### v2.2.6

- `apollo-server-micro`: Set the `Content-type` to `text/html` for GraphQL Playground. [PR #2026](https://github.com/apollographql/apollo-server/pull/2026)

### v2.2.5

- Follow-up on the update to `graphql-playground-html` in previous release by also bumping the minor version of the `graphql-playground-react` dependency to `1.7.10` — which is the version requested from the from the CDN bundle by `graphql-playground-html`. [PR #2037](https://github.com/apollographql/apollo-server/pull/2037)

### v2.2.4

- Fix GraphQL Playground documentation scrolling bug in Safari by updating to latest (rebased) fork of `graphql-playground-html`. [PR #2037](https://github.com/apollographql/apollo-server/pull/2037)

### v2.2.3

- When `generateClientInfo` is not used to define the client name, client version and
client reference ID, Apollo Server will now default to the values present in the HTTP headers
of the request (`apollographql-client-name`, `apollographql-client-reference-id` and
`apollographql-client-version` respectively).  As a last resort, when those headers are not set,
the query extensions' `clientInfo` values will be used. [PR #1960](https://github.com/apollographql/apollo-server/pull/1960)

### v2.2.2

- Fixed TypeScript 2.2 compatibility via updated `apollo-tooling` dependency. [Issue #1951](https://github.com/apollographql/apollo-server/issues/1951) [`26d6c739`](https://github.com/apollographql/apollo-server/commit/26d6c739505b3112694e641c272c748ce38ba86b)
- Throw a more specific error when asynchronous introspection query behavior is detected. [PR #1955](https://github.com/apollographql/apollo-server/pull/1955)

### v2.2.1

- Added support for an array of `modules` on the `ApolloServer` constructor options.  Each element of the `modules` can point to a module which exports `typeDefs` and `resolvers`.  These modules can be used in lieu of, or in combination with, directly specifying `schema` or `typeDefs`/`resolvers` on the constructor options.  This provides greater modularity and improved organization for logic which might be limited to a specific service. [`8f6481e6`](https://github.com/apollographql/apollo-server/commit/8f6481e60f8418738f9ebbe9d5ab5e7e2ce4d319).
- Added `resolveObject` support to query execution.  [`bb67584`](https://github.com/apollographql/apollo-server/commit/bb67584a224843a5b2509c2ebdd94e616fe6227c).
- Fix broken `apollo-server-cloud-functions` in 2.2.0 caused by missing TypeScript project references which resulted in the package not being published to npm in compiled form. [PR #1948](https://github.com/apollographql/apollo-server/pull/1948)

### v2.2.0

- New request pipeline, including support for plugins which can implement lifecycle hooks at various stages of a request. [PR #1795](https://github.com/apollographql/apollo-server/pull/1795).
- Introduce new `apollo-server-testing` utilities. [PR #1909](https://github.com/apollographql/apollo-server/pull/1909)
- Fix mocks configuration to allow disabling of mocks by using `mocks: false`, even if `mockEntireSchema` is `true`. [PR #1835](https://github.com/apollographql/apollo-server/pull/1835)
- Update `graphql-playground-html` to 1.7.8. [PR #1855](https://github.com/apollographql/apollo-server/pull/1855)
- Bring back Azure functions support [Issue #1752](https://github.com/apollographql/apollo-server/issue/1752) [PR #1753](https://github.com/apollographql/apollo-server/pull/1753)
- Allow an optional function to resolve the `rootValue`, passing the `DocumentNode` AST to determine the value. [PR #1555](https://github.com/apollographql/apollo-server/pull/1555)
- Follow-up on the work in [PR #1516](https://github.com/apollographql/apollo-server/pull/1516) to also fix missing insertion cursor/caret when a custom GraphQL configuration is specified which doesn't specify its own `cursorShape` property. [PR #1607](https://github.com/apollographql/apollo-server/pull/1607)
- Azure functions support [Issue #1752](https://github.com/apollographql/apollo-server/issue/1752) [PR #1753](https://github.com/apollographql/apollo-server/pull/1753) [PR #1948](https://github.com/apollographql/apollo-server/pull/1948)
- Allow JSON parsing in `RESTDataSource` of Content Type `application/hal+json`. [PR #185](https://github.com/apollographql/apollo-server/pull/1853)
- Add support for a `requestAgent` configuration parameter within the `engine` configuration.  This can be utilized when a proxy is necessary to transmit tracing and metrics data to Apollo Engine.  It accepts either an [`http.Agent`](https://nodejs.org/docs/latest-v8.x/api/http.html#http_class_http_agent) or [`https.Agent`](https://nodejs.org/docs/latest-v8.x/api/https.html#https_class_https_agent) and behaves the same as the `agent` parameter to Node.js' [`http.request`](https://nodejs.org/docs/latest-v8.x/api/http.html#http_http_request_options_callback). [PR #1879](https://github.com/apollographql/apollo-server/pull/1879)
- Allow an optional parameter to the `RESTDataSource` constructor which takes a `node-fetch`-compatible `fetch` implementation that will be used for HTTP calls instead of the default fetch. [PR #1807](https://github.com/apollographql/apollo-server/pull/1807)

### v2.1.0

- Updated the google-cloud-functions package to handle null paths [PR #1674](https://github.com/apollographql/apollo-server/pull/1674)
- Update link inside Authentication Docs [PR #1682](https://github.com/apollographql/apollo-server/pull/1682)
- Fix making sure all headers are getting reported to Engine properly when using `privateHeaders` [PR #1689](https://github.com/apollographql/apollo-server/pull/1689)
- _(experimental, subject to change/removal)_ Provide ability to specify client info in traces [#1631](https://github.com/apollographql/apollo-server/pull/1631)

### v2.0.8

- Reporting: Catch Error if JSON.Stringify Fails for Engine Trace [PR #1668](https://github.com/apollographql/apollo-server/pull/1668)
- Core: Allow context to be passed to all GraphQLExtension methods. [PR #1547](https://github.com/apollographql/apollo-server/pull/1547)

### v2.0.7

- Fix [#1581](https://github.com/apollographql/apollo-server/issues/1581) `apollo-server-micro` top level error response [#1619](https://github.com/apollographql/apollo-server/pull/1619)
- Switch `ApolloServerBase.schema` from private access to protected access. [#1610](https://github.com/apollographql/apollo-server/pull/1610)
- Add toggle for including error messages in reports [#1615](https://github.com/apollographql/apollo-server/pull/1615)
- Fix `apollo-server-cloud-functions` tests [#1611](https://github.com/apollographql/apollo-server/pull/1611/)

### v2.0.6

- Update `graphql-playground-html` to 1.7.4 [#1586](https://github.com/apollographql/apollo-server/pull/1586)
- Add support for `graphql-js` v14 by augmenting typeDefs with the `@cacheControl` directive so SDL validation doesn't fail [#1595](https://github.com/apollographql/apollo-server/pull/1595)
- Add `node-fetch` extensions typing to `RequestInit` [#1602](https://github.com/apollographql/apollo-server/pull/1602)

### v2.0.5

- Google Cloud Function support [#1402](https://github.com/apollographql/apollo-server/issues/1402) [#1446](https://github.com/apollographql/apollo-server/pull/1446)
- Switch to a fork of `apollo-upload-server` to fix missing `core-js` dependency. [#1556](https://github.com/apollographql/apollo-server/pull/1556)

### v2.0.4

- apollo-server: Release due to failed build and install

### v2.0.3

- apollo-server: failed publish
- pass payload into context function for subscriptions [#1513](https://github.com/apollographql/apollo-server/pull/1513)
- Add option to mock the entire schema(i.e. sets preserveResolvers) [PR #1546](https://github.com/apollographql/apollo-server/pull/1546)

### v2.0.2

- Release with Lerna 3 due
- Hapi: Allow additional route options to be passed to Hapi.js plugin. [PR #1384](https://github.com/apollographql/apollo-server/pull/1384)
- express, koa: remove next after playground [#1436](https://github.com/apollographql/apollo-server/pull/1436)
- Hapi: Pass the response toolkit to the context function. [#1407](https://github.com/apollographql/apollo-server/pull/1407)
- update apollo-engine-reporting-protobuf to non-beta [#1429](https://github.com/apollographql/apollo-server/pull/1429)
- playground would use its own settings as default [#1516](https://github.com/apollographql/apollo-server/pull/1516)
- Lambda: Look in event.path first when picking endpoint for GraphQL Playground [#1527](https://github.com/apollographql/apollo-server/pull/1527)
- Fix to allow enabling GraphQL Playground in production with custom config [#1495](https://github.com/apollographql/apollo-server/pull/1495)

### v2.0.1

- This version failed to publish fully/correctly and should not be used.

### v2.0.0-rc.10

- Fix and Export Extension and Playground Types [#1360](https://github.com/apollographql/apollo-server/pull/1360)
- Pin internal dependencies [#1361](https://github.com/apollographql/apollo-server/pull/1361)

### v2.0.0-rc.9

- This version failed to publish fully/correctly and should not be used.

### v2.0.0-rc.8

- export GraphQLUpload from integrations [#1322](https://github.com/apollographql/apollo-server/pull/1322)
- add `cors` to vanilla [#1335](https://github.com/apollographql/apollo-server/pull/1335)
- export `bodyParser.Options` to koa [#1334](https://github.com/apollographql/apollo-server/pull/1334)
- add and use playground in ApolloServer constructor [#1297](https://github.com/apollographql/apollo-server/pull/1297)
- **breaking**: remove calculate headers as function [#1337](https://github.com/apollographql/apollo-server/pull/1337)
- **breaking**: remove `formatParams` [#1331](https://github.com/apollographql/apollo-server/pull/1331)

### v2.0.0-rc.7

- enable engine reporting from lambda [#1313](https://github.com/apollographql/apollo-server/pull/1313)
- remove flattening of errors [#1288](https://github.com/apollographql/apollo-server/pull/1288)
- dynamic url in datasourece ([#1277](https://github.com/apollographql/apollo-server/pull/1277))

### v2.0.0-rc.6

- BREAKING: errors are passed to user extensions, then engine reporting, and finally `formatError` ([#1272](https://github.com/apollographql/apollo-server/pull/1272))
- `formatError` only called once on validation errors ([#1272](https://github.com/apollographql/apollo-server/pull/1272))
- BREAKING: apollo-server-env does place types in global namespace ([#1259](https://github.com/apollographql/apollo-server/pull/1259))
- export Request from apollo-datasource-rest and graphql-extensions (53d7a75 c525818)
- Use scoped graphql-playground and centralize version (8ea36d8, 84233d2)
- fix dependencies + exports ([#1257](https://github.com/apollographql/apollo-server/pull/1257))
- fix data source + context cloning (7e35305)
- use fetch instead of Node request for engine-reporting ([#1274](https://github.com/apollographql/apollo-server/pull/1274))

### v2.0.0-rc.5

- fix formatError to keep prototype of Error ([#1235](https://github.com/apollographql/apollo-server/pull/1235))

### v2.0.0-rc.4

- Add trailing slash to data source
- allow body passed to data source
- new apollo-engine-reporting agent

### v2.0.0-rc.3

- graphql as peerDependency ([#1232](https://github.com/apollographql/apollo-server/pull/1232))
- APQ in batches ([#1234](https://github.com/apollographql/apollo-server/pull/1234))
- APQ hits/misses in traces

### v2.0.0-rc.2

- Missing apollo-upload-server dependency ([#1221](https://github.com/apollographql/apollo-server/pull/1221))
- encode trace report over each request in apollo-engine-reporting

### v2.0.0-rc.1

- BREAKING: remove logFunction ([71a403d](https://github.com/apollographql/apollo-server/pull/1125/commits/71a403dfa38ee050606d3fa32630005e0a98016f)), see [this commit](https://github.com/apollographql/apollo-server/blob/8914b135df9840051fe81cc9224b444cfc5b61ab/packages/apollo-server-core/src/logging.ts) for an implementation
- move upload option to constructor ([#1204](https://github.com/apollographql/apollo-server/pull/1204))
- fixed hapi gui bugs ([#1211](https://github.com/apollographql/apollo-server/pull/1211))
- remove requirement for exModuleInterop ([#1210](https://github.com/apollographql/apollo-server/pull/1210))
- change BadUserInputError to UserInputError ([#1208](https://github.com/apollographql/apollo-server/pull/1208))
- add cache-control headers for CDN integration ([#1138](https://github.com/apollographql/apollo-server/pull/1138))
- Lambda support (thanks to @adnsio, @bwlt, and @gragio [#1138](https://github.com/apollographql/apollo-server/pull/1138))

Data sources

- add memcache and redis support ([#1191](https://github.com/apollographql/apollo-server/pull/1191))
- add patch method ([#1190](https://github.com/apollographql/apollo-server/pull/1190))

### v2.0.0-rc.0

- Breaking: `registerServer` changed to `server.applyMiddleware` ([3279991](https://github.com/apollographql/apollo-server/pull/1125/commits/327999174cfbcecaa4e401ffd7b2d7148ba0fd65))
- Breaking: subscriptions enabled with `installSubscriptionHandlers`
- Add Data Sources ([#1163](https://github.com/apollographql/apollo-server/pull/1163))

### v2.0.0-beta.4

- Bug fix to allow async context ([#1129](https://github.com/apollographql/apollo-server/pull/1129))
- logFunction is now an extension ([#1128](https://github.com/apollographql/apollo-server/pull/1128))
- Allow user defined extensions and include engine reporting ([#1105](https://github.com/apollographql/apollo-server/pull/#105))

### v2.0.0-beta.3

- remove registerServer configuration from `apollo-server`'s listen ([#1090](https://github.com/apollographql/apollo-server/pull/1090))
- move healthcheck into variants ([#1086](https://github.com/apollographql/apollo-server/pull/1086))
- Add file uploads, **breaking** requires removing `scalar Upload` from the typeDefs ([#1071](https://github.com/apollographql/apollo-server/pull/1071))
- Add reporting to Engine as apollo-engine-reporting ([#1105](https://github.com/apollographql/apollo-server/pull/1105))
- Allow users to define extensions ([#1105](https://github.com/apollographql/apollo-server/pull/1105))

### v2.0.0-beta.2

ListenOptions:

- `engine` -> `engineProxy`
- `port`, `host`, and other http options moved under `http` key ([#1080](https://github.com/apollographql/apollo-server/pull/1080))

- `subscriptions` moved to `server.listen` ([#1059](https://github.com/apollographql/apollo-server/pull/1059))
- Add mocks to server constructor ([#1017](https://github.com/apollographql/apollo-server/pull/1017))
- Add `bodyParserConfig` parameter to `registerServer` in apollo-server ([#1059](https://github.com/apollographql/apollo-server/pull/1059)) [commit](https://github.com/apollographql/apollo-server/pull/1063/commits/d08f862063b60f35d92f903c9ac52702150c10f6)
- Hapi variant ([#1058](https://github.com/apollographql/apollo-server/pull/1058)) ([#1082](https://github.com/apollographql/apollo-server/pull/1082))
- Remove tests and guaranteed support for Node 4 [PR #1024](https://github.com/apollographql/apollo-server/pull/1024)
- Cleanup docs [PR #1233](https://github.com/apollographql/apollo-server/pull/1233/files)

### 1.4.0

- [Issue #626] Integrate apollo-fastify plugin. [PR #1013](https://github.com/apollographql/apollo-server/pull/1013)
- add hapi 16 next() invocation [PR #743](https://github.com/apollographql/apollo-server/pull/743)
- Add skipValidation option [PR #839](https://github.com/apollographql/apollo-server/pull/839)
- `apollo-server-module-graphiql`: adds an option to the constructor to disable url rewriting when editing a query [PR #1047](https://github.com/apollographql/apollo-server/pull/1047)
- Upgrade `subscription-transport-ws` to 0.9.9 for Graphiql

### v1.3.6

- Recognize requests with Apollo Persisted Queries and return `PersistedQueryNotSupported` to the client instead of a confusing error. [PR #982](https://github.com/apollographql/apollo-server/pull/982)

### v1.3.5

- `apollo-server-adonis`: The `Content-type` of an operation response will now be correctly set to `application/json`. [PR #842](https://github.com/apollographql/apollo-server/pull/842) [PR #910](https://github.com/apollographql/apollo-server/pull/910)
- `apollo-server-azure-functions`: Fix non-functional Azure Functions implementation and update examples in Azure Functions' `README.md`. [PR #753](https://github.com/apollographql/apollo-server/pull/753) [Issue #684](https://github.com/apollographql/apollo-server/issues/684)
- Fix `TypeError` on GET requests with missing `query` parameter. [PR #964](https://github.com/apollographql/apollo-server/pull/964)
- The typing on the context of `GraphQLServerOptions` now matches the equivilent type used by `graphql-tools`. [PR #919](https://github.com/apollographql/apollo-server/pull/919)
- Middleware handlers now used named (rather than anonymous) functions to enable easier identification during debugging/profiling. [PR #827](https://github.com/apollographql/apollo-server/pull/827)
- The `npm-check-updates` package has been removed as a "dev dependency" which was resulting in an _older_ version of `npm` being used during testing. [PR #959](https://github.com/apollographql/apollo-server/pull/959)
- The typing on `HttpQueryRequest`'s `query` attribute now enforces that its object properties' keys be `String`s. [PR #834](https://github.com/apollographql/apollo-server/pull/834)
- TypeScript types have been updated via updates to `@types/node`, `@types/connect`, `@types/koa` and `@types/aws-lambda`.

### v1.3.4

- Upgrade to `apollo-cache-control@0.1.0` and allow you to specify options to it (such as the new `defaultMaxAge`) by passing `cacheControl: {defaultMaxAge: 5}` instead of `cacheControl: true`.

### v1.3.3

- Updated peer dependencies to support `graphql@0.13.x`.
- `apollo-server-express`: The `GraphQLOptions` type is now exported from `apollo-server-express` in order to facilitate type checking when utilizing `graphqlExpress`, `graphiqlExpress`, `graphqlConnect` and `graphiqlConnect`. [PR #871](https://github.com/apollographql/apollo-server/pull/871)
- Update GraphiQL version to 0.11.11. [PR #914](https://github.com/apollographql/apollo-server/pull/914)

### v1.3.2

- Updated peer dependencies and tests to support `graphql@0.12`.
- Fix issue where the core `runQuery` method broke the ability to use the Node `async_hooks` feature's call stack. [PR #733](https://github.com/apollographql/apollo-server/pull/733)
- Hoist declarations of rarely used functions out of `doRunQuery` to improve performance. [PR# 821](https://github.com/apollographql/apollo-server/pull/821)

### v1.3.1

- Fixed a fatal execution error with the new `graphql@0.12`.

### v1.3.0

- **Breaking:** `apollo-server-hapi`: now supports Hapi v17, and no longer supports Hapi v16.  For information on running Apollo Server 1.x with Hapi v16, [check this documentation](https://www.apollographql.com/docs/apollo-server/v1/servers/hapi.html#Hapi-16).
- **New package**: `apollo-server-adonis` supporting the Adonis framework!
- The `graphqlOptions` parameter to server GraphQL integration functions now accepts context as a function and as an object with a prototype. [PR #679](https://github.com/apollographql/apollo-server/pull/679)
- `apollo-server-express`: Send Content-Length header.
- `apollo-server-micro`: Allow Micro 9 in `peerDependencies`. [PR #671](https://github.com/apollographql/apollo-server/pull/671)
- GraphiQL integration:
  - Recognize Websocket endpoints with secure `wss://` URLs.
  - Only include truthy values in GraphiQL URL.

### v1.2.0

- **New feature**: Add support for Apollo Cache Control. Enable `apollo-cache-control` by passing `cacheControl: true` to your server's GraphQL integration function.
- Include README.md in published npm packages.

### v1.1.7

- Added support for the vhost option for Hapi [PR #611](https://github.com/apollographql/apollo-server/pull/611)
- Fix dependency on `apollo-tracing` to be less strict.

### v1.1.6

- GraphiQL integration: add support for `websocketConnectionParams` for subscriptions. [#452](https://github.com/apollographql/apollo-server/issues/452) [PR 548](https://github.com/apollographql/apollo-server/pull/548)

(v1.1.4 had a major bug and was immediately unpublished. v1.1.5 was identical to v1.1.6.)

### v1.1.3

- GraphiQL integration: Fixes bug where CORS would not allow `Access-Control-Allow-Origin: *` with credential 'include', changed to 'same-origin' [Issue #514](https://github.com/apollographql/apollo-server/issues/514)
- Updated peer dependencies to support `graphql@0.11`.

### v1.1.2

- Fixed bug with no URL query params with GraphiQL on Lambda [Issue #504](https://github.com/apollographql/apollo-server/issues/504) [PR #512](https://github.com/apollographql/apollo-server/pull/503)

### v1.1.1

- Added support for Azure Functions [#503](https://github.com/apollographql/apollo-server/pull/503)

### v1.1.0

- Added ability to provide custom default field resolvers [#482](https://github.com/apollographql/apollo-server/pull/482)
- Add `tracing` option to collect and expose trace data in the [Apollo Tracing format](https://github.com/apollographql/apollo-tracing)
- Add support for GraphiQL editor themes in [#484](https://github.com/apollographql/apollo-server/pull/484) as requested in [#444](https://github.com/apollographql/apollo-server/issues/444)
- Add support for full websocket using GraphiQL [#491](https://github.com/apollographql/graphql-server/pull/491)
- Updated restify lib ([@yucun](https://github.com/liyucun/)) in [#472](https://github.com/apollographql/apollo-server/issues/472)
- Updated package apollo-server-micro, updated micro in devDependencies and peerDependencies to ^8.0.1

### v1.0.3

- Revert [#463](https://github.com/apollographql/graphql-server/pull/463),
  because it's a breaking change that shouldn't have been a patch update.

### v1.0.2

- Rename packages from graphql-server- to apollo-server- [#465](https://github.com/apollographql/apollo-server/pull/465). We'll continue to publish `graphql-server-` packages that depend on the renamed `apollo-server-` packages for the time being, to ensure backwards compatibility.

### v1.0.1

- Fix Express package not calling the callback on completion ([@chemdrew](https://github.com/chemdrew)) in [#463](https://github.com/apollographql/graphql-server/pull/463)

### v1.0.0

- Add package readmes for Express, Hapi, Koa, Restify ([@helfer](https://github.com/helfer)) in [#442](https://github.com/apollographql/graphql-server/pull/442)
- Updated & fixed typescript typings ([@helfer](https://github.com/helfer)) in [#440](https://github.com/apollographql/graphql-server/pull/440)

### v0.9.0

- Allow GraphiQLOptions to be a function ([@NeoPhi](https://github.com/NeoPhi)) on [#426](https://github.com/apollographql/graphql-server/pull/426)

### v0.8.5

- Fix: graphql-server-micro now properly returns response promises [#401](https://github.com/apollographql/graphql-server/pull/401)

### v0.8.4

### v0.8.3

### v0.8.2

- Fix issue with auto-updating dependencies that caused fibers to update accidentally ([@helfer](https://github.com/helfer)) on [#425](https://github.com/apollographql/graphql-server/pull/425)

### v0.8.1

- **Security Fix** Ensure queries submitted via HTTP GET run through validation ([@DxCx](https://github.com/DxCx)) on [#424](https://github.com/apollographql/graphql-server/pull/424)

### v0.8.0

- Persist `window.location.hash` on URL updates [#386](https://github.com/apollographql/graphql-server/issues/386)
- Added support for `graphql-js` > 0.10.0 [#407](https://github.com/apollographql/graphql-server/pull/407)
- Updated `subscriptions-transport-ws` for GraphiQL with subscriptions [#407](https://github.com/apollographql/graphql-server/pull/407)

### v0.7.2

- Fix include passHeader field that was accidentally removed

### v0.7.1

- Fix graphiql fetcher to use endpointURL parameter instead of hardcoded URI.[#365](https://github.com/apollographql/graphql-server/issues/356)

### v0.7.0

- Add Zeit Micro Integration [#324](https://github.com/apollographql/graphql-server/issues/324)
- add support for subscriptionURL to GraphiQL ([@urigo](https://github.com/urigo) on [#320](https://github.com/apollostack/graphql-server/pull/320)
- Restify: Fix for calling next() ([@jadkap](https://github.com/jadkap)) on [#285](https://github.com/apollostack/graphql-server/pull/285)
- **Breaking:** Update all dependencies [#329](https://github.com/apollographql/graphql-server/issues/329)

### v0.6.0

- Add AWS Lambda Integration [PR #247](https://github.com/apollostack/graphql-server/pull/247)
- Update GraphiQL to version 0.9.1 ([@ephemer](https://github.com/ephemer)) on [#293](https://github.com/apollostack/graphql-server/pull/293)
- **Restify integration** ([@joelgriffith](https://github.com/joelgriffith)) on [#189](https://github.com/apollostack/graphql-server/pull/189)
- run batched requests in parallel ([@DxCx](https://github.com/DxCx)) on [#273](https://github.com/apollostack/graphql-server/pull/273)
- Fix GraphiQL options variables. Issue #193. ([@alanchristensen](https://github.com/alanchristensen)) on
  [PR #255](https://github.com/apollostack/apollo-server/pull/255)
- Allow graphql@0.9.0 as peerDependency ([@Chris-R3](https://github.com/Chris-R3)) on [PR #278](https://github.com/apollostack/graphql-server/pull/278)

### v0.5.1

- add support for HTTP GET Method ([@DxCx](https://github.com/DxCx)) on [#180](https://github.com/apollostack/graphql-server/pull/180)

### v0.5.0

- Switch graphql typings for typescript to @types/graphql [#260](https://github.com/apollostack/graphql-server/pull/260)

### v0.4.4

- Update GraphiQL to version 0.8.0 ([@DxCx](https://github.com/DxCx)) on [#192](https://github.com/apollostack/graphql-server/pull/192)
- Upgrade to GraphQL-js 0.8.1.

### v0.4.2

- **Restructure Apollo Server into 6 new packages, and rename to GraphQL Server** ([@DxCx](https://github.com/DxCx)) and ([@stubailo](https://github.com/stubailo)) in [#183](https://github.com/apollostack/graphql-server/pull/183) and [#164](https://github.com/apollostack/graphql-server/pull/183).
- There are now 6 packages that make up the GraphQL server family:
  - `graphql-server-core`
  - `graphql-module-graphiql`
  - `graphql-module-operation-store`
  - `graphql-server-express`
  - `graphql-server-hapi`
  - `graphql-server-koa`
- Exports have been renamed. Everything that used to export `apollo*` now exports `graphql*`, for example `apolloExpress` has become `graphqlExpress`.
- The repository is now managed using [Lerna](https://github.com/lerna/lerna).

### v0.3.3

- Fix passHeader option in GraphiQL (Both Hapi and Koa)
- Pass `ctx` instead of `ctx.request` to options function in Koa integration ([@HriBB](https://github.com/HriBB)) in [PR #154](https://github.com/apollostack/apollo-server/pull/154)
- Manage TypeScript declaration files using npm. ([@od1k](https:/github.com/od1k) in [#162](https://github.com/apollostack/apollo-server/pull/162))
- Fix connect example in readme. ([@conrad-vanl](https://github.com/conrad-vanl) in [#165](https://github.com/apollostack/apollo-server/pull/165))
- Add try/catch to formatError. ([@nicolaslopezj](https://github.com/nicolaslopezj) in [#174](https://github.com/apollostack/apollo-server/pull/174))
- Clone context object for each query in a batch.

### v0.3.2

- Added missing exports for hapi integration ([@nnance](https://github.com/nnance)) in [PR #152](https://github.com/apollostack/apollo-server/pull/152)

### v0.3.1

- Fixed dependency issue with boom package that affected the hapi integration. ([@sammkj](https://github.com/sammkj) in [#150](https://github.com/apollostack/apollo-server/pull/150))

### v0.3.0

- Refactor Hapi integration to improve the API and make the plugins more idiomatic. ([@nnance](https://github.com/nnance)) in
  [PR #127](https://github.com/apollostack/apollo-server/pull/127)
- Fixed query batching with Hapi integration. Issue #123 ([@nnance](https://github.com/nnance)) in
  [PR #127](https://github.com/apollostack/apollo-server/pull/127)
- Add support for route options in Hapi integration. Issue #97. ([@nnance](https://github.com/nnance)) in
  [PR #127](https://github.com/apollostack/apollo-server/pull/127)
- Camelcase Hapi. Issue #129. ([@nnance](https://github.com/nnance)) in
  [PR #132](https://github.com/apollostack/apollo-server/pull/132)
- Fix error handling when parsing variables parameter. Issue #130. ([@nnance](https://github.com/nnance)) in
  [PR #131](https://github.com/apollostack/apollo-server/pull/131)
- Improve logging function. Issue #79. ([@nnance](https://github.com/nnance)) in
  [PR #136](https://github.com/apollostack/apollo-server/pull/136)
- Output stack trace for errors in debug mode. Issue #111. ([@nnance](https://github.com/nnance)) in
  [PR #137](https://github.com/apollostack/apollo-server/pull/137)
- Allow to pass custom headers in GraphiQL ([@nicolaslopezj](https://github.com/nicolaslopezj) in [#133](https://github.com/apollostack/apollo-server/pull/133)).

### v0.2.6

- Expose the OperationStore as part of the public API. ([@nnance](https://github.com/nnance))
- Support adding parsed operations to the OperationStore. ([@nnance](https://github.com/nnance))
- Expose ApolloOptions as part of the public API.

### v0.2.5

- Made promise compatible with fibers ([@benjamn](https://github.com/benjamn) in [#92](https://github.com/apollostack/apollo-server/pull/92))

### v0.2.2

- Log server events such as request start etc. with logFunction ([@helfer](https://github.com/helfer) in [#78](https://github.com/apollostack/apollo-server/pull/78))

### v0.2.1

- Complete refactor of Apollo Server using TypeScript. PR [#41](https://github.com/apollostack/apollo-server/pull/41)
- Added Hapi integration ([@nnance](https://github.com/nnance) in [#46](https://github.com/apollostack/apollo-server/pull/46))
- Added Koa integration ([@HriBB](https://github.com/HriBB) in [#59](https://github.com/apollostack/apollo-server/pull/59))
- Changed express integration to support connect as well ([@helfer](https://github.com/helfer) in [#58](https://github.com/apollostack/apollo-server/pull/58))
- Dropped express-graphql dependency
- Dropped support for GET requests, only POST requests are allowed now
- Split GraphiQL into a separate middleware
- Factored out core to support Hapi, Koa and connect implementations
- Added support for query batching
- Added support for query whitelisting / stored queries
- Removed body parsing from express integration. Body must be parsed outside of apollo now
- Added `formatRequest` and `formatResponse` functions to apollo options.
- Removed support for shorthand schema definitions, connectors and mocks (use `graphql-tools` instead)

### v0.1.5

- BUG: Fixed a spelling error with `tracer.submit()` from PR [#26](https://github.com/apollostack/apollo-server/pull/26)
  in PR [#31](https://github.com/apollostack/apollo-server/pull/31)

### v.0.1.4

- BUG: Fixed a bug with tracer mocks that would throw a TypeError when using Ava [#26](https://github.com/apollostack/apollo-server/pull/26)

### v0.1.3

- Updated graphql dependency to 0.6.0
