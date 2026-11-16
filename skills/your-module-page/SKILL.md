---
name: <<MODULE>>-page
description: Present <<MODULE_TITLE>> output in a Grok Bot Page. Use when a user wants a readable <<PAGE_KIND>>.
---

<!--
  FILL IN every <<PLACEHOLDER>> in this file, then rename this directory
  to <<MODULE>>-page. Delete this comment before you publish a release.
  Keep this skill only if the bot should present this module's JSON as a
  Grok Bot Page. If the module has no Page view, delete this directory.
-->

# Present <<MODULE_TITLE>> data as a Page

The bot renders the Grok Bot Page. The `<<MODULE>>` module returns JSON and does not render, publish or modify documents.

1. Run <<COMMANDS_TO_LOAD_THE_VIEW>> for the requested scope, passing the same `--now` and `--seed` to every command in that set. Check every envelope before rendering anything.
2. Render <<SECTION_RULES>>. Show identifiers the module returned. Do not recompute, reorder or infer values the module already provides.
3. State the scope and the time from `meta`, so a reader knows what the Page covers and how fresh it is.
4. After an authorized change, rerun the commands and refresh the Page from the new responses. A Page edit alone never changes stored module state.

If a command fails, report the error and its hint. Do not publish stale data as current. Follow the user's existing Page sharing and publishing permissions.
