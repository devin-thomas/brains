---
brain: matt-pocock
topic: typescript-practice
sources: [the-tsconfig-cheat-sheet, tsconfig-cheat-sheet, no-unchecked-indexed-access, js-files-in-typescript-why, import-vs-require-the-biggest-javascript-divide, node-23-fixes-the-worst-thing-about-javascript, it-finally-happened, you-can-do-that-in-node-2, typescript-shipped-a-flag-to-disable-enums, live-anders-hejlsberg-typescript-go-port, typescript-performance-with-aleksandra-sikora, typescript-is-10x-faster-on-a-1-5m-loc-codebase, blazing-fast-tips-publishing-to-npm, the-npm-library-speedrun-90-minutes-to-build-ci-and-publish, preconstruct-is-amazing-for-bundling-typescript-libraries, my-as-few-deps-as-possible-monorepo-setup, how-to-create-an-npm-package, typescript-node-esbuild-commander-lets-build-a-cli, testing-in-typescript-sucks-lets-make-it-better, coding-road-trip-driving-truck-typescript, dont-put-your-types-in-dts-files, dont-put-your-types-in-dts-files-typescript, where-to-put-your-types-in-application-code, declaring-globals-isnt-that-hard, does-ts-reset-fix-typescript, youre-typing-process-env-wrong, how-to-strongly-type-process-env, when-should-you-use-zod, what-is-zod-typescript, zod-4-is-out-and-it-is-crazy, everyones-talking-about-valibot, open-source-libraries-you-have-to-know-valibot, everyones-talking-about-arktype, open-source-libraries-you-have-to-know-arktype, learn-trpc-in-5-minutes, what-is-trpc, codegen-the-good-the-bad-and-the-ugly, total-typescript-tour-and-ama, blazing-fast-tips-react-typescript, react-fc-used-to-be-crap-now-its-fine, reacts-most-useful-types-matt-pocock-react-day-berlin-2023, understanding-reacts-types, dan-abramov-asked-me-to-help-fix-react, reacts-typescript-types-with-sebastian-silbermann-maintain-podcast-1, typescript-just-unblocked-react-server-components, most-react-devs-dont-understand-generic-components, rethink-your-life-choices-before-you-do-this, typing-createcontext-in-react-typescript-react, stop-using-defaultprops-react-18-3-preview, fix-your-userefs-with-elementref, jsxelement-vs-react-reactnode-typescript-react, super-nice-pattern-for-react-props, i-hate-it-when-ts-devs-do-this, so-many-typescript-devs-make-this-mistake, typescript-loves-errors, the-most-common-typescript-error, ending-every-typescript-debate-in-90-minutes, solving-the-any-problem-is-harder-than-you-think, why-does-any-exist-in-typescript-typescript, any-considered-harmful, the-typescript-hill-im-willing-to-die-on, typescript-and-career-interview-with-tejas-kumar, hunting-for-anys-in-sentrys-codebase, effect-the-unreadable-library-that-captured-my-heart, template-literal-types-for-error-codes-are-awesome, should-you-use-return-types, return-types-vs-inference-typescript, why-typescript-migrations-fail, dejavue-e023-typescript-and-content-creation-with-matt-pocock, typescript-react-and-api-issues-with-matt-pocock, to-typescript-or-not-to-typescript-jsj-538, jsdoc-vs-typescript-typescript, typescript-vs-javascript, why-you-should-migrate-to-typescript, will-ts-or-js-win-our-ai-future, typescript-vs-eslint-typescript-javascript, typescript-version-mismatches-are-a-pita-typescript, the-ts-playground-has-plugins, enums-considered-harmful, types-vs-interfaces-what-i-got-wrong-in-2022, typescript-types-or-interfaces-deprecated-2022, typescript-5-0-beta-is-here, typescript-5-3-first-look, typescript-5-5-is-a-banger, force-claude-code-to-use-the-right-cli, ship-working-code-while-you-sleep-with-the-ralph-wiggum-technique, implement]
drafted_by: sonnet
reviewed: 2026-10-24 by sonnet lead pass (citation check with tools/check_citations.py; claims spot-checked against corrected transcripts; not every claim re-read against its source)
---

# Practical TypeScript: setup, packages, React, errors, migration, tooling (after Matt Pocock)

Matt Pocock treats TypeScript first as tooling: autocomplete, rename, inline errors, and a short feedback loop, with type safety as a means to that. [typescript-vs-javascript @ 00:00:33] This file gathers what the corpus records about the practical side of using it: configuring a project, building and publishing packages, validating data at the edges, typing React, reading and handling errors, migrating a JavaScript codebase, and the surrounding tooling. It restates his positions with dates, because several of them changed between 2022 and 2025. Everything cited is a dated source note in `corpus/youtube/` or a research note in `corpus/typescript/`; nothing here comes from a note that was not read.

Citation form: a corpus slug, then either an at-sign and a timestamp (the bullet in that video note whose range covers the time) or a section sign and a heading (`Detailed source notes` for the short paragraph-style notes, `Research note` for the article notes), all inside square brackets. Statements marked *Inference:* appear only under "Applying this" and are ours, not his.

## Where he draws the line between three kinds of TypeScript

Matt separates application code, library code, and the shared code inside an application (a utils folder used by many parts). [application-vs-library-ts-typescript § Detailed source notes] Application code that mostly imports from libraries is the simple level, library code needs generics and other advanced techniques to stay general, and the shared middle layer is the one he says people talk about less, even though it can become complex and is a good place to look when improving an app's types. [application-vs-library-ts-typescript § Detailed source notes] The same split drives most of his practical advice below: rules that suit a library (explicit return types, exported types, no enums) are not the rules he gives for application leaves.

## tsconfig and compiler settings

### The cheat sheet (2023)

His central statement is the TSConfig Cheat Sheet, published as an article on 2023-09-13 and as a video on 2023-10-11. [tsconfig-cheat-sheet § Research note] [the-tsconfig-cheat-sheet @ 00:00:02] The pitch is that `tsconfig.json` intimidates people because TypeScript changes slowly and its defaults changed, so he offers the set that needs the fewest decisions. [the-tsconfig-cheat-sheet @ 00:00:02] The article groups the options by strictness, transpilation, library building, monorepos, and browser versus non-browser runtimes. [tsconfig-cheat-sheet § Research note]

