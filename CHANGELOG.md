# Changelog

All notable changes to this project are documented here. The format follows
[Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and this project
adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.0.2] - 2026-10-26

### Changed

- Move the persona identity sentence — the one that declares the agent and its
  reply language — from the top of `prefix` to directly beneath the persona
  marker tokens, in both presets. The token block now opens the prompt as a load
  directive, and the sentence shares a paragraph with the invariant rule it
  belongs to. Text, token count, and section order are unchanged.

## [1.0.1] - 2026-10-25

### Changed

- Fold the persona invariant paragraph into `prefix` and drop `suffix`. The
  persona row mounts its suffix at the `DEPLOYMENT_PERSONA_SUFFIX` section
  order, which puts it near the end of the system prompt — far from the persona
  block it qualifies. Both presets now carry the rule directly beneath the
  persona marker tokens, and no `suffix` key remains.

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
