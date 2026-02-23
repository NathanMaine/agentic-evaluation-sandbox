# How to Add Dark Factory Scenarios to Your AES Repo

## Step 1: Copy the scenario files into your repo

```bash
# Navigate to your AES repo
cd path/to/agentic-evaluation-sandbox

# The scenarios/ directory already exists in your repo.
# Copy these three files into it:
#   - scenarios/dark-factory-fault-escalation.yaml
#   - scenarios/dark-factory-spec-satisfaction.yaml
#   - scenarios/dark-factory-digital-twin.yaml
#   - scenarios/README.md (add to the scenarios/ folder)
```

## Step 2: Test that they load

```bash
pip install -e .

# Verify the YAML loads without errors
aes run --scenario scenarios/dark-factory-fault-escalation.yaml --out out/dark-factory-test/

# You should see:
# Run ID: <uuid>
# Scenario: Dark Factory — Autonomous Fault Escalation
# Steps executed: 8
# Overall: SUCCESS
# Evidence: out/dark-factory-test/evidence.jsonl
```

## Step 3: Commit and push

```bash
git add scenarios/dark-factory-fault-escalation.yaml
git add scenarios/dark-factory-spec-satisfaction.yaml
git add scenarios/dark-factory-digital-twin.yaml
git add scenarios/README.md
git commit -m "feat: add dark factory holdout scenario suite

Adds three production-grade dark factory validation scenarios:
- Fault escalation: circuit breaker, retry, audit trail
- Spec satisfaction: anti-reward-hacking via holdout evaluation
- Digital twin: DTU fidelity validation, zero live API calls

Probabilistic satisfaction scoring (>=0.85 threshold).
Doer/Judge/Adversary/Observer pattern mapped to dark factory roles.
Agent never sees evaluation criteria during development.

Inspired by StrongDM Software Factory (factory.strongdm.ai)"

git push origin main
```

## Step 4: Run the live demo

For a live demo, run all three in sequence:

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

1. **The scenario YAML** — show that it's stored outside the codebase. The agent never sees this file. This is the holdout set.

2. **The run output** — show the step-by-step results: Doer → Judge → verdict, satisfaction score.

3. **The evidence JSONL** — show the append-only audit trail. Every decision logged. Tamper-evident.

4. **The architecture diagram** in scenarios/README.md — factory builds, AES validates, humans evaluate outcomes not code.