The base options he says to set on every project:

- Set `esModuleInterop` to true, which smooths the boundary between ESM and CJS, and set `skipLibCheck` so declaration files you do not control in `node_modules` do not produce errors in your code. [the-tsconfig-cheat-sheet @ 00:00:35]
- Pick any `target`, with ES2022 a good choice because most browsers support its language features. [the-tsconfig-cheat-sheet @ 00:00:55]
- Turn on `allowJs` and `resolveJsonModule` to import JS and JSON files, and force module detection so every file is a module rather than a global script. [the-tsconfig-cheat-sheet @ 00:01:10]
- Use `strict: true`, since libraries assume it; leave out `noUnusedLocals` and `noUnusedParameters`, which he calls noise; add `noUncheckedIndexedAccess` for safer record and array indexing. [the-tsconfig-cheat-sheet @ 00:01:30]

### The first decision: do you transpile with tsc?

The first real decision is whether TypeScript itself emits the JavaScript. [the-tsconfig-cheat-sheet @ 00:02:10] If it does, he gives four emit options: `moduleResolution` and `module` both `NodeNext`, `outDir` set to `dist`, and `sourceMap` true. [the-tsconfig-cheat-sheet @ 00:02:10] `NodeNext` is the setting that makes emitted code consumable by Node, and it is the only case where he says you need it. [the-tsconfig-cheat-sheet @ 00:02:30]

If something else runs the code (Deno, Bun, TSX, esbuild, or a frontend framework such as Next, Remix or Svelte), he says to use `bundler` resolution with `module` set to `ESNext` and `noEmit` true, so type checking never writes JS. [the-tsconfig-cheat-sheet @ 00:02:50] He estimates that 80 to 90 percent of viewers will land on `moduleResolution: bundler`. [the-tsconfig-cheat-sheet @ 00:03:40]

Remaining pieces: an output directory so emit does not litter the source tree, and source maps so go-to-definition works better. [the-tsconfig-cheat-sheet @ 00:03:20] `lib` should be ES2022 plus DOM and DOM.Iterable when the code runs in the DOM, ES2022 alone for a Node app, and a library bundle adds `declaration: true`, which applies only when TypeScript does the transpiling. [the-tsconfig-cheat-sheet @ 00:04:00] In a monorepo, `composite: true` lets `tsc` rebuild only what changed, and `declarationMap: true` makes go-to-definition open source rather than a declaration file, set in the project that does the transpiling. [the-tsconfig-cheat-sheet @ 00:04:30] He notes the cheat sheet leaves out options such as `jsx` and still gets most projects 80 to 90 percent of the way there. [the-tsconfig-cheat-sheet @ 00:05:00]

### Starting from a community base

On a livestream in 2023-08 he built a CLI by copying the base config for the current Node LTS from the community `tsconfig/bases` repository, then summarised strict mode, `noUncheckedIndexedAccess`, `esModuleInterop`, `skipLibCheck` and the `include` setting. [typescript-node-esbuild-commander-lets-build-a-cli @ 00:20:40] In a monorepo setup shown in 2024-05, each package extends a shared tsconfig preset for a library monorepo and adds only `outDir` locally. [my-as-few-deps-as-possible-monorepo-setup @ 00:01:10]

TypeScript 5.5 added a `configDir` template variable so a shared base config can resolve an output path relative to each derived project; without it he had to specify `rootDir` and `outDir` in each package. [typescript-5-5-is-a-banger @ 01:09]

### Strictness and indexing

He treats strict mode as effectively mandatory. [ending-every-typescript-debate-in-90-minutes @ 00:08:00] In a 2022-12 beginner video he explains that `strict: true` turns on the recommended rules, recommends `noUncheckedIndexedAccess` as well, and says the only time to turn strict off is migrating a very large JavaScript codebase. [typescript-speedrun-crash-course-for-beginners @ 00:08:13] The most common error beginners hit is an implicit `any` on untyped parameters, which `strict` reports because it bundles the no-implicit-any rule. [the-most-common-typescript-error @ 00:10]

On `noUncheckedIndexedAccess`, his 2022 short shows indexing an object of string-keyed arrays and calling `push`, which fails at runtime without a guarantee that the key exists; turning the option on makes the indexed value possibly undefined, and initialising the key narrows it. [no-unchecked-indexed-access @ 00:44] He presents that as the better answer than writing a custom generic helper. [no-unchecked-indexed-access @ 00:44]

### File extensions and module resolution

Under `node16` and `nodenext`, relative imports need explicit extensions, and the working fix is to write `.js` even though the file on disk is `.ts`; `.ts` extensions need `allowImportingTsExtensions`. [js-files-in-typescript-why @ 00:00:31] His reasons: Node should not have to guess between `foo.js` and `foo/index.js`, and the compiled output is JavaScript, so only the `.js` paths will exist after compilation. [js-files-in-typescript-why @ 00:01:00] If you prefer extensionless imports, switch to `bundler` resolution, because bundlers such as esbuild and SWC resolve those paths. [js-files-in-typescript-why @ 00:02:43]

TypeScript 5.0 beta introduced the `bundler` option, which maps to modern bundlers such as Vite, esbuild, SWC and webpack. [typescript-5-0-beta-is-here @ 00:20] In a 2023-03 livestream he said none of the TSConfig options fully match what bundlers do and that module-resolution choices are painful. [total-typescript-tour-and-ama @ 00:29]

### ESM, CJS and what actually ships

Matt's 2023-09 explainer: ES modules can import CommonJS but not the reverse (apart from an asynchronous dynamic import), so package authors must ship for both, and using one package through both `require` and `import` can load two copies of its code, the dual package hazard. [import-vs-require-the-biggest-javascript-divide @ 01:30] The syntax you write does not decide the output; to know what ships, look at the built code, where `require` calls mean CommonJS and `import`/`export` means ESM. [import-vs-require-the-biggest-javascript-divide @ 02:12] His recommendation is to adopt ESM where possible, since much toolchain complexity exists to support two systems. [import-vs-require-the-biggest-javascript-divide @ 03:21]

