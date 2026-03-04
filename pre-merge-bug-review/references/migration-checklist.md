# Migration Checklist

## 1) Safety of Schema Changes

- Check for destructive operations (`DROP`, lossy type changes, unsafe renames).
- Verify nullable-to-not-null transitions have backfill + default strategy.
- Confirm new constraints can succeed on existing production-like data.

## 2) Deploy Compatibility

- Ensure old app version can run with new schema during rolling deploy.
- Ensure new app version can tolerate mixed schema states when applicable.
- Validate ordering: additive schema first, app usage second, cleanup last.

## 3) Data Backfill Strategy

- Verify backfill is idempotent and chunked for large tables.
- Confirm timeout/retry behavior and operational observability.
- Ensure partial backfills do not break reads/writes.

## 4) Performance and Locking

- Check index creation strategy (online/concurrent where supported).
- Evaluate lock duration risk for altered hot tables.
- Flag full-table rewrites and expensive default computations.

## 5) Rollback and Recovery

- Confirm down migration feasibility, or state non-reversible decision clearly.
- Define recovery plan for failed migration mid-deploy.
- Confirm backup/restore assumptions if destructive operations exist.

## 6) Verification Evidence

For every migration issue, attach at least one:
- SQL explain/plan or lock-risk reasoning
- Migration dry-run/test output
- Deterministic code+schema incompatibility proof
- Failing integration test tied to migration
