# Quickstart

Target KPI: first profile in about 10 minutes.

## Prerequisites
- Rust (stable)
- Docker (optional for local Nitro dev node)
- `cargo install stylus-trace-studio` or local binary from this repository

## Full 10-minute command flow

Run from repository root:

```bash
# 1) Build local CLI binary (fastest if you are developing in this repo)
REPO_ROOT="$(pwd)"
cargo build --bin stylus-trace

# 2) Copy starter template to a clean temp folder
rm -rf /tmp/stylus-trace-starter
cp -R templates/starter-repo /tmp/stylus-trace-starter
cd /tmp/stylus-trace-starter

# 3) Generate local diff artifacts from bundled baseline/current profiles
"$REPO_ROOT"/target/debug/stylus-trace diff \
  profiles/baseline.json \
  profiles/current_profile.json \
  --output artifacts/diff/diff_report.json \
  --flamegraph artifacts/diff/diff.svg \
  --summary

# 4) Verify artifact files exist
ls -la artifacts/diff
```

Expected files:
- `artifacts/diff/diff.svg`
- `artifacts/diff/diff_report.json`

## Live transaction profiling (optional)

Start Nitro dev node (optional local tracing target):

```bash
git clone https://github.com/OffchainLabs/nitro-devnode.git
cd nitro-devnode
./run-dev-node.sh
```

In another shell:

```bash
stylus-trace capture \
  --rpc http://localhost:8547 \
  --tx 0xYOUR_TX_HASH \
  --output artifacts/capture/current_profile.json \
  --flamegraph artifacts/capture/capture.svg \
  --summary
```

Then compare to baseline:
```bash
stylus-trace diff \
  artifacts/capture/baseline.json \
  artifacts/capture/current_profile.json \
  --output artifacts/diff/diff_report.json \
  --flamegraph artifacts/diff/diff.svg \
  --summary
```

## Full template test commands

For complete starter, education, and orbit validation commands, use:
- `docs/template-validation.md`