### Performance settings

In a 2023-09 stream on TypeScript performance, the guest and Matt separated runtime performance from developer-experience performance, with the type checker as the bottleneck for editor responsiveness and build time. [typescript-performance-with-aleksandra-sikora @ 00:00] They measured with `extendedDiagnostics`, noted that a project far larger than expected can mean `node_modules` is being included, and used `--generateTrace` and `analyzeTrace` for hot spots. [typescript-performance-with-aleksandra-sikora @ 00:06] [typescript-performance-with-aleksandra-sikora @ 00:24:35] The stream's advice was to extract complex conditional types into aliases so TypeScript can cache them, and to avoid giant unions; Matt's own `skipLibCheck` advice is that it is his top performance tip. [typescript-performance-with-aleksandra-sikora @ 01:01:00] [dont-put-your-types-in-dts-files @ 00:01:00]

## Node, runtimes and running TypeScript directly

- 2024-07: on a livestream he explained that Deno, Bun and SWC-based tools must do more than strip types, because enums and parameter properties need compiling, and described the Node discussion about adding TypeScript support as tense and unfocused. [coding-road-trip-driving-truck-typescript @ 56:00]
- 2024-10: Node 23 lets CommonJS `require` an ES module, which he called one of the worst errors a Node developer could hit; Node 22 can get the same behaviour behind `--experimental-require-module`, and top-level `await` still breaks the case. [node-23-fixes-the-worst-thing-about-javascript @ 02:03] [node-23-fixes-the-worst-thing-about-javascript @ 02:43]
- 2025-01: Node runs `.ts` entry files without extra configuration, still experimentally; on Node 22 it needs the strip-types flag, while Node 23 does not. [it-finally-happened @ 00:00:01]
- 2025-06: he showed that on Node 24 the experimental warning was gone, with the caveats that imports must use the explicit TypeScript extension and enums and namespaces are unsupported because they need extra transformation. [you-can-do-that-in-node-2 @ 00:27] [you-can-do-that-in-node-2 @ 00:37]

TypeScript 5.8 added an erasable-syntax-only option, which errors on enums, runtime namespaces and constructor parameter properties, because Node's default type stripping does not transform them. [typescript-shipped-a-flag-to-disable-enums @ 00:20] [typescript-shipped-a-flag-to-disable-enums @ 02:00] Matt said he had previously recommended avoiding those features and that the flag was not something he influenced. [typescript-shipped-a-flag-to-disable-enums @ 01:40]

## Project setup, libraries and npm packages

### The 2023 recipe: pnpm, tsup, Changesets, GitHub Actions

His 2023-01 short on publishing to npm sets the default stack: pnpm installs TypeScript as a dev dependency, `tsc --init` creates the config with `noUncheckedIndexedAccess` and `noEmit` on so TypeScript acts as checker and linter, and `.gitignore` excludes `node_modules` and `dist`. [blazing-fast-tips-publishing-to-npm @ 00:00] tsup bundles the entry file to CommonJS and ES module outputs, with `main` pointing at the CJS file, `module` at the ESM file and `types` at the declaration file. [blazing-fast-tips-publishing-to-npm @ 00:36] A lint script runs `tsc`; Changesets records each change as a markdown file with its bump type; one GitHub Actions workflow runs lint and build on every push; a second workflow, on main only, uses the Changesets action to open a release pull request, and merging it publishes to npm. [blazing-fast-tips-publishing-to-npm @ 01:14] [blazing-fast-tips-publishing-to-npm @ 01:24] [blazing-fast-tips-publishing-to-npm @ 02:30]

The 2023-02 "90 minute" npm speedrun followed the same path: a pnpm and tsup scaffold with a CJS plus ESM build, Vitest with a watch script and a run-once script for CI, an npm access token stored as a repository secret, and Changesets with a publish workflow. [the-npm-library-speedrun-90-minutes-to-build-ci-and-publish @ 00:17:29] [the-npm-library-speedrun-90-minutes-to-build-ci-and-publish @ 00:21:26] [the-npm-library-speedrun-90-minutes-to-build-ci-and-publish @ 00:28:38] [the-npm-library-speedrun-90-minutes-to-build-ci-and-publish @ 00:32:01] The first publish failed with a private-package error, fixed by setting the Changesets access to public. [the-npm-library-speedrun-90-minutes-to-build-ci-and-publish @ 00:37:30] He also noted that an npm package cannot be unpublished cleanly. [the-npm-library-speedrun-90-minutes-to-build-ci-and-publish @ 01:49:21]

A 2024-08 article, "How to create an npm package", walks through repository setup, package metadata and licence files, compiler configuration, formatting, tests, GitHub Actions and Changesets-based releases, and favours a complete repeatable release setup over a minimal skeleton. [how-to-create-an-npm-package § Research note]

An earlier tool, Preconstruct, was shown in 2022-06 for preparing a package: install the CLI, run `preconstruct fix` to populate entry points, add an ESM output, and ignore the generated `dist`. [preconstruct-is-amazing-for-bundling-typescript-libraries § Detailed source notes] The note flags that this is a 2022 example that may not match current Preconstruct releases. [preconstruct-is-amazing-for-bundling-typescript-libraries § Detailed source notes]

### Monorepos

In 2024-05 he described a monorepo with "as few deps as possible": pnpm as package manager, TypeScript for linting and building, and Turborepo to coordinate tasks. [my-as-few-deps-as-possible-monorepo-setup @ 00:00:01] Each package's build runs `tsc` into `dist`, Turborepo builds dependencies first and caches results, the dev task is non-cached and persistent running `tsc` in watch mode, and apps link to packages through pnpm's `workspace:` protocol. [my-as-few-deps-as-possible-monorepo-setup @ 00:00:40] [my-as-few-deps-as-possible-monorepo-setup @ 00:02:30] [my-as-few-deps-as-possible-monorepo-setup @ 00:03:00] On a 2024-07 stream he said monorepos are worth the extra tooling because the front end and back end deploy as one unit, and that he prefers sharing types through a monorepo over generated types from a separate repository. [coding-road-trip-driving-truck-typescript @ 03:30] [coding-road-trip-driving-truck-typescript @ 1:10:00]

