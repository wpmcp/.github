# What this changes

<!-- One or two sentences. What behaviour is different after this merges. -->

Closes #

## Why

<!-- The problem this solves. Link the issue or the discussion if there is one. -->

## How to verify

<!-- The exact commands or steps a reviewer runs to see it work. -->

```bash
composer test
composer lint
```

## Checklist

- [ ] Tests cover the change, and were written before the implementation where practical
- [ ] Every new write path goes through the safety layer, so the operation is snapshotted and restorable
- [ ] New or renamed abilities are reflected in the ability manifest (`composer manifest:regenerate`) and the full suite was run, not just a filtered run
- [ ] Capability checks and input validation are in place on anything reachable from an agent
- [ ] Coverage is at or above the CI floor
- [ ] User-facing strings and docs contain no em-dashes and no en-dashes
- [ ] Release notes state honestly what is verified by tests and what is only verified in production

## Safety impact

<!-- Delete the lines that do not apply. -->

- [ ] No change to the safety, governance, identity, or auth layers
- [ ] Changes governance, scoped identities, or auth
- [ ] Touches snapshot, rollback, or restore
- [ ] Adds or changes an execution path (WP-CLI, PHP snippets, file writes), which needs an adversarial security review before merge

## Scope notes

<!-- Anything this deliberately does not cover. Deferred is fine, undocumented is not. -->
