# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.0.0] - 2026-03-06

### Added
- Fleet orchestrator command (`speckit.fleet.run`) with 10-phase lifecycle
  - Specify → Clarify → Plan → Checklist → Tasks → Analyze → Review → Implement → Verify → CI
- Cross-model review command (`speckit.fleet.review`) with 7-dimension evaluation
- Human-in-the-loop gates after every phase (approve / revise / skip / abort)
- Artifact detection and mid-workflow resume on fresh conversations
- Stale artifact detection with timestamp chain comparison
- Parallel subagent execution (up to 3 concurrent) during Plan and Implement phases
- `[P]` marker and `<!-- parallel-group: N -->` task organization
- Verify extension integration with auto-install prompt
- Error recovery for parallel task failures and sub-agent timeouts
- Configurable models, parallelism, and verify settings via `fleet-config.yml`
