# Migration verification

Verification date: 26 July 2026

Verified source commit:

`114ea9ac86cbabc9f7ced24b493b2c2cb161ab2b`

## Environment

- macOS on ARM64;
- Clang 21.0.0;
- generated MAME version:
  `0.288 (mame0288-771-g114ea9ac86c)`.

## Source equivalence

The following files were compared byte-for-byte with the operational checkout
at `/Users/andrea/dev/mame`:

- `src/emu/emuopts.cpp`;
- `src/mame/sega/model1.cpp`;
- `src/mame/sega/segas32.cpp`;
- `src/mame/sega/segas32.h`.

All four comparisons passed.

## Build

Command:

```sh
make SUBTARGET=retro-controls \
  SOURCES=src/mame/sega/model1.cpp,src/mame/sega/segas32.cpp \
  REGENIE=1 -j8
```

Result: successful.

The generated binary is an ARM64 Mach-O executable named `retro_controls`.
Its SHA-256 for this build is:

`90029217c772c2deb8cbf817d3323395adaf89aadd81791efb788fd43f109d24`

The compiler emitted deprecation warnings from existing MAME and third-party
macOS code. It emitted no error for the reconstructed control changes.

## Non-interactive checks

- `retro_controls -validate`: passed.
- `retro_controls -listfull swa radm radr`: all three systems found.
- `retro_controls -joystick_saturation 1.5 -showconfig`: accepted and reported
  `1.5`.
- `retro_controls -listxml radm radr`: both systems expose:
  - `Steering Response`;
  - `Steering Slew Step`;
  - `Steering Output Range`;
  - default output range `100%`;
  - default slew `255 (Direct)`.
- Git connectivity check: passed.
- `git diff --check upstream/master...HEAD`: passed.

## Checks still requiring the operational setup

Compilation and metadata inspection do not replace physical controller tests.
Before a public release, verify with the appropriate ROMs and controller:

1. the complete Star Wars Arcade throttle range;
2. Rad Mobile and Rad Rally with the direct defaults;
3. progressive and FBNeo steering curves;
4. reduced output ranges;
5. each non-direct slew setting intended for use.

No ROM or controller-dependent result is claimed by this report.
