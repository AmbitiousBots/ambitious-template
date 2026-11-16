---
name: <<MODULE>>-for-bots
description: <<ONE_LINE_WHEN_TO_USE>>. Use when a user asks to <<USER_GOALS>>.
---

<!--
  FILL IN every <<PLACEHOLDER>> in this file, then rename this directory
  to <<MODULE>>-for-bots. Delete this comment before you publish a release.
  This skill is the bot-facing guide for the module CLI. It is not the
  module contract; keep command rules consistent with MODULE_CONTRACT.md
  and with the module README at <<README_URL>>.
-->

# <<SKILL_TITLE>>

Use the installed `ambitious-<<MODULE>>` binary at `$AMBITIOUS_HOME/bin/ambitious-<<MODULE>>`. When `AMBITIOUS_HOME` is not set, that path is `/workspace/ambitious/bin/ambitious-<<MODULE>>`. Run `selfcheck --json` at routine start and read `data.problems`. Resolve a reported problem before writing data. Run `commands` to list the operations this binary offers. Read the module documentation at <<README_URL>> for every input shape.

Pass one JSON object on stdin or with `--in`, never both. Keep every input file outside `/home/box/agent-data`. You may pass `--json`; the module accepts it and changes nothing, because output is always JSON. When you need a repeatable result, pass `--now` and `--seed`, and keep the `now`, `seed` and `version` values from `meta` with the evidence.

<<WRITE_RULES>>
<!-- Example, then replace: Always pass an idem_key for a write. Choose one stable key per intended action and keep both the key and the input unchanged for a retry. -->

<<READ_RULES>>
<!-- Example, then replace: Never compute <<KIND_OF_NUMBERS>> by hand. Use the command that returns them, and report the values with the time in meta. -->

Read the envelope and the exit code after every command. Require the expected `module` and `command`, a successful exit, and `ok: true` before using a result. When `ok` is false, relay `errors[].message`, `errors[].hint`, and `data.problems[]` when present, in plain words. Do not invent a successful result, and do not treat stderr logs as results.

The module does not render documents. If the user needs a readable document, write a Grok Bot Page from the JSON, or follow this module's Page skill when this module has one. Never place credentials or private bot files in module input.
