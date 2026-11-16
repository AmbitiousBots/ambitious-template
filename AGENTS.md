# Agent guide

## Goal

Ambitious is a Grok Bot configuration and monitoring service: a free Ambitious Bot template, flat per-org monthly monitoring with human review, and open-source single-purpose CLI modules with no UI that the bot drives.

This repository is a GitHub template for one of those modules. It has no module code yet. It is not the Ambitious Bot template (`ambitious-template-<version>.tgz` on a kit release) and it is not the kit's repo-template, which `scripts/apply-template.mjs` copies from ambitious-kit.

A change is good when an author can start a module that the bot can drive, or when the guides stay aligned with the contract that [ambitious-kit](https://github.com/AmbitiousBots/ambitious-kit) publishes. Once this copy has become a module, a change is good when the bot can do more for the person who talks to it, or the same with less risk. That person never sees this code.

After you adopt a kit release, rewrite this file so it describes your module. You can start from [repo-template/AGENTS.md](https://github.com/AmbitiousBots/ambitious-kit/blob/main/repo-template/AGENTS.md) and delete any section that is only about starting from this GitHub template.

## Starting a module

Follow [README.md](README.md). Do not invent a layout, an envelope, an installer, or a pin process.

- The contract is [MODULE_CONTRACT.md](MODULE_CONTRACT.md), a copy of the contract in ambitious-kit. The kit is the source of truth. In that copy, "this repository" means ambitious-kit. When the two differ, follow the kit, replace this copy with the contract from the kit release you adopted, and say so in the pull request under `## Decisions`.
- Add code only by adopting a kit release, as [Start a module on the kit](https://github.com/AmbitiousBots/ambitious-kit#start-a-module-on-the-kit) describes. Run `scripts/apply-template.mjs` from a kit checkout, vendor the release tarballs after checking `SHA256SUMS`, and copy the remaining build files from the kit's `examples/hello` (TypeScript) or the kit's Python package. That command never overwrites a file that already exists, so follow [Files to replace after you adopt the kit](README.md#files-to-replace-after-you-adopt-the-kit).
- Fill every `<<PLACEHOLDER>>` in `skills/`. Rename `skills/your-module-for-bots` and, if you keep it, `skills/your-module-page`. Skills in a module repository are named without the `ambitious-` prefix.
- Keep [LICENSE](LICENSE) as Apache-2.0 and keep a [NOTICE](NOTICE) file. Set the first line of `NOTICE` to the new repository name, and set the copyright holder to whoever owns this module.
- Do not copy a binary into `$AMBITIOUS_HOME/bin` by hand, and do not try to edit the pins inside a bot's installer. To try one exact release on a computer you control, use the kit's `ambitious-install` command after checking it against the kit release's `SHA256SUMS`. That check is download integrity, not the pin chain an Ambitious Bot uses. An Ambitious Bot installs only the modules pinned in its current template, and only through `ambitious-modules sync`.

## How to work

- Work in small steps and prove each one: say what the change must do, make the smallest change that does it, then run the checks and show their output. Model choices per agent role are in `.cursor/rules/agent-models.mdc`. Claude Code analysis runs use `--model claude-fable-5-2`.
- Trust in this order: types and API design, then lint and the dependency graph, then tests and verification, then prose.
- When you are corrected, turn the correction into a lint rule with a fix-the-root-cause message or into an API change. Do not add it to this file.
- When a check fails, fix the cause. Never weaken or skip a check to make it pass.

## Data shape

- In: JSON on stdin or `--in`, for commands that take input. Out: one JSON envelope on stdout, `{ ok, schemaVersion, module, command, data, errors, meta }`.
- State: one SQLite file per module, at `$AMBITIOUS_HOME/<module>/<module>.sqlite` unless `--db` names another file. `AMBITIOUS_HOME` defaults to `/workspace/ambitious`. After you adopt the kit, the database shape is the files in `migrations/` plus the types in `src/domain/`. Read those before changing behavior; they are the source of truth, not this file.
- Refuse every path under `/home/box/agent-data/`.
- The full contract, including `selfcheck`, install assets, versioning, and the release gates, is [MODULE_CONTRACT.md](MODULE_CONTRACT.md).

## Directory boundaries

These directories appear once you adopt the kit:

- `src/cli/`: argument parsing and the envelope only. The one place that reads the clock, randomness and the environment.
- `src/domain/`: pure logic with no IO. Time and randomness arrive as arguments.
- `src/db/`: the only place that imports `node:sqlite` (or, in Python, that opens SQLite).
- `src/ai/`: the only place with network calls, and only for keyed AI calls.
- `spec/`: TLA+ models. `proofs/`: Lean, where used. `test/`: tests. `verify/`: the verification skill.
- `skills/`: bot-facing guides. They tell the bot how to call the installed binary under `$AMBITIOUS_HOME/bin`. They must not claim that the module renders documents, and they must not tell the bot to compute numbers the module already returns.

## Judgment calls

- Which layer a piece of code belongs in: if it can be stated without IO, it is domain logic, even when it is first needed by a database or AI call.
- Whether a behavior needs a model in `spec/` before code: yes when it has ordering, retries, concurrency or a state machine a person relies on.
- What an error says: plain words a bot can relay, and a hint the bot can act on without a human.
- How much to build: the smallest change that meets the goal. Delete before you add.
- What counts as proof: output from `verify/run` against a fresh `AMBITIOUS_HOME`, not a reading of the diff.
- What to write down: a choice you made without asking goes in the PR under `## Decisions`.
- Wording: keep code and docs generic for every kind of organization. No person, bot or customer names, no industry terms, and no compliance or security claim beyond what the code enforces.
