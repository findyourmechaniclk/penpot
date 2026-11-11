# Clojure Logging Conventions

Structured logging through `app.common.logging` (`l/trc`/`l/dbg`/`l/info`/`l/wrn`/`l/error`/`l/fatal`): `:hint` carries the human-readable message, every other datum goes in its own keyword field (`:id`, `:container`, `:removed`, ...) — never interpolate payloads into the `:hint` string.

- `:hint` values start lowercase: `"fix dangling children"`, not `"Fix dangling children"`. Uppercase-first hints exist as legacy (~57 occurrences, the densest in `backend/src`; also `l/trc` in migration `legacy-28` and `log/error` in `app.common.files.repair`) — do not copy them, and do not mass-rewrite them unless the surrounding code needs the change.
- A name that requires uppercase (an env var like `PENPOT_MEDIA_PROCESSING_SERVICE_URI`, an acronym like `MCP` or `LDAP`) stays uppercase inside an otherwise lowercase sentence.
- Repair work done by a file migration logs at `l/trc` (never `warn` — migrations run unattended on file open, and a trace line is the operator's only record), only when it changes something, and with the ids it touched.
