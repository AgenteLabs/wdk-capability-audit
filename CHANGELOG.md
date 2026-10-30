# Changelog

All notable changes to this project are documented here. Format loosely follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

## 0.2.0 — 2026-10-30

**Changed:** the npm scope moved — this package now publishes as
[`@flashyfinancial/wdk-capability-audit`](https://www.npmjs.com/package/@flashyfinancial/wdk-capability-audit),
not `@flashylabs/wdk-capability-audit`. Every import in this README, the shipped
schema, the generated manifest, and the publish workflow moved with it. **This
is a breaking change for anyone with the old name pinned** — run
`npm install @flashyfinancial/wdk-capability-audit` and update the import path;
nothing about the package's behaviour, exports, or report contract changed.
The old name is not removed from the registry: a maintainer can run the new
`Deprecate old npm scope` workflow (`workflow_dispatch` only, never on a push
or a schedule) to mark it with `npm deprecate`, pointing anyone who installs
it at the new scope.

## 0.1.0 — 2026-09-22

Initial public release.

- `discoverWdkPackages()`, `inspectModule()`, `classifyChain()`, `buildReport()`, `validateReport()`, `auditDirectory()` — the core scan/inspect/classify/report pipeline, each a pure, independently testable function.
- `bin/wdk-capability-audit.js` — the CLI (`wdk-capability-audit [--dir <path>] [--json]`), with a vacuity guard: zero installed `@tetherto/wdk-*` packages is a refusal (non-zero exit), never a silent empty report.
- `schema/report.schema.json` — a versioned JSON Schema (draft 2020-12) for the `wdk-capability-audit-report/1` report shape, plus a hand-rolled structural validator (`src/schema-validate.js`) that checks a report against it with no added runtime dependency.
- A hand-maintained chain registry (`src/registry.js`) covering Ethereum mainnet/Sepolia/Holesky, four EVM L2 mainnet/testnet pairs, Tron mainnet/Nile/Shasta, and Bitcoin mainnet/testnet3/testnet4/signet — nothing else, on purpose. A chain not listed classifies `unknown`, never a guess.
- `src/faucets.js`, `getFaucet()`/`listFaucets()`/`listNoFaucetByDesign()`, and `wdk-capability-audit --faucets` — a hand-maintained, source-checked faucet per recognized testnet chain. `bitcoin:testnet3` is deliberately listed with no faucet and a named reason (Bitcoin Core 30.0 removed it) rather than silently omitted.
- The chain-inspection convention (`wdk.chains` in a package's own `package.json`) is stated as an explicit, unverified assumption — see README "Status." No `@tetherto/wdk-*` package installed while this package was built declares it; `inspectModule()` returns `unrecognized-shape` for all six that were checked, and that refusal is itself tested.
- `wdk-capability-audit.manifest.json` — a generated, machine-readable statement of the report contract, the closed vocabularies, and the registered chains this version ships.
