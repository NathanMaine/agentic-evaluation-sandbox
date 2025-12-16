# Agentic Evaluation Sandbox

A research sandbox for evaluating AI agents in dynamic, multi-agent environments rather than static prompt tests.

The sandbox supports:
- Scenario-driven simulations
- Multiple agent roles (Doer, Judge, Adversary, Observer)
- Structured scoring and evidence logs
- Replayable evaluation runs

This is a research prototype, not a production system.

## Quickstart

```bash
python -m venv .venv
source .venv/bin/activate
python -m aes.cli run --scenario scenarios/example.yaml --out out
```

Outputs: `out/runs/<run_id>.json` and `out/evidence.jsonl` with stubbed Doer/Judge traces.