On project layout for learning material, he said such repositories are kept separate from the pnpm monorepo because each must be simple to install and read. [typescript-node-esbuild-commander-lets-build-a-cli @ 00:56:30]

### A CLI

His 2023-08 CLI stream: `npm init`, pnpm chosen over npm and Yarn, TypeScript as a dev dependency, a `bin` entry in `package.json`, a shebang using `/usr/bin/env node`, `tsc --watch` into a `dist` folder via `outDir`, `@types/node` for globals such as `console`, and Commander for subcommands. [typescript-node-esbuild-commander-lets-build-a-cli @ 00:17:00] [typescript-node-esbuild-commander-lets-build-a-cli @ 00:26:00] [typescript-node-esbuild-commander-lets-build-a-cli @ 00:31:00] [typescript-node-esbuild-commander-lets-build-a-cli @ 00:35:30] His production beginner-course CLI instead uses an esbuild build script, Commander, and Changesets with a GitHub Action. [typescript-node-esbuild-commander-lets-build-a-cli @ 01:01:45]

### Testing types and test helpers

A 2023-04 article compares testing library type behaviour with Vitest, small custom assertion helpers, and the dedicated `tsd` package. [how-to-test-your-types § Research note] In a 2023-03 stream he built test utilities such as `fromPartial`, which accepts a subset of the target type, to avoid `as` casts or large fake objects; he explained that `as` feels wrong because it does not express intent and needs the exact type written out. [testing-in-typescript-sucks-lets-make-it-better @ 09:00] [testing-in-typescript-sucks-lets-make-it-better @ 30:00] That stream also shows the repo setup with Turborepo, a main workflow and a Changesets publish workflow. [testing-in-typescript-sucks-lets-make-it-better @ 1:08:00]

### Where types live

- Do not put ordinary types in `.d.ts` files: in 2023-07 he showed that a `.d.ts` file is not a module, can hold only types, and puts declarations in global scope, so declaration files are for global alterations and interface augmentation. [dont-put-your-types-in-dts-files @ 00:01:00] [dont-put-your-types-in-dts-files @ 00:02:30] He reported that about 40% of nearly 10,000 respondents to his Twitter question put types in `.d.ts` files. [dont-put-your-types-in-dts-files @ 00:03:10] The 2023-12 short repeats the advice: types go in `.ts` files next to the runtime code or in their own file. [dont-put-your-types-in-dts-files-typescript § Detailed source notes]
- Placement follows reuse: beside a single use, in a shared location when several modules use it, in a shared package when several monorepo packages do. [where-to-put-your-types-in-application-code § Research note]
- For globals from third-party scripts, use `declare global` to augment `Window` rather than casting `window` to `any`; declaration merging works for interfaces and namespaces, not type aliases, and a file-local `declare const window` limits the change to one file. [declaring-globals-isnt-that-hard @ 00:00:41] [declaring-globals-isnt-that-hard @ 00:01:50] [declaring-globals-isnt-that-hard @ 00:02:53]
- ts-reset, released in 2023-02, is an import-once library of global fixes: `JSON.parse` returns `unknown` instead of `any`, and `filter(Boolean)` removes `undefined` from the array type. [does-ts-reset-fix-typescript @ 00:35] [does-ts-reset-fix-typescript @ 02:20] It is meant for application code, not library code, so consumers are not opted in without knowing. [does-ts-reset-fix-typescript @ 02:50]

## Environment variables and the runtime boundary

### process.env

His 2023-04 video "You're typing process.env wrong" starts from the fact that `process.env` is untyped, then makes the global `ProcessEnv` interface extend the inferred type of a Zod env schema, so keys are declared once and autocomplete on `process.env`. [youre-typing-process-env-wrong @ 00:40] [youre-typing-process-env-wrong @ 01:12] He names the limit himself: `process.env` values are always strings, so numeric or boolean keys do not fit, and an alternative is to export the parsed env object and use it app-wide. [youre-typing-process-env-wrong @ 02:03]

The 2024-04 article revisits it with two approaches: augmenting the global environment type, which helps the editor but does not validate real values, and validating at runtime with t3-env, which checks values at the boundary. [how-to-strongly-type-process-env § Research note] Which to use depends on whether compile-time declarations alone are enough. [how-to-strongly-type-process-env § Research note] The same global-augmentation technique can add keys to `process.env` or `import.meta`. [declaring-globals-isnt-that-hard @ 00:03:10]

### Validate at the edge

Matt's rule for Zod is organised by trust at the input boundary: validate untrusted external values, consider partly trusted integrations, and skip duplicate checks on values controlled inside the application. [when-should-you-use-zod § Research note] In 2024-07 he repeated that Zod belongs at system edges such as user input, webhooks and API responses, not throughout already typed internal code. [coding-road-trip-driving-truck-typescript @ 08:00] His short definition: a runtime library for validating values whose structure you do not know, such as a webhook payload or form submission, which also generates static types from the schemas. [what-is-zod-typescript @ 00:00:02] [what-is-zod-typescript @ 00:00:10] In 2023-03 he advised validating large external API objects with Zod at the boundary, generating types from OpenAPI or Swagger where available, and typing only the properties you need. [total-typescript-tour-and-ama @ 52]

Zod 4 (a 2025-04 beta announcement) is reported with parsing gains of 2.6x for strings, 3x for arrays and 7x for objects, a schema-extension case dropping from about 25,000 to about 1,000 type instantiations, and a small Zod Mini bundle; the note says these are figures Matt reported from shown examples. [zod-4-is-out-and-it-is-crazy @ 00:32] [zod-4-is-out-and-it-is-crazy @ 02:19]

### Other validation libraries

