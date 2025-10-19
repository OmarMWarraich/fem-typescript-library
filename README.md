# fem-typescript-library

A small TypeScript utility library with a couple of tiny helpers useful for chat-style applications:

- `Deferred<T>`: a small Deferred/Promise wrapper exposing resolve/reject functions.
- `stringifyError`: a safe helper to convert thrown values (Errors, objects, primitives) to strings for logging.

This repo is configured with TypeScript, Jest tests, ESLint (type-aware rules) and API Extractor + API Documenter to produce docs.

## Quick links

- Source: `src/index.ts`
- Tests: `tests/index.test.ts`
- API report: `etc/fem-typescript-library.api.md`
- Generated docs: `docs/`

## Installation

Install dev dependencies and build as part of your workflow. This project uses Yarn (berry) per the repository config.

```bash
yarn install
```

## Usage

Import the package (when built/published) or import directly from source when using a monorepo/workspace.

TypeScript example

```ts
import { Deferred, stringifyError } from 'fem-typescript-library';

const d = new Deferred<string>();

// resolve later
setTimeout(() => d.resolve('done'), 100);

await d.promise; // 'done'

// stringify an error for logging
console.log(stringifyError(new Error('Boom'), 'Operation failed'));
```

JavaScript (compiled) example

```js
const { Deferred, stringifyError } = require('fem-typescript-library');

const d = new Deferred();
d.resolve('ok');
d.promise.then(console.log);

console.log(stringifyError('something bad', 'Oops'));
```

## API

- class Deferred<T>
	- constructor()
	- readonly promise: Promise<T>
	- readonly resolve(value: T | PromiseLike<T>): void
	- readonly reject(reason?: any): void

- function stringifyError(err: unknown, errorDescription: string): string
	- Safely converts an Error, primitive, or object to a readable string. Attempts `JSON.stringify` for objects with a fallback to `Object.prototype.toString`.

For a generated, machine-friendly API report see `etc/fem-typescript-library.api.md` and human-readable docs under `docs/` (produced by API Documenter).

## Scripts

- `yarn build` - compile TypeScript into `dist/` (produces `.js` and `.d.ts`)
- `yarn test` - build and run Jest tests
- `yarn lint` - run ESLint on `src` (see notes below about type-aware linting)
- `yarn api-report` - run API Extractor to produce the API report
- `yarn api-docs` - turn the API report into markdown docs under `docs/`

Example run:

```bash
yarn build
yarn test
yarn lint
```

## Development notes

- TypeScript is configured strictly (see `tsconfig.json`) and emits declarations (`declaration: true`).
- ESLint is configured for type-aware rules (the project uses `parserOptions.project = true` in the flat config). That enables helpful checks such as `@typescript-eslint/no-floating-promises` and `@typescript-eslint/restrict-plus-operands`. Turning `project` off will silence those rules but you lose many important checks; instead, prefer to fix the code or add narrow suppressions.
- If you see lint errors about stringifying unknown values (for example when concatenating arbitrary `unknown`), prefer explicit narrowing and safe conversion strategies (e.g. `String(value)` for primitives, `JSON.stringify` or `util.inspect` for objects, or guard by `instanceof Error`).

## Testing

Tests are written with Jest. Run them with:

```bash
yarn test
```

The test suite includes basic coverage for `Deferred` and `stringifyError` located in `tests/index.test.ts`.

## Linting

Run ESLint with:

```bash
yarn lint
```

If you upgrade/downgrade ESLint or `@typescript-eslint` packages, ensure compatible versions are installed. The repository initially used ESLint v8+ and the corresponding `@typescript-eslint` versions.

## Generating docs

To produce the API report and markdown docs:

```bash
yarn api-report
yarn api-docs
```

The `etc/` folder contains the API report and `docs/` contains the generated markdown files.

## Node / Tooling

- Node (Volta): `18.18.2` (see `package.json` `volta` block)
- Yarn: configured in the repo (berry)

## Contributing

Patches, issues, and improvements welcome. Run the test and lint targets locally before opening a PR.

## License

This repository does not include a license file. Add a LICENSE if you plan to publish.

