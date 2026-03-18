# Provider Notes

## Supported tracing source
Stylus Trace expects `debug_traceTransaction` responses with Stylus tracer-compatible output.

## Recommended environments
- Local Nitro dev node for repeatable profiling.
- Orbit chains where debug tracing is enabled.

## Orbit-specific notes
- Keep custom chain ID documented in repo config and CI variables.
- Store RPC URL and tx hash in repository variables.
- Track one stable baseline transaction hash for regression checks.

## Required RPC capability check

Run this against your provider before using live capture:

```bash
curl -s -X POST "$RPC_URL" \
  -H "Content-Type: application/json" \
  -d '{
    "jsonrpc":"2.0",
    "id":1,
    "method":"debug_traceTransaction",
    "params":[
      "0xYOUR_TX_HASH",
      {"tracer":"stylusTracer"}
    ]
  }'
```

If this method is blocked or unavailable, use fixture-driven diff in CI and keep live capture optional.

## Reliability guidance
- For first-run CI stability, ship fixture profiles and generate artifacts from those.
- Add optional live capture only when environment variables are present.
