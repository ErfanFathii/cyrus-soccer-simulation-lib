# Cyrus observation research adapters

This fork adds two opt-in adapters used by the instantaneous-denoising collector:

- `CYRUS_ALL_PLAYERS_VISIBLE=1`: interpret current visual updates as covering 360
  degrees, including view history and resetting all direction-age counters.
- `CYRUS_GAUSSIAN_SEE=1`: retain received distances instead of inverse quantizing
  them. Localization uses an approximate three-sigma uncertainty envelope with
  a minimum of 5 cm. This is an untrained decoder, not a learned denoiser.

Unset flags preserve the existing library behavior. The Gaussian adapter requires
server distance-noise rates of 0.025 for movable objects and 0.0025 for landmarks,
with both focus-noise rates zero. The Cyrus team requests `(gaussian_see)`;
setting the library environment variable alone does not negotiate that channel.

The server supplies fresh noisy observations, not fullstate. Position-age
counters reset through normal observation processing, rather than by overriding
stale counters. The legacy denoiser is disabled by the research team collector.

Use all three coordinated branches:

- [Library fork](https://github.com/ErfanFathii/cyrus-soccer-simulation-lib/tree/research-denoising-only)
- [Server](https://github.com/ErfanFathii/rcssserver/tree/research-denoising-only)
- [Team, build instructions and collection guide](https://github.com/ErfanFathii/cyrus-soccer-simulation-team/tree/research-denoising-only/script/research)

The team guide documents the isolated build prefix, equal quantized/Gaussian
collection, separate server/client logs, exact current-state supervision and
verification from eight short games. No movement-prediction model is included.
