# Macro — TODO

> **Legend** — priority `P0` critical · `P1` high · `P2` normal · `P3` low
> categories `security` `bug` `feature` `performance` `design` `docs` `testing` `infra` `research`
> owner `@me` (needs you — accounts, keys, money, judgement) · `@ai` (Claude can do this)

---

## v1 — current

- [ ] `P2` `testing` `@ai` Pin the regime score against known historical conditions. The weights are published and the components named, which makes the heuristic auditable — but nothing asserts that a 2008 or 2020 input produces the regime a reader would expect, so a weight change can drift the output with no test objecting.
- [ ] `P2` `bug` `@ai` Say when the cache is serving stale values. The six-hour TTL keeps the last good numbers on disk when FRED is unreachable, which is the right call — but a regime built from three-day-old data currently looks identical to a fresh one.
- [ ] `P3` `design` `@ai` Split fetching, caching and scoring out of the single `__init__.py`. Fine at its current size; worth doing before a seventh series or a second source arrives.
