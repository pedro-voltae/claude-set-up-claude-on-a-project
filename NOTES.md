# NOTES

## What went into CLAUDE.md, and what was left out

I kept a one-line description, the four commands I actually run (`dev`, `test`, `lint`, single-test), the CI gate, and the non-obvious rules: CommonJS only, one route file per resource, all data access through `db/store.js`, and the validation/error convention. I left out the file tree, dependency list, and `.env.example` details because they are trivially discoverable from the code and just dilute the signal.

## Permission rules, and the risk without the deny rule

I allowed `Bash(npm test:*)` since it is safe and runs constantly, set `Bash(git push:*)` to ask so nothing reaches the remote without my sign-off, and denied `Read(./.env)`. Without that deny rule, a routine request like "check my config" could have Claude open `.env` and echo real secrets straight into the transcript.
