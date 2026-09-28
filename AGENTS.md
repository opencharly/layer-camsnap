# AGENTS.md — layer-camsnap

Standalone candy repo for the `camsnap` layer — an RTSP/ONVIF camera snapshot
and clip CLI. The entire candy lives in `charly.yml` at the repo root: the
`camsnap:` candy entity (a `go install` build step plus its ordered `check:`
assertions) and the embedded `camsnap-skill:` entity that is projected into the
marketplace corpus as `/charly-selkies:camsnap`. There is no source tree and no
runtime service — only a Go install and its verifiable assertions.

Canonical files:

- `charly.yml` — the `camsnap:` candy entity and the `camsnap-skill:` skill entity.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — user overview only; never agent guidance.

## Load these skills first (R0)

- `/charly-selkies:camsnap` — the owning skill. What the candy installs, how it
  is consumed, and its environment (`GOPATH`, `PATH`). Load before editing or
  troubleshooting the candy.
- `/charly-image:layer` — the candy authoring reference (`charly.yml` schema,
  `plan:` step verbs incl. `check:`, package sections, service declarations).
  Load before editing any entity field or plan step.

## Build / validate / test

- `charly box validate` at the repo root — the structural check: the manifest
  must parse and validate at the installed charly.
- The merge gate is the **org-wide** `charly/pr-validator` (required check
  `validate / validate`, defined in `opencharly/.github`); this repo has no
  per-repo candy gate.
- There is no live bed: the candy is a Go install, so the evidence is its
  `plan:` `check:` steps, which assert the binary exists in the user's GOPATH
  bin, is executable, and `go version -m` reports it was built from
  `github.com/steipete/camsnap`.

## Modify this repo

- Edit the `camsnap:` candy entity AND the `camsnap-skill:` skill entity in
  `charly.yml` together. The skill is the projected usage source, so a build or
  behaviour change that is not mirrored in the skill leaves the corpus stale.
- The install target is `@latest`; pinning or changing the module path is a
  behaviour change that must be mirrored in the skill and the `check:` steps.
- Behaviour claims go in `plan:` as observable `check:` steps.

## Landing

- PR-only. Every change lands through a pull request; the org-required
  `charly/pr-validator` validates the diff and body and arms native auto-merge
  on PASS. Direct pushes to `main` are blocked.
- History lives in `CHANGELOG/` (written by `tag-on-merge` at merge time); the
  PR body IS the changelog.
- The authoritative rulebook is the umbrella `AGENTS.md` in
  `opencharly/opencharly` and `charly/AGENTS.md` in the charly repo. Do not
  restate its rules here.
