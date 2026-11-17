# PS4 Slim optimization — experiment 01

This is a **test profile**, not a proven performance improvement. It does not alter the original port's `ps4/configs/native.toml` and does not include any game assets.

## Files and branch

- Branch: `ps4-slim-optimization`
- Reference: `ps4/configs/native.toml` (upstream build CX)
- Experiment: `ps4/configs/native-slim-performance.toml`
- Console runtime config: `/data/LibertyRecomp/native.toml` (no hyphen in the directory name)

## Objective

Test whether reducing scene complexity improves PS4 Slim frame times. The PS4 Pro baseline in the original README is CPU-limited. The Slim has **not** been benchmarked by this experiment. Reducing screen resolution alone previously did not help on the Pro.

Changes relative to upstream profile:
- View distance: 0.50 → 0.35
- Shadow distance: 0.50 → 0.25
- Traffic / parked cars / pedestrians / scenario pedestrians: 0.50 → 0.30
- Sun shadow update interval: 2 → 3 frames

The renderer texture detile mode, memory arena, texture memory limit, frame limiter, logging and other compatibility values stay unchanged. Lower crowd density may change traffic, NPC appearances and mission behavior.

## How to test (GoldHEN / FTP)

1. Have a working GTA4-PS4 installation using legally obtained Xbox 360 game data and matching title update, as documented in the original README. Do not upload proprietary game files to GitHub.
2. Back up the **current** console file `/data/LibertyRecomp/native.toml` to your PC. Keep it for recovery.
3. First run a **reference test** with your known-working configuration. Record the firmware, build, display mode, scene and FPS (at least 30 seconds per scene). Record whether audio stays in sync, and note any glitches.
4. Rename the contents of `native-slim-performance.toml` to `native.toml` when copying to `/data/LibertyRecomp/` on the console. The executable reads this path, not the file from GitHub.
5. Restart the app and repeat the *same* scenes under similar conditions; collect logs from `/data/Liberty Recompiled/logs/` if available.
6. If it fails to launch or behavior worsens, restore the backup `native.toml`.

## Benchmark sheet

| Test | Config | Location/scenario | Resolution | FPS range | Audio | Draw or visual bugs | Notes |
|---|---|---|---|---|---|---|---|
| A | Original | Roman's apartment, idle | | | | | |
| B | Slim experimental | Roman's apartment, idle | | | | | |
| A | Original | Same short driving route | | | | | |
| B | Slim experimental | Same short driving route | | | | | |

Use a video recording if possible to review 1% lows and repeated frame-time spikes; report average and lows only if actually measured. The goal is **stable frame times**, not a momentary peak FPS.

## Later investigation

If the Slim experiments show that fewer draws help, profile before modifying the renderer; see `ps4/RENDERER-REWORK-SCOPE.md`. That document records negative results from several CPU-side changes, so avoid repeating them without a new hypothesis. Focus on how changes affect both the game render thread **and** Vulkan recording thread. Texture downscaling alone may save memory but is unlikely to cure a CPU bottleneck.

**Status:** Configuration committed. No PS4 Slim run, compiled PKG or FPS gain has been verified.
