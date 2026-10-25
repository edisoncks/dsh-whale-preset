# dsh-whale-preset

A DSH bundle that adds two agent presets, **Whale-chan** and **鲸鱼娘**: the
shipped `ptc` preset with `tool-web` disabled, one preset per persona language.
That is the whole idea.

There is no code here: a `package.json`, a 300-line YAML patch, a license,
and an `.editorconfig`. Nothing to build, no dependencies, no test suite. If
that seems small for a project, good. It is a configuration change, and a
configuration change is easier to review than a fork.

## Why

`ptc` hands an agent a filesystem, a shell, jobs, schedules, skills, goals,
plan mode, compaction, and subagents. It also hands it `tool-web`. If you want
all of that and no web tool — offline work, a locked-down profile, or you
simply do not want the model browsing — you had two options: fork the preset,
or override its plugin rows from your profile patch and hope the ids stay put.

A bundle is the third option. It inserts separate preset rows, so `ptc` stays
untouched and the whale presets coexist with it.

## What it changes

[`cordis.patch.yml`](cordis.patch.yml) inserts two rows, `preset-whale` and
`preset-whale-zh`, using `@deepseek-ai/dsh-agent-preset`. Each plugin list is
`ptc`'s, minus one entry:

| | `ptc` | `Whale-chan` | `鲸鱼娘` |
| --- | --- | --- | --- |
| row id | `preset-ptc` | `preset-whale` | `preset-whale-zh` |
| preset id | `ptc` | `whale` | `whale-zh` |
| `order` | 2 | 6 | 7 |
| `name` | unset | `Whale-chan` | `鲸鱼娘` |
| `description` | unset | English persona copy | Chinese persona copy |
| `tool-web` | enabled | `disabled: true` | `disabled: true` |

Otherwise both plugin lists are identical: persona, instructions, time context,
bash/pwsh, filesystem, filesystem search, jobs, schedule, skills, goals, plan
mode, compaction, subagents, ask-user, todo, presentation, and the disabled
workflow and plugin-manager rows. The lists are duplicated rather than shared,
so update both when porting upstream changes.

The descriptions cover each preset's persona and reply language; the toolset is
the same in both. Both entries appear in every UI language, so pick the one
whose reply language you want.

## Install

Install it into a profile that loads the agent preset registry; the `web`
profile does. The package is distributed from GitHub, not npm, so pin the tag
you want:

```sh
dsh plugin --profile web add github:edisoncks/dsh-whale-preset#v1.0.0
```

`pnpm` resolves the git tag into the profile's lockfile. To track the default
branch instead, drop the `#v1.0.0` fragment — but then the next install may
pick up unreleased changes.

The package declares `dsh.bundle.patch`, so a fresh `dsh plugin ... add`
appends it to `dsh.profile.bundles` in the profile's `package.json` after the
install. You do not edit that list by hand. Then restart the profile: bundle
layers are composed at boot.

To upgrade, add the new tag — `pnpm` replaces the dependency spec in place:

```sh
dsh plugin --profile web add github:edisoncks/dsh-whale-preset#v1.1.0
```

To make a whale preset the default, add this to the profile's
`cordis.patch.yml` and set `selectedDefault` to `whale` (Whale-chan) or
`whale-zh` (鲸鱼娘):

```yaml
- id: agent-preset-registry
  name: "@deepseek-ai/dsh-agent-preset-registry"
  config:
    selectedDefault: whale
```

To check that the layer composed:

```sh
dsh --profile web --dump-config | grep -A6 preset-whale
```

You can also enable or disable the bundle from Plugins in the Web UI, which
does the same bookkeeping live.

To remove it:

```sh
dsh plugin --profile web remove dsh-whale-preset
```

Removing the dependency also drops it from `dsh.profile.bundles`; the plugin
manager reconciles the list.

## Overriding it

Do not edit this bundle to change the presets. It is a snapshot; the supported
override point is the profile patch. Override either row by id — `preset-whale`
or `preset-whale-zh`:

```yaml
- id: preset-whale
  name: "@deepseek-ai/dsh-agent-preset"
  config:
    plugins:
      # your list
```

That is the supported path, and it keeps the diff here small.

## What this is not

- **Not a sandbox.** Disabling `tool-web` removes one tool from the model's
  tool list. It does not stop `bash` from running `curl`, and it does not stop
  a plugin from opening a socket. If you need network isolation, isolate the
  network: container, namespace, firewall, proxy. A YAML flag is not a
  security boundary, and pretending otherwise is how people get burned.
- **Not a fork, and it does not track upstream.** The preset lists are a
  snapshot of `ptc` as shipped in DSH 0.2.1-rc.3. When upstream changes, this
  file does not change with it. After a DSH upgrade, diff:

  ```sh
  diff <dsh-install>/node_modules/@deepseek-ai/dsh-web-app/presets/ptc.patch.yml cordis.patch.yml
  ```

  Port what changed to both rows, keep `tool-web` disabled in both, and re-read
  the result. The diff is the changelog.
- **Not a package on npm.** It is distributed from GitHub by tag, and there
  is no build step to break. "`git clone` and a YAML file" is the whole
  distribution story.

## Patches

Keep them small and say what breaks. This file is 300 lines of YAML; reflowing
it makes review impossible. Two spaces, LF, final newline — see
[`.editorconfig`](.editorconfig). There is no contributor guide because there
is one file to argue about. Releases are tagged `vX.Y.Z`; see
[`CHANGELOG.md`](CHANGELOG.md).

## License

[MIT](LICENSE) © 2026 Edison Chong. The persona text in
[`cordis.patch.yml`](cordis.patch.yml) ships under the same license; the
plugin rows it references belong to DeepSeek Harness and keep their own.
