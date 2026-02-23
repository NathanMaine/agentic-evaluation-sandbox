# Dark Factory Scenarios — Setup & Demo

## Setup

```bash
git clone https://github.com/NathanMaine/agentic-evaluation-sandbox.git
cd agentic-evaluation-sandbox
pip install -e .
```

## Run a single scenario

```bash
aes run --scenario scenarios/dark-factory-fault-escalation.yaml --out out/dark-factory-test/

# Expected output:
# Run ID: <uuid>
# Scenario: Dark Factory — Autonomous Fault Escalation
# Steps executed: 8
# Overall: SUCCESS
# Evidence: out/dark-factory-test/evidence.jsonl
```

## Run the full demo

Run all three dark factory scenarios in sequence:

```bash
#!/bin/bash
echo "=== DARK FACTORY SCENARIO SUITE ==="
echo ""

for scenario in \
  "scenarios/dark-factory-fault-escalation.yaml" \
  "scenarios/dark-factory-spec-satisfaction.yaml" \
  "scenarios/dark-factory-digital-twin.yaml"
do
  name=$(grep "^title:" "$scenario" | sed 's/title: "//' | sed 's/"//')
  echo "Running: $name"
  echo "---"
  aes run --scenario "$scenario" --out "out/demo/$(basename $scenario .yaml)/"
  echo ""
done

echo "=== EVIDENCE AUDIT TRAIL ==="
cat out/demo/*/evidence.jsonl | python3 -m json.tool | head -60
```

## What to show in the demo

1. **The scenario YAML** — stored outside the codebase. The agent never sees this file. This is the holdout set.

2. **The run output** — step-by-step results: Doer → Judge → verdict, satisfaction score.

3. **The evidence JSONL** — append-only audit trail. Every decision logged. Tamper-evident.

4. **The architecture diagram** in scenarios/README.md — factory builds, AES validates, humans evaluate outcomes not code.