- Valibot, 2023-08: he liked Zod's chained builder because autocomplete leads to the right type, expected Valibot's separate imports to add friction, had not tested it, and said undercutting Zod on bundle size was a smart move. [everyones-talking-about-valibot @ 00:03:45] [everyones-talking-about-valibot @ 00:04:35] By 2025-06 he introduced Valibot as very small and tree-shakable, suited to front-end and serverless work where bundle size matters. [open-source-libraries-you-have-to-know-valibot § Detailed source notes]
- ArkType, 2023-04: he called the documentation poor and the project an alpha, and said he would not yet recommend it over Zod. [everyones-talking-about-arktype @ 00:02:00] By 2025-06 he called it the closest thing to running TypeScript types at runtime and a solid option for anyone able to leave Zod. [open-source-libraries-you-have-to-know-arktype @ 01:28]

### Typed APIs: tRPC, codegen, gql.tada

- tRPC: the router's types flow to the front end, but it only works when front end and back end are in the same repo and both TypeScript; otherwise he suggests a GraphQL server or a normal REST API. [what-is-trpc § Detailed source notes] Procedures take a Zod schema through `.input()`, and the context type is declared once at initialisation. [learn-trpc-in-5-minutes @ 03:06] [learn-trpc-in-5-minutes @ 04:07]
- Codegen (2023-03): the output requires a working understanding of TypeScript, types do not reach the editor until codegen reruns so he adds a watch script, and on large projects updates can take five to ten seconds. [codegen-the-good-the-bad-and-the-ugly @ 02:19] [codegen-the-good-the-bad-and-the-ugly @ 04:31] The cost does not stop him using codegen or Prisma, because those tools fill a gap with no real alternative. [codegen-the-good-the-bad-and-the-ugly @ 05:44]
- gql.tada (2024-02) derives result and variable types from a query string through a language-service plugin, with no separate CLI. [everyones-talking-about-gqltada @ 00:30] [everyones-talking-about-gqltada @ 01:00]

## React and TypeScript patterns

### The type vocabulary

In a 2023-12 conference talk, Matt said React ships no types of its own; they come from a separate package, with major versions synced to React's and patch releases able to ship independently. [reacts-most-useful-types-matt-pocock-react-day-berlin-2023 @ 01:00] His core points:

- `JSX.Element` is what TypeScript infers for a JSX expression, and it is too narrow for a prop or return that may also be a string, number, `null` or `undefined`. [reacts-most-useful-types-matt-pocock-react-day-berlin-2023 @ 02:58] [reacts-most-useful-types-matt-pocock-react-day-berlin-2023 @ 03:50]
- `React.ReactNode` covers every renderable value and is the type he needs about 99 percent of the time for slots that accept content. [reacts-most-useful-types-matt-pocock-react-day-berlin-2023 @ 04:15]
- For native element props he points to `JSX.IntrinsicElements` because it is globally available and avoids a conditional helper that costs TypeScript more; `React.ComponentProps` is for third-party components whose props are not exported. [reacts-most-useful-types-matt-pocock-react-day-berlin-2023 @ 08:20] [reacts-most-useful-types-matt-pocock-react-day-berlin-2023 @ 12:16]
- `React.ComponentType<P>` accepts class or function components, and `React.ElementType` suits `as`-prop and polymorphic patterns. [reacts-most-useful-types-matt-pocock-react-day-berlin-2023 @ 14:00] [reacts-most-useful-types-matt-pocock-react-day-berlin-2023 @ 16:00]
- `React.ElementRef<typeof Component>` gives the ref type for components you do not control. [reacts-most-useful-types-matt-pocock-react-day-berlin-2023 @ 18:58] The 2023-07 video makes the same point for typing `useRef` without looking up the DOM element type. [fix-your-userefs-with-elementref @ 00:00:01]

His 2024-02 short states the split plainly: `React.ReactNode` is anything React can render, `JSX.Element` only a JSX element. [jsxelement-vs-react-reactnode-typescript-react § Detailed source notes]

### Typing components

His 2023-01 React and TypeScript tips: a component is a function returning JSX so it types like any function; handler props should use React's event types; `ReactNode` for children; a type argument on `useState` constrains updates; `useRef` takes the element type; custom hooks returning a pair use a const assertion so TypeScript infers a tuple; default values by destructuring rather than `defaultProps`; context with a hook that throws when the provider is missing; a discriminated union when a prop belongs to one variant. [blazing-fast-tips-react-typescript @ 00:04] [blazing-fast-tips-react-typescript @ 01:00] [blazing-fast-tips-react-typescript @ 02:10] [blazing-fast-tips-react-typescript @ 03:45] He also recommended interfaces for component-library props because they can be extended, and string unions for autocomplete in application code. [blazing-fast-tips-react-typescript @ 00:04]

- Context: create it as user-or-null and read it only through a hook that throws when null, so consumers get the narrowed type and a missing provider fails visibly at runtime. [typing-createcontext-in-react-typescript-react @ 00:19] [typing-createcontext-in-react-typescript-react @ 00:26]
- Generic components: a table whose renderer is typed `any` loses the link to the row type; add a type parameter, constrained to an object type so the row can be spread. [most-react-devs-dont-understand-generic-components @ 00:43] [most-react-devs-dont-understand-generic-components @ 03:35]
- Polymorphic button-or-link: the large interface is the right answer only for a simple polymorphic component, so ask first whether the design is needed and otherwise split into separate components; where a union is used, an `as` prop makes it discriminated. [rethink-your-life-choices-before-you-do-this @ 00:01:35] [rethink-your-life-choices-before-you-do-this @ 00:03:15]
- Props namespace pattern (2024-06): put the props interface in a namespace named after the component so users reach it as `MyComponent.Props` without a separate import. [super-nice-pattern-for-react-props @ 00:06] [super-nice-pattern-for-react-props @ 00:18]
- Decoupling: do not derive a component's props from a database model, because that couples the front end to the data layer; pass the two fields directly. [i-hate-it-when-ts-devs-do-this § Detailed source notes] The counter-case from 2025-06 is two closely related definitions, such as an environment config object and a separately written union of its keys, which drift apart; derive the type with `keyof typeof` so one edit updates both. [so-many-typescript-devs-make-this-mistake @ 00:24] [so-many-typescript-devs-make-this-mistake @ 00:46]

