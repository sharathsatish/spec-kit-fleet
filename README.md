# Fleet Orchestrator — Spec-Kit Extension

Orchestrate a full feature lifecycle with human-in-the-loop gates across all SpecKit phases.

```
specify → clarify → plan → checklist → tasks → analyze → review → implement → verify → CI
```

The fleet orchestrator chains 10 phases into a single command, detecting partially complete work and resuming from the right phase. It dispatches up to 3 parallel subagents during Plan and Implement, and uses a **cross-model review** before implementation to catch blind spots.

## Features

- **10-phase workflow** — end-to-end from idea to CI-passing code
- **Human gates** — approve, revise, skip, or abort after every phase
- **Mid-workflow resume** — detects existing artifacts and picks up where you left off
- **Stale artifact detection** — warns when upstream changes invalidate downstream files
- **Parallel execution** — up to 3 concurrent subagents for `[P]`-marked tasks
- **Cross-model review** — Phase 7 uses a different model to evaluate plan + tasks
- **Verify integration** — auto-prompts to install the [verify extension](https://github.com/ismaelJimenez/spec-kit-verify) if missing

## Prerequisites

- [Spec-Kit](https://github.com/github/spec-kit) >= 0.1.0
- These core SpecKit commands must be available:
  - `speckit.specify`, `speckit.clarify`, `speckit.plan`, `speckit.checklist`
  - `speckit.tasks`, `speckit.analyze`, `speckit.implement`
- (Optional) [Verify extension](https://github.com/ismaelJimenez/spec-kit-verify) for Phase 9

## Installation

### From GitHub Release

```bash
specify extension add fleet --from https://github.com/commerce-ecosystems/spec-kit-fleet/archive/refs/tags/v1.0.0.zip
```

### Local Development

```bash
specify extension add --dev /path/to/spec-kit-fleet
```

### Verify Installation

```bash
specify extension list
# Should show: fleet (1.0.0) — Fleet Orchestrator
```

After installation, the following commands are registered:

| Command | Alias | Description |
|---------|-------|-------------|
| `speckit.fleet.run` | `speckit.fleet` | Full lifecycle orchestrator |
| `speckit.fleet.review` | `speckit.review` | Cross-model pre-implementation review |

## Usage

### Start the Fleet

In VS Code Copilot Chat:

```
/speckit.fleet Build a capability browser that lets users search and filter available capabilities
```

Or with no arguments to auto-detect progress on the current feature branch:

```
/speckit.fleet
```

### Resume Mid-Workflow

The fleet automatically detects existing artifacts on every invocation. If you're on a feature branch with `spec.md` and `plan.md` already created, it will show:

```
Phase 1 Specify      ✅ spec.md found
Phase 2 Clarify      ✅ ## Clarifications present
Phase 3 Plan         ✅ plan.md found
Phase 4 Checklist    ⬜ —
...
▶ Resuming at Phase 4: Checklist
```

You can confirm or override to any phase.

### Run Review Standalone

```
/speckit.review
```

This runs the cross-model evaluation independently (read-only, no file modifications).

## Configuration

After installation, optionally customize settings by editing `.specify/extensions/fleet/fleet-config.yml`:

```yaml
# Parallel execution (1-3 concurrent subagents)
parallel:
  max_concurrency: 3

# Model preferences
models:
  primary: "Claude Opus 4.6 (copilot)"
  review:
    - "Claude Sonnet 4.6 (copilot)"
    - "GPT-5.3 Codex (copilot)"
    - "GPT 5.4 (copilot)"

# Verify extension auto-install prompt
verify:
  auto_prompt_install: true
  install_url: "https://github.com/ismaelJimenez/spec-kit-verify/archive/refs/tags/v1.0.0.zip"
```

| Setting | Default | Description |
|---------|---------|-------------|
| `parallel.max_concurrency` | `3` | Max subagents dispatched simultaneously |
| `models.primary` | Claude Opus 4.6 | Model for the fleet orchestrator |
| `models.review` | Sonnet 4.6 → GPT-5.3 → GPT 5.4 | Review model fallback chain |
| `verify.auto_prompt_install` | `true` | Prompt to install verify extension if missing |
| `verify.install_url` | GitHub archive URL | Verify extension download URL |

## Workflow Phases

| # | Phase | Agent | What It Does |
|---|-------|-------|--------------|
| 1 | Specify | `speckit.specify` | Generate feature specification |
| 2 | Clarify | `speckit.clarify` | Ask targeted clarification questions (repeatable) |
| 3 | Plan | `speckit.plan` | Create technical implementation plan |
| 4 | Checklist | `speckit.checklist` | Generate quality checklists |
| 5 | Tasks | `speckit.tasks` | Break plan into dependency-ordered tasks |
| 6 | Analyze | `speckit.analyze` | Cross-artifact consistency analysis |
| 7 | Review | `speckit.fleet.review` | Cross-model evaluation (different model) |
| 8 | Implement | `speckit.implement` | Execute tasks (parallel groups) |
| 9 | Verify | `speckit.verify` | Validate code against spec artifacts |
| 10 | CI | Terminal | Run `./run-ci-local.ps1` |

### Human Gates

After every phase, you're asked to:
- **Approve** — proceed to the next phase
- **Revise** — re-run with feedback
- **Skip** — skip this phase
- **Abort** — stop the workflow

### Parallel Execution

During Plan (Phase 3) and Implement (Phase 8), tasks marked with `[P]` are grouped into parallel batches:

```markdown
<!-- parallel-group: 1 (max 3 concurrent) -->
- [ ] T002 [P] Create ModelA.cs
- [ ] T003 [P] Create ModelB.cs
- [ ] T004 [P] Create ModelC.cs

<!-- sequential -->
- [ ] T005 Create service that depends on all models
```

Constraints:
- Max 3 concurrent subagents per group
- Tasks touching the same file always run sequentially
- Human gate applies after each implementation phase completes

## Troubleshooting

### Fleet doesn't detect my feature directory

The fleet runs `check-prerequisites.ps1 -Json -PathsOnly` to discover `FEATURE_DIR`. This requires a feature branch with a matching specs directory. Ensure your branch follows the naming convention and a specs folder exists.

### Review phase uses the wrong model

Model selection is in the `commands/review.md` frontmatter. After installation, the registered agent file is in `.github/agents/speckit.fleet.review.md`. The `model` field accepts an array — the first available model is used.

### Verify extension not found

If Phase 9 reports the verify extension isn't installed, run:

```bash
specify extension add verify --from https://github.com/ismaelJimenez/spec-kit-verify/archive/refs/tags/v1.0.0.zip
```

Or set `verify.auto_prompt_install: false` in config to always skip verification.

### Stale artifact warning

This means an upstream file (e.g., `spec.md`) was modified after a downstream file (e.g., `plan.md`) was generated. Re-run the affected phase or proceed if the change was cosmetic.

## Contributing

1. Fork this repository
2. Create a feature branch
3. Make changes and test with `specify extension add --dev /path/to/your-fork`
4. Submit a pull request

## License

[MIT](LICENSE)
