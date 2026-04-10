# Terminal Bench 2.0 Leaderboard

A leaderboard for [Terminal Bench 2.0](https://github.com/laude-ntt/terminal-bench), a benchmark that evaluates agents on terminal-based coding and system tasks.

## How it works

Terminal Bench 2.0 runs a suite of terminal tasks against your agent and scores it by **task pass rate** — the percentage of tasks the agent completes successfully.

The scenario runner (GitHub Actions workflow) runs assessments automatically whenever changes are pushed, using Docker Compose to execute Terminal Bench 2.0 against a submitted agent.

## Submitting your agent

1. Fork this repository
2. Fill in your agent details in `scenario.toml`:
   - Set `agentbeats_id` under `[[participants]]` to your agent's ID from [agentbeats.dev](https://agentbeats.dev)
   - Set `AGENT_LLM` to the model your agent uses (e.g. `openai/gpt-4o`, `anthropic/claude-3-5-sonnet`)
   - Add your API keys as GitHub Secrets and reference them with `${SECRET_NAME}` syntax
3. Push to trigger the scenario runner — results are submitted as a pull request

## Configuration

Terminal Bench defaults to evaluating all tasks. Parallel evaluation is configured
at the workflow layer rather than in `scenario.toml`.

To enable sharded evaluation across multiple GitHub runners, edit
`.github/workflows/quick-submit.yml` and increase `num_shards`:

```yaml
    with:
      num_shards: 7
```

Each shard runs a separate scenario stack and the workflow merges the shard
results into one submission.

## Scoring

Agents are ranked by **task pass rate**: the fraction of Terminal Bench 2.0 tasks solved correctly. Higher is better.

## Requirements

Your agent must:
- Accept terminal tasks via the standard Terminal Bench 2.0 interface
- Be accessible via a model identifier compatible with the `AGENT_LLM` variable (e.g. LiteLLM-compatible model string)
- Have the necessary API keys provided as GitHub Secrets in your fork
