# Original pre-Codex patch artifacts

These five files were found in `/Users/andrea/dev/patches`. They were created
between 16 and 18 July 2026 during the MAME controls work reconstructed from
the Retro-Script conversations.

They are preserved verbatim. SHA-256 and stable Git patch IDs were verified on
26 July 2026.

| Artifact | SHA-256 | Stable patch ID | Classification |
|---|---|---|---|
| `mame-swa-throttle-final.patch` | `254cbcd0bd4f8a16704d2edb85360e0252b5300e4509246ff5e4296244b9107b` | `6d9b25273a5d3eaba45998a2d462511a80c4e226` | Original commit `83e763fe68c`; replayed as `3b822ce13dd` |
| `mame-radm-radr-steering-final.patch` | `04fa5bdfd805c9b47cc29381dec998cd33e2a23d078198cec49243d02d5bf204` | `8e343dd8a5887a9035600f408cdd1aa9e4bdb629` | Original commit `902913930ef`; replayed as `3d6fc7808fd` |
| `mame-joystick-saturation-final.patch` | `ff964bca3c76c47f364e2f1e425f29f8fa2a6ff3c4a8eefc3b7772bb6a866172` | `e43ec43b621a2f85b2e9bafd4ba1f733d65f6b46` | Original commit `ea30b4846d0`; replayed as `b5febf3f5ad` |
| `mame-swa-throttle.patch` | `ef93d6217b61672a5e027af7c67ef1607afd8a8b9c9e058a49dedc6243400cca` | `6d9b25273a5d3eaba45998a2d462511a80c4e226` | Earlier source-context version; semantically identical to the final artifact |
| `mame-radm-radr-steering-limit.patch` | `d173a6a57e508aa8e7b35d9ae60d211b322d61e94292d5f51ce4c9a05cdffc2d` | `c94c5249db2e31d7cb92826501c7f6ab4502247c` | Superseded fixed-range prototype, retained only as development history |

The three `*-final.patch` files also pass a reverse-application check against
the current `retro-controls` branch, confirming that their changes are already
present. Do not apply the intermediate artifacts to the active branch.