### The history he tracks

- `React.FC` (2023-12): its return type used to reject strings and `undefined`; he calls it fine now, but prefers annotating a plain function's props, which he finds easier to turn into a generic component. [react-fc-used-to-be-crap-now-its-fine @ 00:00] [reacts-most-useful-types-matt-pocock-react-day-berlin-2023 @ 05:20]
- `defaultProps` (2022-11): he prefers destructured parameter defaults over assigning a property to a function, which is awkward in TypeScript. [stop-using-defaultprops-react-18-3-preview § Detailed source notes]
- `useRef` (2023-03): the `null` form makes `current` read-only in the types, which does not match runtime behaviour, and he reported changing the React type annotations after Dan Abramov confirmed the behaviour. [dan-abramov-asked-me-to-help-fix-react @ 00:00:23] [dan-abramov-asked-me-to-help-fix-react @ 00:02:41]
- Server components (2023-04): TypeScript 5.1 decoupled JSX element checking from tag types so async components work, and he called it a must-upgrade for React users. [typescript-just-unblocked-react-server-components @ 00:01:00] [typescript-just-unblocked-react-server-components @ 00:02:30]
- The React types are maintained by open-source contributors outside the React team, and the maintainer on the 2023-06 podcast said improvements that break many call sites are held for a React major. [understanding-reacts-types @ 00:09] [reacts-typescript-types-with-sebastian-silbermann-maintain-podcast-1 @ 23:00]

## Errors: reading them, suppressing them, handling them

### Reading compiler errors

He calls misreading errors the most common mistake and explains that the most specific information sits at the bottom of a long message; he recommends the TS Error Translator VS Code extension. [typescript-react-and-api-issues-with-matt-pocock @ 00:27:10] A free tutorial on reading the structure of an error and handling common ones followed in 2024-01. [new-free-total-typescript-tutorial-typescript @ 00:16]

### any, unknown and assertions

- 2023-02 verdict from the "90 minute" debate stream: enable the no-explicit-any lint rule in application code with rare, documented exceptions, and allow `any` in library code and in test suites. [ending-every-typescript-debate-in-90-minutes @ 01:08:00]
- Why `any` hurts most in shared utilities: in a group-by helper, `any` silences errors but the return stops being checked and the `any` spreads; `unknown` is safer but needs manual narrowing, so his conclusion is generics. [solving-the-any-problem-is-harder-than-you-think § Detailed source notes] His 2024-03 article adds that bounded uses exist where TypeScript needs a permissive constraint, such as generic constraints and conditional return types. [any-considered-harmful § Research note]
- Why it exists: `any` limits what TypeScript claims to cover, which lets it describe nearly all of JavaScript. [why-does-any-exist-in-typescript-typescript @ 00:25]
- Preferring a cast to a suppression: when hunting `any` in Sentry's codebase, he cast to the real motion style type instead of `as any`, which kept autocomplete working. [hunting-for-anys-in-sentrys-codebase @ 00:00:37]
- 2025-06 hill-to-die-on: of the cast, the expect-error directive and the ignore directive, avoid all where possible, but the cast is the narrowest because it affects one node; the directive cannot name the error it expects. [the-typescript-hill-im-willing-to-die-on @ 00:43] [the-typescript-hill-im-willing-to-die-on @ 01:51]
- In a 2024-11 interview he explained both directives suppress the following line and that `@ts-expect-error` also reports when the error disappears, while arguing a targeted `as any` can be narrower; these are his preferences, not policy. [typescript-and-career-interview-with-tejas-kumar @ 48:40]
- He treats `unknown` as right for public APIs and form data. [dejavue-e023-typescript-and-content-creation-with-matt-pocock @ 47:00]
- Narrowing by throwing early beats optional chaining for a missing DOM element, because the error appears where it went wrong and the type narrows below the guard. [typescript-loves-errors @ 00:16]

### Return types

2023-02: leave return types off simple functions, add them for functions with several branches and for libraries, where he says to export every type consumers need, and turn off the ESLint rule requiring them everywhere. [should-you-use-return-types @ 00:00:49] [should-you-use-return-types @ 00:02:32] The 2024-03 short says forced return types can be wider than the actual value, and a lint rule requiring them should be switched off. [return-types-vs-inference-typescript @ 00:19] In the debate stream his verdict was that requiring explicit return types by default is a bad rule, while they remain useful in libraries and widely used utilities. [ending-every-typescript-debate-in-90-minutes @ 01:35:30]

### Typed errors

On a 2025-06 video about Effect, Matt said the effect's type records both success value and possible errors, so callers see the failure types, and that dependency injection was what ended his scepticism. [effect-the-unreadable-library-that-captured-my-heart @ 02:00] [effect-the-unreadable-library-that-captured-my-heart @ 05:55] On the 2024-07 stream he said Effect suits backend services where robustness matters, not front-end code. [coding-road-trip-driving-truck-typescript @ 1:02:00] For application error codes, he showed a template literal type that combines a feature with an error type (joined by a colon), so adding an error to the error union makes it reportable on each feature. [template-literal-types-for-error-codes-are-awesome @ 00:00:01] [template-literal-types-for-error-codes-are-awesome @ 00:00:38]

## Migration and adoption

### Why move

- He says TypeScript was the most commonly used language in GitHub pull requests when he made his 2022-11 case: errors in the editor, wrong arguments caught, library and third-party types, and `as any` as an escape hatch. [why-you-should-migrate-to-typescript @ 00:00:02] [why-you-should-migrate-to-typescript @ 00:05:40]
- 2023-12: he recommends TypeScript over JavaScript because it brings current tooling, and because types state intent that code alone does not show, which he argued also helps AI assistants. [typescript-vs-javascript @ 00:00:50] [will-ts-or-js-win-our-ai-future @ 00:18]
- 2024-01: he frames TypeScript as one contributor to the delivery loop, strengthening the CI stage rather than delivering a measured speedup. [does-typescript-help-you-ship-faster-typescript § Detailed source notes] On the 2023-04 panel he said it roughly doubled his team's velocity by removing a class of bugs. [to-typescript-or-not-to-typescript-jsj-538 @ 02:20]

