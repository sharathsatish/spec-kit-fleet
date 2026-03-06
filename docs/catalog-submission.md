# Catalog & PR Templates

These are pre-built snippets for submitting the fleet extension to the
spec-kit community catalog. Copy-paste when ready.

---

## catalog.community.json Entry

Add this under `"extensions"` in `extensions/catalog.community.json` in the
[spec-kit repo](https://github.com/github/spec-kit):

```json
"fleet": {
  "name": "Fleet Orchestrator",
  "id": "fleet",
  "description": "Orchestrate a full feature lifecycle with human-in-the-loop gates across all SpecKit phases.",
  "author": "sharath",
  "version": "1.0.0",
  "download_url": "https://github.com/commerce-ecosystems/spec-kit-fleet/archive/refs/tags/v1.0.0.zip",
  "repository": "https://github.com/commerce-ecosystems/spec-kit-fleet",
  "homepage": "https://github.com/commerce-ecosystems/spec-kit-fleet",
  "documentation": "https://github.com/commerce-ecosystems/spec-kit-fleet/blob/main/README.md",
  "changelog": "https://github.com/commerce-ecosystems/spec-kit-fleet/blob/main/CHANGELOG.md",
  "license": "MIT",
  "requires": {
    "speckit_version": ">=0.1.0",
    "tools": []
  },
  "provides": {
    "commands": 2,
    "hooks": 1
  },
  "tags": [
    "orchestration",
    "workflow",
    "human-in-the-loop",
    "parallel"
  ],
  "verified": false,
  "downloads": 0,
  "stars": 0,
  "created_at": "2026-03-06T00:00:00Z",
  "updated_at": "2026-03-06T00:00:00Z"
}
```

---

## extensions/README.md Table Row

Insert alphabetically in the Available Extensions table:

```markdown
| Fleet Orchestrator | Orchestrate a full feature lifecycle with human-in-the-loop gates | [spec-kit-fleet](https://github.com/commerce-ecosystems/spec-kit-fleet) |
```

---

## Pull Request Description

```markdown
## Extension Submission

**Extension Name**: Fleet Orchestrator
**Extension ID**: fleet
**Version**: 1.0.0
**Author**: sharath
**Repository**: https://github.com/commerce-ecosystems/spec-kit-fleet

### Description
Orchestrate a full feature lifecycle with human-in-the-loop gates across all
SpecKit phases. Chains 10 phases (specify -> clarify -> plan -> checklist ->
tasks -> analyze -> review -> implement -> verify -> CI) into a single command
with artifact detection, mid-workflow resume, parallel execution (up to 3
concurrent subagents), and cross-model review.

### Checklist
- [x] Valid extension.yml manifest
- [x] README.md with installation and usage docs
- [x] LICENSE file included
- [ ] GitHub release created (v1.0.0)
- [x] Extension tested on real project
- [x] All commands working
- [x] No security vulnerabilities
- [x] Added to extensions/catalog.community.json
- [x] Added to extensions/README.md Available Extensions table

### Testing
Tested on:
- Windows 11 with spec-kit 0.1.0 (specify-cli)
- Project: docs-iq (DocsIQ content management system)

### Additional Notes
- Provides 2 commands: `speckit.fleet.run` (alias `speckit.fleet`) and
  `speckit.fleet.review` (alias `speckit.review`)
- Includes `after_tasks` hook for automatic cross-model review
- Optional dependency on the verify extension (auto-prompts to install)
- All files are pure ASCII for Windows cp1252 compatibility
```
