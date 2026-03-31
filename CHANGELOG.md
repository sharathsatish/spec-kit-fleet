# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.0.1] - 2026-03-31

### Fixed
- Removed invalid 2-segment command aliases (`speckit.fleet`, `speckit.review`) that caused `Validation Error: must follow pattern 'speckit.{extension}.{command}'` on install (#2)

## [1.0.0] - 2026-03-06

### Added
- Fleet orchestrator command (`speckit.fleet.run`) -- 10-phase lifecycle from spec to CI with human-in-the-loop gates, artifact-based resume, and parallel subagent execution
- Cross-model review command (`speckit.fleet.review`) -- 7-dimension pre-implementation evaluation using a different model for blind-spot detection
- Configurable models, parallelism, and verify settings via `fleet-config.yml`
