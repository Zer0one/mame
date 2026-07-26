# Retro controls branch

This branch preserves three local MAME control changes reconstructed from the
Retro-Script ChatGPT project and from the patch files that were still in use on
26 July 2026.

It is intentionally based on MAME commit
`ee298d84e0c7e409f4bb7ce4da10bb69b06edc4f`.

## Changes

| Commit | Original patch | SHA-256 | Purpose |
|---|---|---|---|
| `83e763fe68c` | `mame-swa-throttle-final.patch` | `254cbcd0bd4f8a16704d2edb85360e0252b5300e4509246ff5e4296244b9107b` | Map the Star Wars Arcade throttle as an analog Z stick with the required range and centre. |
| `902913930ef` | `mame-radm-radr-steering-final.patch` | `04fa5bdfd805c9b47cc29381dec998cd33e2a23d078198cec49243d02d5bf204` | Add response curve, output range and slew controls for Rad Mobile and Rad Rally. |
| `ea30b4846d0` | `mame-joystick-saturation-final.patch` | `ff964bca3c76c47f364e2f1e425f29f8fa2a6ff3c4a8eefc3b7772bb6a866172` | Accept joystick saturation values up to 2.0 while keeping the upstream default. |

The original working tree was:

`/Users/andrea/dev/mame`

It is an operational reference and must not be modified by migration work. At
the time of reconstruction, the four affected files in this branch were
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

No `origin` is configured yet. It is reserved for the user's GitHub fork.

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

1. Confirm that the targeted build completes without errors.
2. Check that `joy_saturation` accepts values greater than 1.0 and retains the
   default value `0.85`.
3. Verify the Star Wars Arcade throttle over its complete usable range.
4. Verify Rad Mobile and Rad Rally with the default settings:
   linear response, 100% output range and direct slew.
5. Test the optional progressive and FBNeo response curves.
6. Test reduced steering ranges and non-direct slew values.
7. Compare the four affected files with the operational reference before
   declaring a migrated revision equivalent.

The default Rad Mobile/Rad Rally settings are deliberately intended to preserve
upstream-like direct behaviour. Optional processing is selected through the
new configuration ports.

## Updating from upstream

Fetch upstream, create a temporary integration branch and replay the three
source commits in chronological order:

```sh
git fetch upstream
git switch -c retro-controls-update upstream/master
git cherry-pick 83e763fe68c 902913930ef ea30b4846d0
```

Resolve conflicts one change at a time and repeat the targeted and runtime
checks. Do not overwrite the operational working tree as part of an update.
