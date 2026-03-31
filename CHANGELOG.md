# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.1.0] - 2026-03-31

### Fixed
- Context window exhaustion: orchestrator now compacts context at every phase gate, carrying forward only a structured summary instead of full sub-agent output (#1)
- Workflow stalling between phases: added mandatory gate menu after every sub-agent return -- orchestrator can no longer end a turn silently (#1)
- Repeated review model question on resume: orchestrator now reads config file first and skips the prompt if a concrete model is already set; offers to persist the choice immediately (#1)
- Simultaneous questions (e.g., WIP commit + phase gate): added "one question per turn" rule -- prompts are now strictly sequential (#1)

### Added
- Operating Rule 11: one question per turn
- Operating Rule 12: every orchestrator turn must end with a user prompt
- Context compaction instructions in Phase Execution Template
- Earlier context-pressure warnings starting at Phase 5 (previously Phase 8)

## [1.0.1] - 2026-03-31

### Fixed
- Removed invalid 2-segment command aliases (`speckit.fleet`, `speckit.review`) that caused `Validation Error: must follow pattern 'speckit.{extension}.{command}'` on install (#2)

## [1.0.0] - 2026-03-06

### Added
- Fleet orchestrator command (`speckit.fleet.run`) -- 10-phase lifecycle from spec to CI with human-in-the-loop gates, artifact-based resume, and parallel subagent execution
- Cross-model review command (`speckit.fleet.review`) -- 7-dimension pre-implementation evaluation using a different model for blind-spot detection
- Configurable models, parallelism, and verify settings via `fleet-config.yml`
