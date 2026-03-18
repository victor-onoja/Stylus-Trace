# CI Cookbook

## Goal
Keep CI green by default, always upload profiling artifacts, and gate regressions when thresholds are exceeded.

## Use full workflow files

Use these full files directly:
- `templates/starter-repo/.github/workflows/performance.yml`
- `templates/orbit/.github/workflows/orbit-performance.yml`

Copy commands from repo root:

```bash
# Starter
mkdir -p /path/to/target-repo/.github/workflows
cp templates/starter-repo/.github/workflows/performance.yml \
  /path/to/target-repo/.github/workflows/performance.yml

# Orbit
cp templates/orbit/.github/workflows/orbit-performance.yml \
  /path/to/target-repo/.github/workflows/orbit-performance.yml
```

## Starter workflow behavior
1. Stages `profiles/baseline.json` and `profiles/current_profile.json`.
2. Optionally captures live profile when vars exist:
   - `STYLUS_TRACE_RPC_URL`
   - `STYLUS_TRACE_TX_HASH`
3. Runs `stylus-trace diff`.
4. Uploads `artifacts/` as `stylus-trace-artifacts`.

## Orbit workflow behavior
1. Requires:
   - Variable `ORBIT_RPC_URL`
   - Variable `ORBIT_TX_HASH`
   - Secret `ORBIT_CHAIN_ID`
2. Captures profile from Orbit RPC transaction hash.
3. Diffs against committed baseline.
4. Uploads `artifacts/` as `orbit-stylus-trace-artifacts`.

## Baseline gate examples
- Global gate:
  ```bash
  stylus-trace diff baseline.json current_profile.json --threshold-percent 3.0
  ```
- Gas-only gate:
  ```bash
  stylus-trace diff baseline.json current_profile.json --gas-threshold 1.5
  ```
- HostIO-only gate:
  ```bash
  stylus-trace diff baseline.json current_profile.json --hostio-threshold 5.0
  ```

## Cache guidance
- Cache Cargo index and target with `Swatinem/rust-cache@v2`.
- Keep baseline profiles committed to repo for deterministic first runs.

## Artifact set
- `artifacts/capture/current_profile.json`
- `artifacts/diff/diff_report.json`
- `artifacts/diff/diff.svg`

## End-to-end validation commands
- Full command file:
  - `docs/template-validation.md`
