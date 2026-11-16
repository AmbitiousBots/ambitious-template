# ambitious-template

This repository is a GitHub template for a new [Ambitious](https://ambitiousbots.com) module. An Ambitious module is an open-source, single-purpose command-line tool with no user interface. A bot runs it on a Grok Bot computer, passes JSON in, and reads JSON out.

This repository contains no module code. It contains the files you need to start: the module contract, skill templates, coding-agent guides, and the standard open-source files used by the existing public module repositories. You add the code by adopting [ambitious-kit](https://github.com/AmbitiousBots/ambitious-kit), which is the source of the contract, the shared helpers, the checks, and the installer.

The word "template" is used in three places, and they are not the same thing:

| Name | What it is |
| --- | --- |
| This GitHub template | This repository: a starting point for a new module. |
| The kit's repo-template | The files `scripts/apply-template.mjs` copies from [ambitious-kit](https://github.com/AmbitiousBots/ambitious-kit/tree/main/repo-template) into a module repository. |
| The Ambitious Bot template | The free bot's instructions, skills, routines, and module pins. It ships as `ambitious-template-<version>.tgz` on each [kit release](https://github.com/AmbitiousBots/ambitious-kit/releases). This GitHub template is not that archive, and it publishes no release assets of its own. |

## What a module is

A module does one job and exposes it as commands. Every command prints one JSON envelope on stdout. A command that needs input reads JSON from stdin or from `--in`. Commands that take no input, including `selfcheck` and `commands`, still print the same envelope shape.

The short envelope in rule 1 of the contract looks like this:

```json
{ "ok": true, "schemaVersion": 1, "module": "tasks", "command": "next", "data": {}, "errors": [], "meta": { "now": "2026-01-01T00:00:00.000Z", "seed": 7, "version": "0.1.0" } }
```

The ten rules, the shared error codes, the worked examples, and the release gates are in [MODULE_CONTRACT.md](MODULE_CONTRACT.md). That file is a verbatim copy of the contract published in ambitious-kit `kit-v0.2.7`. The kit remains the source of truth. In that copy, "this repository" and "the kit" mean ambitious-kit, so a proposed change to the contract belongs there. When you adopt a later kit release, replace this copy with the contract from that release.

A module is built to run on a Grok Bot computer, and it runs the same way on any machine with the right runtime (Node 24 for a TypeScript module, Python 3.13 for a Python module). It keeps its data under `AMBITIOUS_HOME`, which defaults to `/workspace/ambitious` on a Grok Bot computer. It refuses every path under `/home/box/agent-data/`, the bot's private data directory.

Existing public modules include [ambitious-tasks](https://github.com/AmbitiousBots/ambitious-tasks), [ambitious-records](https://github.com/AmbitiousBots/ambitious-records), [ambitious-contracts](https://github.com/AmbitiousBots/ambitious-contracts), [ambitious-compliance](https://github.com/AmbitiousBots/ambitious-compliance), [ambitious-audit](https://github.com/AmbitiousBots/ambitious-audit), and [ambitious-video](https://github.com/AmbitiousBots/ambitious-video). Use them as examples. Do not invent a different envelope, a different install layout, or a different `selfcheck`.

## Start from this GitHub template

1. On GitHub, use this repository as a GitHub template and create a new public repository named `ambitious-<<MODULE>>`, where `<<MODULE>>` is a short lowercase name such as `tasks` or `records`. You can also clone this repository and change the remote. Enable [private vulnerability reporting](https://docs.github.com/en/code-security/security-advisories/working-with-repository-security-advisories/configuring-private-vulnerability-reporting-for-a-repository) on the new repository, so [SECURITY.md](SECURITY.md) and the [code of conduct](CODE_OF_CONDUCT.md) have a private form to point at.
2. Fill in every `<<PLACEHOLDER>>` in the skill files listed under [Layout](#layout). Rename the example skill directories under `skills/` from `your-module-*` to `<<MODULE>>-for-bots` and, if you need a Page skill, `<<MODULE>>-page`.
3. Update [NOTICE](NOTICE): change the first line to the new repository name, and put the copyright holder who owns this module in place of Ambitious Corporation if that is not you.
4. Adopt the latest [ambitious-kit](https://github.com/AmbitiousBots/ambitious-kit) release, following [Start a module on the kit](https://github.com/AmbitiousBots/ambitious-kit#start-a-module-on-the-kit). That is the supported way to add code, checks, and the verification bar. In short, for a TypeScript module:
   1. From a checkout of ambitious-kit, run `node scripts/apply-template.mjs <path-to-your-repo>`. It creates missing files and never overwrites a file that already exists. Add `--dry-run` to see the list without writing. Because this GitHub template already ships many of the same paths as the kit's repo-template, those files are left as they are. After the command runs, do the replacements in [Files to replace after you adopt the kit](#files-to-replace-after-you-adopt-the-kit).
   2. Download the kit's `ambitious-kit`, `ambitious-lint`, and `ambitious-depcruise` `.tgz` files plus `SHA256SUMS` from that release, check them with `shasum -a 256 -c SHA256SUMS --ignore-missing`, and commit them under `vendor/`.
   3. Add those packages as development dependencies and copy the remaining build files from [examples/hello](https://github.com/AmbitiousBots/ambitious-kit/tree/main/examples/hello), as the kit README describes.
   4. Write the module in `src/`, add an entry to `verify/features.json` for each command, and run the checks the kit lists.
5. If the kit release you adopted is newer than `kit-v0.2.7`, replace [MODULE_CONTRACT.md](MODULE_CONTRACT.md) with the contract from that release. Compare `.mise.toml`, `.gitignore` and `.standalone.json` with that release's repo-template as well, and take any updates you still want.

For a Python module, apply the kit's repo-template the same way and follow the kit's [Python package](https://github.com/AmbitiousBots/ambitious-kit/tree/main/packages/kit-python).

Do not start from a blank layout, and do not copy an unofficial fork of the kit. The contract requires a module repository to adopt a kit release before module code is merged, because the kit is what checks most of the rules.

### Files to replace after you adopt the kit

`apply-template.mjs` will not overwrite these. Replace them yourself so the repository describes your module, not this GitHub template:

| File | What to do |
| --- | --- |
| `README.md` | Write a module README. The contract requires a table of every command marked `read` or `write`, and a table of the module's own error codes with the same columns as the contract's shared-code table. Existing modules such as [ambitious-tasks](https://github.com/AmbitiousBots/ambitious-tasks) show the shape. |
| `AGENTS.md` | Rewrite it for this module. You can start from [repo-template/AGENTS.md](https://github.com/AmbitiousBots/ambitious-kit/blob/main/repo-template/AGENTS.md) and delete any section that is only about starting from this GitHub template. |
| `CONTRIBUTING.md` | Rewrite it for this module. You can start from [repo-template/CONTRIBUTING.md](https://github.com/AmbitiousBots/ambitious-kit/blob/main/repo-template/CONTRIBUTING.md). |
| `CHANGELOG.md` | Start the module's own changelog. Do not keep the entry that records this GitHub template. |
| `NOTICE` | Keep Apache-2.0 attribution, with this module's repository name and the correct copyright holder. |
| `skills/` | Finish every `<<PLACEHOLDER>>` and rename the example directories. Delete the Page skill directory if this module has no Page view. |

Leave [LICENSE](LICENSE), [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md), [SECURITY.md](SECURITY.md) and the issue forms as they are unless a later kit release changes them.

## Layout

| Path | What it is |
| --- | --- |
| [MODULE_CONTRACT.md](MODULE_CONTRACT.md) | The module contract, copied from ambitious-kit `kit-v0.2.7`. |
| [AGENTS.md](AGENTS.md) | Rules for coding agents that build or change a module. [CLAUDE.md](CLAUDE.md) points at this file. |
| [skills/](skills/) | Skill templates the bot follows when it drives the module. See [skills/README.md](skills/README.md). |
| [LICENSE](LICENSE) and [NOTICE](NOTICE) | Apache License 2.0, which every module must use (contract rule 10). |
| [CONTRIBUTING.md](CONTRIBUTING.md), [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md), [SECURITY.md](SECURITY.md) | How to file issues, the Contributor Covenant, and private vulnerability reporting. |
| [CHANGELOG.md](CHANGELOG.md) | Changes to this GitHub template. A module started from it keeps its own changelog. |
| [.github/](.github/) | Issue and pull request templates. |
| [.gitignore](.gitignore) | Ignores for the directories a module grows once you adopt the kit. |
| [.mise.toml](.mise.toml) | Pinned Node, Python, and Java versions, matching the kit's repo-template at `kit-v0.2.7`. |
| [.standalone.json](.standalone.json) | The documented runtime paths that the kit's standalone check is allowed to see. |
| [.cursor/rules/agent-models.mdc](.cursor/rules/agent-models.mdc) | Model choices per agent role, as used in the existing public module repositories. |
| `.markdownlint.json`, `.markdownlintignore`, `.markdown-link-check.json` | Config for the Markdown and link checks in [CONTRIBUTING.md](CONTRIBUTING.md). |

After you adopt the kit, the repository also has the layout the contract and [AGENTS.md](AGENTS.md) describe: `src/cli/`, `src/domain/`, `src/db/`, and when needed `src/ai/`; `migrations/`; `test/`; `spec/`; `verify/`; `vendor/`; and `scripts/`. A Python module follows the same ideas in the kit's Python package.

## Install and test on a computer you control

`AMBITIOUS_HOME` is the one directory where modules keep everything. When it is not set, it defaults to `/workspace/ambitious`, which is the right place on a Grok Bot computer. On any other machine, and in tests, set it to a directory of your choice.

| Path | What lives there |
| --- | --- |
| `$AMBITIOUS_HOME/<module>/<module>.sqlite` | The module's SQLite database, unless `--db` names another file. |
| `$AMBITIOUS_HOME/bin/` | Installed release files, a `SHA256SUMS` file with the verified checksum line for each installed asset, and a link named `ambitious-<module>` that points at the current one. |

A module must refuse every path under `/home/box/agent-data/` with `E_FORBIDDEN_PATH`.

### Test a build yourself

Set `AMBITIOUS_HOME` to a fresh temporary directory, then run the built binary (a single `.mjs` or `.pyz` file) with Node 24 or Python 3.13. Start with `selfcheck` and `commands`. The contract tests and `verify/run` each use a fresh temporary `AMBITIOUS_HOME` as well. A run is repeatable when you pass back the `now` and `seed` values from `meta`.

To install one exact release into `$AMBITIOUS_HOME/bin` on a computer you control, use the kit's single-module installer. Get `ambitious-install-<version>.mjs` and `SHA256SUMS` from the same [kit release](https://github.com/AmbitiousBots/ambitious-kit/releases) you adopted, and check the installer against that `SHA256SUMS` before you run it:

```text
node ambitious-install-0.2.7.mjs tasks 0.1.2 --from https://github.com/AmbitiousBots/ambitious-tasks/releases/download/tasks-v0.1.2
```

Replace `tasks`, `0.1.2` and the `--from` address with your module name, version, and release directory or release address. The installer checks the checksum in that release's `SHA256SUMS` so the downloaded file matches what the release published. That is a download-integrity check. It is not the pin chain an Ambitious Bot uses, which is described below. The installer writes the asset into `bin` and is safe to run again. It does not change any module database. Do not copy a binary into `bin` by hand.

### What an Ambitious Bot installs

An Ambitious Bot runs one command, described in the `ambitious-module-installer` skill that ships with the Ambitious Bot template:

```sh
node "${AMBITIOUS_HOME:-/workspace/ambitious}/bin/ambitious-modules" sync --json
```

`sync` installs, repairs, or upgrades every module pinned by that kit release, then runs each module's `selfcheck --json`. A file is installed only when its SHA-256 matches the pin inside the installer. The bot does not install a module that is not in that pin list, and it does not take a pin, an address, or a checksum from a request.

The official pin list is [template/modules.lock.json](https://github.com/AmbitiousBots/ambitious-kit/blob/main/template/modules.lock.json) in ambitious-kit. How a maintainer changes a pin is documented in the [kit README](https://github.com/AmbitiousBots/ambitious-kit#the-ambitious-bot-template). This GitHub template does not add an intake process on top of that. A pin lives inside the installer that a kit release ships. You cannot edit it from this repository, and you should not try to edit it on a bot computer.

An Ambitious Bot gets a new pin only when its owner updates that bot's template to a kit release that includes the pin. Shipping a kit release is not enough on its own, because the installer never updates itself.

Until a kit release pins your module, you and anyone else can still run it with `AMBITIOUS_HOME` set, or install one exact release with `ambitious-install` on a computer you control, as shown above. The Ambitious Bot's own skill tells the bot to run `sync` and not to install modules by hand, so do not use `ambitious-install` as a substitute for `sync` on a bot that already follows that skill.

## Publish

1. Pass every gate in Part 4 of [MODULE_CONTRACT.md](MODULE_CONTRACT.md). Do not weaken or skip a check to make it pass.
2. Keep the repository public and licensed Apache-2.0, with [LICENSE](LICENSE) and [NOTICE](NOTICE) at the root.
3. Create a GitHub release tagged `<<MODULE>>-v<<VERSION>>`, for example `tasks-v0.3.1`. Attach exactly the files the contract requires: `ambitious-<<MODULE>>-<<VERSION>>.mjs` (or `.pyz` for Python) and `SHA256SUMS` in `sha256sum` format.
4. Leave the skills in this repository named without the `ambitious-` prefix. If a later kit release copies a skill into the Ambitious Bot template, that process adds the prefix and a `Template version` line. That copy step is part of pinning the module in the kit; it does not happen for a module the kit has not pinned.

See contract rule 5 for the asset names, the checksum file, and how the single-module installer behaves, and rule 9 for version numbers and `schemaVersion`.

## Contributing

[CONTRIBUTING.md](CONTRIBUTING.md) says how to file an issue, set up a checkout, run the checks, and open a pull request. Everyone taking part follows the [code of conduct](CODE_OF_CONDUCT.md). Report a security vulnerability privately, as [SECURITY.md](SECURITY.md) describes, not in an issue.

Part of [Ambitious](https://ambitiousbots.com). Licensed under the Apache License, Version 2.0; see [LICENSE](LICENSE) and [NOTICE](NOTICE).
