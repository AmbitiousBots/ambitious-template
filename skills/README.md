# Skills

A skill is a Markdown file the bot follows when it drives this module. Put one skill in its own directory under `skills/`, with `SKILL.md` as the file the bot reads. A skill cannot store companion files for the bot to load; the Ambitious Bot template copies the skill body only.

This file is part of this GitHub template for a new module. It is not the Ambitious Bot template, and it is not the kit's repo-template.

## Naming

Name a skill in this repository without the `ambitious-` prefix, for example `records-for-bots`. If a later kit release copies the skill into the Ambitious Bot template, that process adds the prefix and a `Template version:` line at the top of the body. Do not add either of those here. The copy step is part of pinning the module in the kit; it does not happen for a module the kit has not pinned.

Use this pattern:

| Directory | When to keep it |
| --- | --- |
| `<<MODULE>>-for-bots` | Always. It tells the bot how to call the installed binary. |
| `<<MODULE>>-page` | When the bot should present the module's JSON as a Grok Bot Page. |

Rename the `your-module-*` directories in this GitHub template to match `<<MODULE>>`. Change the `name` in each file's frontmatter to the same directory name.

## Rules

- A skill tells the bot to run the installed binary at `$AMBITIOUS_HOME/bin/ambitious-<<MODULE>>` (on a Grok Bot computer, `/workspace/ambitious/bin/ambitious-<<MODULE>>` when `AMBITIOUS_HOME` is unset).
- Pass JSON on stdin or with `--in`, never both. Keep input files outside `/home/box/agent-data`.
- The bot must read the JSON envelope (`ok`, `schemaVersion`, `module`, `command`, `data`, `errors`, `meta`) and the exit code. It must not invent a successful result, and it must not compute a number the module already returns.
- The module must not render documents. The bot writes Grok Bot Pages; the module may only store a Page reference. A Page skill has to say this.
- Do not put credentials, private bot files, or paths under `/home/box/agent-data` in module input.
- Name the module's documentation by its public address, using the `<<README_URL>>` placeholder in the skill, not by a file path on the bot's computer.
- In the skill body, do not mention another skill by an unprefixed name. If this module has a Page skill, say "this module's Page skill" rather than a skill name. The kit fails its template checks when a copied skill body names a skill without the `ambitious-` prefix, and the body is not rewritten on copy.
- Fill every `<<PLACEHOLDER>>` before you publish a release. A published skill must not still contain a placeholder marker.
