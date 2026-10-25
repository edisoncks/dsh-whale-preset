# Changelog

All notable changes to this project are documented here. The format follows
[Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and this project
adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.0.0] - 2026-10-25

First public release.

### Added

- `preset-whale` (preset id `whale`, "Whale-chan"): the shipped `ptc` plugin
  list for English replies, with `tool-web` disabled.
- `preset-whale-zh` (preset id `whale-zh`, "鲸鱼娘"): the same plugin list for
  Simplified Chinese replies.
- Bundle metadata via `dsh.bundle.patch`, so `dsh plugin ... add` appends the
  package to `dsh.profile.bundles` by itself.
- MIT license.

### Notes

- The two plugin lists are duplicated on purpose; update both when porting an
  upstream `ptc` change.
- Disabling `tool-web` is not a network sandbox. `bash` still reaches the
  network.
