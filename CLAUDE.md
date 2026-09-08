# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

Starter Express REST API backed by an in-memory store — the base project for the Claude Code course.

## Commands

- `npm run dev` — start the API on http://localhost:3000 with auto-reload
- `npm test` — run the full test suite (Node's built-in runner)
- `npm run lint` — ESLint over the repo
- Run one test file: `node --test tests/users.test.js`

CI (`.github/workflows/ci.yml`) runs `npm run lint` then `npm test` on Node 22; both must pass.

## Conventions

- CommonJS only (`require` / `module.exports`) — `.eslintrc.json` sets `sourceType: "script"`, so no `import`/`export`.
- Add a resource by creating `routes/<name>.js` that exports a router, then mounting it in `server.js`. Keep one resource per file.
- Route handlers never touch data directly — all reads and writes go through `db/store.js`.
- Validate required fields in the handler and respond with `{ error: "message" }` and the matching status (`400` bad input, `404` not found), as the existing handlers do.
- Tests use `node:test` + `supertest` against the exported `app`. No Jest or Mocha.

## Architecture

- `server.js` — entry point. Builds the Express app, mounts the routers, and calls `app.listen` only when run directly (`require.main === module`) so tests can `require("../server")` without binding a port. Exports `app`.
- `routes/` — one router file per resource (`users.js`, `health.js`), each exporting an `express.Router`, mounted in `server.js` under its path prefix.
- `db/store.js` — the only data layer: an in-memory array exposed as `getAllUsers` / `getUserById` / `createUser`. Not persisted; resets on restart.
