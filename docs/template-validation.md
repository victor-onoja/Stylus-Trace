# Template Validation (Full Commands)

This file contains full copy-paste command sequences to test all template examples.

## 0) Build local binary

Run from repository root:

```bash
REPO_ROOT="$(pwd)"
cargo build --bin stylus-trace
```

## 1) Starter template local test

```bash
rm -rf /tmp/m4-check-starter
cp -R "$REPO_ROOT"/templates/starter-repo /tmp/m4-check-starter
cd /tmp/m4-check-starter

"$REPO_ROOT"/target/debug/stylus-trace diff \
  profiles/baseline.json \
  profiles/current_profile.json \
  --output artifacts/diff/diff_report.json \
  --flamegraph artifacts/diff/diff.svg \
  --summary

ls -la artifacts/diff
```

Expected:
- `artifacts/diff/diff_report.json`
- `artifacts/diff/diff.svg`

## 2) Education template local test

```bash
rm -rf /tmp/m4-check-education
cp -R "$REPO_ROOT"/templates/education /tmp/m4-check-education
cd /tmp/m4-check-education

"$REPO_ROOT"/target/debug/stylus-trace diff \
  profiles/baseline.json \
  profiles/current_profile.json \
  --output artifacts/diff/diff_report.json \
  --flamegraph artifacts/diff/diff.svg \
  --summary

ls -la artifacts/diff
```

Expected:
- `artifacts/diff/diff_report.json`
- `artifacts/diff/diff.svg`

## 3) Orbit template local smoke test (no RPC required)

This verifies template structure and diff artifact generation.

```bash
rm -rf /tmp/m4-check-orbit
cp -R "$REPO_ROOT"/templates/orbit /tmp/m4-check-orbit
cd /tmp/m4-check-orbit

cp artifacts/capture/baseline.json artifacts/capture/current_profile.json

"$REPO_ROOT"/target/debug/stylus-trace diff \
  artifacts/capture/baseline.json \
  artifacts/capture/current_profile.json \
  --output artifacts/diff/diff_report.json \
  --flamegraph artifacts/diff/diff.svg \
  --summary

ls -la artifacts/diff
```

Expected:
- `artifacts/diff/diff_report.json`
- `artifacts/diff/diff.svg`

## 4) Orbit GitHub workflow real run (RPC + chain ID)

Workflow file:
- `templates/orbit/.github/workflows/orbit-performance.yml`

### Repo config
- Variable: `ORBIT_RPC_URL`
- Variable: `ORBIT_TX_HASH`
- Secret: `ORBIT_CHAIN_ID`

### Steps
1. Copy the workflow file into your target repository at `.github/workflows/orbit-performance.yml`.
2. Commit/push.
3. Trigger `workflow_dispatch` in GitHub Actions.
4. Confirm job `orbit-profile` is green.
5. Confirm artifact `orbit-stylus-trace-artifacts` is uploaded.

## 5) Starter workflow real run

Workflow file:
- `templates/starter-repo/.github/workflows/performance.yml`

### Steps
1. Copy into target repository `.github/workflows/performance.yml`.
2. Commit/push (or open PR).
3. Confirm job `profile` is green on first run.
4. Confirm artifact `stylus-trace-artifacts` is uploaded.
5. Optionally set vars for live capture:
   - `STYLUS_TRACE_RPC_URL`
   - `STYLUS_TRACE_TX_HASH`
