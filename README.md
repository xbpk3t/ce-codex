# ce-codex

Mirror repository for Codex-compatible artifacts generated from `EveryInc/compound-engineering-plugin`.

This repository is intentionally thin:

- `.github/workflows/update.yml` checks out upstream source, runs the local Bun CLI, and commits fresh Codex artifacts
- `skills/` and `prompts/` contain the generated artifacts committed by CI
- `manifest.json` records the last generation metadata

The intended consumer pattern is to treat this repository as a stable Codex skills source instead of running the upstream conversion step inside a local Nix activation.

## Triggering updates

- Scheduled: once per day
- Manual: GitHub Actions `workflow_dispatch`

## Generation flow

The workflow clones `EveryInc/compound-engineering-plugin`, installs its dependencies, and runs:

```bash
bun run src/index.ts install ./plugins/compound-engineering --to codex
```

Artifacts are generated into an isolated temporary `HOME`, then copied into this repository.

## Output layout

- `skills/<name>/SKILL.md`
- `prompts/*.md`
- `manifest.json`
