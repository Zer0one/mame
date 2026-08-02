# MAME controls fork

This branch preserves three local MAME control changes reconstructed from the
Retro-Script ChatGPT project and from the patch files that were still in use on
26 July 2026.

It is intentionally based on MAME commit
`3279399312dcea4b860f371bdede48a1b536bad1`.

The changes were originally reconstructed on
`ee298d84e0c7e409f4bb7ce4da10bb69b06edc4f`, then replayed in order on the
current upstream base on 2 August 2026.

## Changes

| Commit | Original patch | SHA-256 | Purpose |
|---|---|---|---|
| `13a78cce478` | `mame-swa-throttle-final.patch` | `254cbcd0bd4f8a16704d2edb85360e0252b5300e4509246ff5e4296244b9107b` | Map the Star Wars Arcade throttle as an analog Z stick with the required range and centre. |
| `418b4989106` | `mame-radm-radr-steering-final.patch` | `04fa5bdfd805c9b47cc29381dec998cd33e2a23d078198cec49243d02d5bf204` | Add response curve, output range and slew controls for Rad Mobile and Rad Rally. |
| `6a009ed86fd` | `mame-joystick-saturation-final.patch` | `ff964bca3c76c47f364e2f1e425f29f8fa2a6ff3c4a8eefc3b7772bb6a866172` | Accept joystick saturation values up to 2.0 while keeping the upstream default. |

The five pre-Codex patch artifacts are preserved under
`docs/history/original-patches/`. Stable Git patch IDs confirm that the three
final artifacts are exactly the changes recorded by the corresponding source
commits. The earlier `mame-swa-throttle.patch` has the same patch ID as its
final counterpart, while `mame-radm-radr-steering-limit.patch` records a
superseded fixed-range prototype.

The pre-migration operational working tree occupied:

`/Users/andrea/dev/mame`

It was used as a read-only reference during migration and moved to the Trash
before this checkout adopted that path. At the time of the original
reconstruction, the four affected files in the original branch were
byte-for-byte identical to that working tree:

- `src/emu/emuopts.cpp`;
- `src/mame/sega/model1.cpp`;
- `src/mame/sega/segas32.cpp`;
- `src/mame/sega/segas32.h`.

## Provenance

Primary conversation:

- `Sincronizzare e compilare MAME`;
- ChatGPT conversation ID:
  `6a57f31a-6a00-83ed-9011-fcdd6919ac59`.

Related operational context:

- `Visualizzare icone in MAME`;
- `Opzioni script MAME macOS`;
- the `mame.my` launcher in the macOS emulation toolkit.

The three source commits use the modification times of the final patch files
as reconstructed author and commit dates. This documentation commit was added
later and does not pretend to be part of the original development timeline.

## Remotes

`upstream` points to:

`https://github.com/mamedev/mame.git`

`origin` points to the public GitHub fork:

`https://github.com/Zer0one/mame.git`

## Targeted build

MAME supports partial builds using `SUBTARGET` and `SOURCES`. A targeted build
covering both modified drivers can be started with:

```sh
make SUBTARGET=retro-controls \
  SOURCES=src/mame/sega/model1.cpp,src/mame/sega/segas32.cpp \
  REGENIE=1 -j8
```

A full build remains the final verification:

```sh
make -j8
```

Build prerequisites and current platform requirements are documented upstream
in `docs/source/initialsetup/compilingmame.rst`.

## Verification checklist

1. Confirm that the required build completes without errors. For routine
   upstream updates this is one complete native ARM64 build with `make -j8`;
   standalone source development may use the targeted build first.
2. Check that `joy_saturation` accepts values greater than 1.0 and retains the
   default value `0.85`.
3. Verify the Star Wars Arcade throttle over its complete usable range.
4. Verify Rad Mobile and Rad Rally with the default settings:
   linear response, 100% output range and direct slew.
5. Test the optional progressive and FBNeo response curves.
6. Test reduced steering ranges and non-direct slew values.
7. Confirm that the three replayed commits retain the stable patch IDs recorded
   for the final archived artifacts.

The default Rad Mobile/Rad Rally settings are deliberately intended to preserve
upstream-like direct behaviour. Optional processing is selected through the
new configuration ports.

## Updating from upstream

Routine upstream updates are integrated directly on the local `main` branch.
Fetch both remotes, inspect the incoming commits (particularly Model 1, Model 2
and the files modified by this fork), and create a dated local safety tag at
the current commit before changing history:

```sh
git switch main
git fetch upstream origin
git tag upstream-update-backup-YYYY-MM-DD main
```

Rebase/replay the three source changes separately and in chronological order
onto the selected `upstream/master` commit. Resolve any conflict directly on
`main`, preserving the logical independence and stable patch identity of each
change.

For this routine, do not perform a preliminary targeted build. Run one complete
native ARM64 build and validate the resulting binary:

```sh
make -j8
./mame -validate
```

Then perform the fork-specific configuration checks described above. Fix any
problem directly on local `main` and rebuild only when necessary. Once the
result is verified, publish it to `origin/main`; because replaying the patches
rewrites their commit IDs, use `git push --force-with-lease origin main` when
required. Never push to `upstream`.