### How to migrate

- 2023-05 short: migrations fail for two reasons, scope (one massive pull request) and order (converting a file while the files it imports are still JavaScript forces loose escape hatches). [why-typescript-migrations-fail § Detailed source notes]
- 2023-02 debate stream: pick a strictness level and migrate file by file, starting high in the dependency tree with utilities and widely imported modules, rather than turning TypeScript on everywhere loosely and tightening later in one large change. [ending-every-typescript-debate-in-90-minutes @ 01:30:00]
- 2023-08 podcast: type core utilities and API boundaries first and leave components in JavaScript; JSDoc comments can provide some benefit. [typescript-react-and-api-issues-with-matt-pocock @ 00:22:30]
- 2024-08 podcast: set the strictness level first, then convert one file at a time, starting with files that have no dependencies and that others depend on, with Sentry's gradual conversion as the example. [dejavue-e023-typescript-and-content-creation-with-matt-pocock @ 21:00]
- Strict can be turned off only for migrating a very large JavaScript codebase. [typescript-speedrun-crash-course-for-beginners @ 00:08:13]

### JSDoc and types as comments

In the 2023-04 panel he said he wants JSDoc in every project for type hints without TypeScript syntax, while the guest pointed out the checking still comes from TSC. [to-typescript-or-not-to-typescript-jsj-538 @ 05:00] By 2024-01 his short says JSDoc keeps files as JavaScript with no emit step but editor checking still relies on `tsconfig.json` and the compiler. [jsdoc-vs-typescript-typescript § Detailed source notes] In 2024-11 he said he favours annotations that tooling checks but the runtime ignores, to cut the separate toolchain. [typescript-and-career-interview-with-tejas-kumar @ 57:07]

## Tooling and ecosystem

- ESLint checks one file at a time and is therefore fast; TypeScript needs the whole project and so is slower, and TypeScript leaves syntax and formatting to ESLint. [typescript-vs-eslint-typescript-javascript @ 00:01] [typescript-vs-eslint-typescript-javascript @ 00:15] [typescript-vs-eslint-typescript-javascript @ 00:27]
- VS Code may use its bundled TypeScript version rather than the project's, so set `typescript.tsdk` to the workspace copy; this is the 2024-01 version-mismatch short. [typescript-version-mismatches-are-a-pita-typescript § Detailed source notes]
- The TypeScript Playground has plugins, including Prettier and a link shortener. [the-ts-playground-has-plugins @ 00:59] [the-ts-playground-has-plugins @ 01:40]
- `satisfies` (shipped in TypeScript 4.9; explained 2024-01-01): use it to check a value against a shape while keeping its narrow type, as with config objects, and use a variable annotation when you want the wider type. [most-ts-devs-dont-understand-satisfies § Detailed source notes]
- TypeScript 5.3 previews (2023-08): isolated declarations should speed type checking in monorepos by requiring explicit annotations on exported declarations; 5.5 (2024-06) adds isolated declaration generation, which he expected to help simpler packages first and inference-heavy libraries such as Zod, XState and tRPC less. [typescript-5-3-first-look @ 00:02:20] [typescript-5-5-is-a-banger @ 02:23]
- The Go port: in 2025-03 Anders Hejlsberg said the compiler compiled very large projects like VS Code about ten times faster, and in 2025-05 Matt measured 67 seconds versus 7.7 seconds on a codebase of about 1.5 million lines. [live-anders-hejlsberg-typescript-go-port @ 00:01:00] [typescript-is-10x-faster-on-a-1-5m-loc-codebase @ 00:00:01] [typescript-is-10x-faster-on-a-1-5m-loc-codebase @ 00:00:30] Hejlsberg also asked the community to write responsible types, since users can spend the speedup on more complex ones. [live-anders-hejlsberg-typescript-go-port @ 00:45:00]

### Agent-era tooling

Two notes bear on practical setup. In 2026-02 he argued that a global `CLAUDE.md` rule cannot deterministically stop a command, so he turned "use pnpm, not npm" into a pre-tool-use hook that blocks npm, and suggested ESLint rules for style preferences. [force-claude-code-to-use-the-right-cli @ 02:09] [force-claude-code-to-use-the-right-cli @ 04:14] His `implement` skill text says to run typechecking regularly, single test files regularly, and the full suite once at the end. [implement § Source text] In the Ralph loop talk he asks for type checks, unit tests, green CI and end-to-end browser checks as feedback. [ship-working-code-while-you-sleep-with-the-ralph-wiggum-technique @ 10:19]

## Defaults he states, in one place

| Question | His default | Source |
| --- | --- | --- |
| `strict` | on | [the-tsconfig-cheat-sheet @ 00:01:30] |
| Emitting with tsc for Node | `NodeNext` | [the-tsconfig-cheat-sheet @ 00:02:30] |
| Anything else runs the code | `bundler` plus `noEmit` | [the-tsconfig-cheat-sheet @ 00:02:50] |
| Types in `.d.ts` files | no | [dont-put-your-types-in-dts-files-typescript § Detailed source notes] |
| Enums | avoid in new code | [enums-considered-harmful @ 00:07:30] |
| type vs interface for one object | does not matter | [types-vs-interfaces-what-i-got-wrong-in-2022 @ 00:09:16] |
| Interface `extends` vs intersections | prefer `extends` | [types-vs-interfaces-what-i-got-wrong-in-2022 @ 00:09:16] |
| Return types in app code | not by default | [should-you-use-return-types @ 00:04:00] |
| `React.ReactNode` for children | yes | [reacts-most-useful-types-matt-pocock-react-day-berlin-2023 @ 04:15] |
| Runtime validation | at the edges | [when-should-you-use-zod § Research note] |

