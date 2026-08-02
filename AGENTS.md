# Codex instructions for the personal MAME fork

## Scope

This checkout is the persistent development source for Andrea's personal MAME
fork. It currently preserves three local control changes and may contain
unrelated future custom patches. Its local path is
`/Users/andrea/dev/mame-my`.

## Repository structure

- `upstream` is the official `mamedev/mame` repository.
- `origin` is the public `Zer0one/mame` fork.
- `main` contains three reconstructed source commits followed by
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
- Treat the archived patch artifacts under `docs/history/original-patches/` as
  immutable provenance.

## Verification

For standalone source changes, run the narrowest relevant partial build first:

```sh
make SUBTARGET=retro-controls \
  SOURCES=src/mame/sega/model1.cpp,src/mame/sega/segas32.cpp \
  REGENIE=1 -j8
```

Before publishing or rebasing, also inspect:

```sh
git diff --check upstream/master...HEAD -- . \
  ':(exclude)docs/history/original-patches/*.patch'
git log --reverse --oneline upstream/master..HEAD
```

Runtime verification requires the games and controller setup described in
`RETRO_CONTROLS.md`; do not claim those checks passed based on compilation
alone.

## Upstream updates

The standard upstream-update workflow is performed directly on the local
`main` branch; do not create a temporary integration branch. Fetch `upstream`
and `origin`, inspect the incoming changes (especially Model 1, Model 2 and the
files touched by this fork), and create a dated local safety tag at the current
`main` commit before rewriting history.

Rebase/replay the source changes separately and in chronological order onto
the intended `upstream/master` commit, resolving each conflict on `main` and
preserving logical commit separation. For a routine upstream update, perform
one complete native ARM64 build with `make -j8`, followed by
`./mame -validate` and the fork-specific checks. Address failures directly on
local `main` and rebuild as required.

Publish the verified result to `origin/main` using `--force-with-lease` when
history has been rewritten. Never push to `upstream`.
