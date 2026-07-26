# Codex instructions for the Retro controls branch

## Scope

This checkout is a migration copy of MAME used to preserve three local control
changes. The operational checkout at `/Users/andrea/dev/mame` is read-only
reference material.

## Repository structure

- `upstream` is the official `mamedev/mame` repository.
- `origin` is the public `Zer0one/mame-controls` fork.
- `retro-controls` contains three reconstructed source commits followed by
  fork-specific documentation.
- `RETRO_CONTROLS.md` is the authoritative migration and provenance note.
- `docs/history/original-patches/` preserves the five pre-Codex patch
  artifacts, including two superseded/intermediate revisions.

## Editing rules

- Keep the three functional changes logically independent.
- Avoid unrelated formatting or generated-file changes.
- Follow the upstream MAME coding style: tabs for initial indentation and
  minimal whitespace churn.
- Do not copy ROMs, user configuration, build products or personal paths into
  commits.
- Do not alter `/Users/andrea/dev/mame`; treat the archived patch artifacts
  under `docs/history/original-patches/` as immutable provenance.

## Verification

For source changes, run the narrowest relevant partial build first:

```sh
make SUBTARGET=retro-controls \
  SOURCES=src/mame/sega/model1.cpp,src/mame/sega/segas32.cpp \
  REGENIE=1 -j8
```

Before publishing or rebasing, also inspect:

```sh
git diff --check upstream/master...HEAD
git log --reverse --oneline upstream/master..HEAD
```

Runtime verification requires the games and controller setup described in
`RETRO_CONTROLS.md`; do not claim those checks passed based on compilation
alone.

## Upstream updates

Never merge an upstream update blindly into the operational branch. Start from
the intended upstream commit on a temporary branch, replay the three source
commits in order, resolve each conflict independently, and repeat the tests.