## How Matt explains it

- He frames everything as a decision with a named trigger. The tsconfig cheat sheet is built as a single early fork (does TypeScript transpile?) with settings falling out of the answer. [the-tsconfig-cheat-sheet @ 00:02:06]
- He demonstrates the failure before the fix: untyped parameters, a missing key, a null element, a mutable ref. [the-most-common-typescript-error @ 00:00] [no-unchecked-indexed-access @ 00:00] [typescript-loves-errors @ 00:01]
- He separates audiences: library authors versus application developers versus the shared layer. [to-typescript-or-not-to-typescript-jsj-538 @ 17:00] [application-vs-library-ts-typescript § Detailed source notes]
- He runs verdict-style streams where each rule gets arguments from both sides and a ruling. [ending-every-typescript-debate-in-90-minutes @ 00:00:00]
- He builds in public and shows the failures: the first publish failed on a private-package error, the proxy debugging failed, and he says so. [the-npm-library-speedrun-90-minutes-to-build-ci-and-publish @ 00:37:30] [testing-in-typescript-sucks-lets-make-it-better @ 48:00]
- He frames contested calls, such as suppression directives, as his own preferences rather than universal policy. [typescript-and-career-interview-with-tejas-kumar @ 48:40]

## Changed over time

| Topic | Earlier | Later |
| --- | --- | --- |
| Types vs interfaces | 2022-12-05: moved through three phases, from interface-for-performance to preferring `type` unless an interface feature is needed. [typescript-types-or-interfaces-deprecated-2022 § Detailed source notes] | 2025-04-18: the 2022 performance advice conflated interfaces with intersections; prefer `interface extends` over intersections, and for a single object it does not matter. [types-vs-interfaces-what-i-got-wrong-in-2022 @ 00:00:44] |
| Enums | 2022-12-15: avoid in new code, flag in review, do not reject existing PRs. [enums-considered-harmful @ 00:07:30] | 2025-01-24: erasable-syntax flag lands; he says he had already recommended avoiding these features. [typescript-shipped-a-flag-to-disable-enums @ 01:40] |
| Node running TS | 2024-07-12: type-stripping talk was tense and unfocused. [coding-road-trip-driving-truck-typescript @ 56:00] | 2025-06-12: Node supports it out of the box, with caveats. [you-can-do-that-in-node-2 @ 00:53] |
| Package builds | 2023-01-14: tsup to CJS and ESM. [blazing-fast-tips-publishing-to-npm @ 00:36] | 2024-05-17: each package builds with `tsc` and Turborepo caches. [my-as-few-deps-as-possible-monorepo-setup @ 00:00:40] |
| Zod alternatives | 2023-04-17: ArkType not yet recommended over Zod. [everyones-talking-about-arktype @ 00:02:00] | 2025-06-04: ArkType a solid option. [open-source-libraries-you-have-to-know-arktype @ 01:28] |
| React.FC | 2023-03-16: returns only an element or null. [understanding-reacts-types @ 00:10] | 2023-12-22: fine now. [react-fc-used-to-be-crap-now-its-fine @ 00:00] |
| JSDoc | 2023-04-26: wants it in every project. [to-typescript-or-not-to-typescript-jsj-538 @ 05:00] | 2024-01-10: lighter syntax, same tooling needs. [jsdoc-vs-typescript-typescript § Detailed source notes] |
| Migration order | 2023-02-28: start high in the dependency tree. [ending-every-typescript-debate-in-90-minutes @ 01:30:00] | 2024-08-29: start with files with no dependencies that others depend on. [dejavue-e023-typescript-and-content-creation-with-matt-pocock @ 21:00] |

*Synthesis:* the types-versus-interfaces trail is the clearest self-correction: the 2022 note warns that it is advice he later changed, and the 2025 video says what he got wrong. [typescript-types-or-interfaces-deprecated-2022 § Detailed source notes] [types-vs-interfaces-what-i-got-wrong-in-2022 @ 00:00:44] The migration-order rows may be the same advice phrased differently; the sources do not say he changed his mind.

## Applying this

- *Inference:* for a new app, copy the cheat sheet's base options, then answer the transpile question: Node via tsc means `NodeNext` and `.js` import extensions; bundler or framework means `bundler`, `ESNext`, `noEmit`.
- *Inference:* for a new package, use his 2023 stack (pnpm, tsup, Vitest, Changesets, two workflows) when it must ship CJS and ESM, and the 2024 `tsc`-only monorepo layout inside a monorepo. The tsup recipe is from 2023, so check it against current tooling.
- *Inference:* parse environment variables once with a schema at startup and export the parsed object; reserve `process.env` augmentation for cases where compile-time hints suffice.
- *Inference:* in React, default to `ReactNode` for slots, plain-function props, and `ComponentProps`/`ElementRef` for third-party components; split polymorphic components before generalising them.
- *Inference:* for a JavaScript codebase, set strictness first and convert dependencies before dependents, with small pull requests.
- *Inference:* when a lint rule forces explicit return types or bans every `any`, scope it to library code, as he does.
- *Inference:* put mechanical rules (use pnpm, no unused variables) into hooks or lint, not prose instructions.

## Caveats

- Most sources are auto-caption-derived notes; the project's review wave corrected many mis-hearings, but some ambiguous passages were left as spoken.
- The shorter notes are summaries, so a missing topic is not evidence he never addressed it.
- Several recommendations are explicitly time-bounded: the Preconstruct example, the `defaultProps` preview, and the TypeScript feature previews were recorded before final releases, and the numbers in the Zod 4 and Go-port sources are as spoken, not independently measured. [preconstruct-is-amazing-for-bundling-typescript-libraries § Detailed source notes] [stop-using-defaultprops-react-18-3-preview § Detailed source notes] [i-met-anders-hejlsberg § Detailed source notes]
- Several articles are rights-limited research notes rather than full text, so their detail is thin: the tsconfig, process.env, npm package and `satisfies` articles in particular.
- Nothing in this file was checked against current TypeScript, Node or React documentation. Version-specific claims carry their video date for that reason.
