# cognipipe

## 0.1.0

### Minor Changes

- d28df2f: Add cognipipe test command with DAG cycle detection and execution preview
- 5b99143: Bootstraps the CLI package from scratch (package.json, tsconfig, jest config, entry point) and implements the --version / -v flag, reading version from package.json at runtime via createRequire. Uses ESM-mode Jest (via cross-env + --experimental-vm-modules) since import.meta.url requires real ESM, unlike the CommonJS ts-jest pattern used in packages/core.

### Patch Changes

- d28df2f: Fix ERR_MODULE_NOT_FOUND when consuming compiled ESM output under Node's native module loader. Relative imports across all packages were missing explicit .js extensions — TypeScript's `bundler` moduleResolution compiled these without error, but Node's native ESM resolver requires an exact extension on every relative specifier and has no fallback. Jest's more lenient resolver masked this in every test run, so it was never caught until a real `node dist/index.js` invocation surfaced it. Fixed by adding .js extensions to all relative imports and adding a `moduleNameMapper` to each ESM-mode/CJS-transform Jest config so `ts-jest` can still resolve the real .ts source files despite the literal .js specifiers.
