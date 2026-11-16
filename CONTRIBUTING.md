# Contributing

Thank you for helping. This repository is a GitHub template for an [Ambitious](https://ambitiousbots.com) module: an open-source, single-purpose command-line tool with no user interface. A bot runs it, passes JSON in and reads JSON out. The rules every module follows are in [MODULE_CONTRACT.md](MODULE_CONTRACT.md), a copy of the contract in the [ambitious-kit](https://github.com/AmbitiousBots/ambitious-kit) repository. In that copy, "this repository" means ambitious-kit. [AGENTS.md](AGENTS.md) describes how to start a module from this GitHub template and how a module repository is laid out.

This GitHub template is not the Ambitious Bot template, which ships as `ambitious-template-<version>.tgz` on a kit release.

Everyone taking part follows the [code of conduct](CODE_OF_CONDUCT.md).

## File an issue

- A bug: open an issue with the bug report form. Give the exact command, its input, everything it printed, and the versions the form asks for.
- An idea: open an issue with the feature request form before you write much code, so the approach can be agreed first.
- A security vulnerability: do not open an issue. Follow [SECURITY.md](SECURITY.md).
- A change to the module contract itself: open the issue or pull request on [ambitious-kit](https://github.com/AmbitiousBots/ambitious-kit), which is the source of the contract.

Replace real keys, passwords and personal data with made-up values in anything you post.

## Set up a checkout

You need two tools:

- [git](https://git-scm.com).
- [mise](https://mise.jdx.dev), a tool-version manager. It installs the exact Node, Python and Java versions pinned in `.mise.toml`, so you do not install those yourself. Node runs TypeScript modules and the checks, Python is for modules written in Python, and Java runs TLC, the TLA+ model checker.

Then, in your clone:

```text
mise trust && mise install
```

This GitHub template ships with no module code and no `package-lock.json`. After you adopt a kit release, as [README.md](README.md) describes, run `npm ci` to install the packages pinned in `package-lock.json`.

If you created the repository from this GitHub template, enable [private vulnerability reporting](https://docs.github.com/en/code-security/security-advisories/working-with-repository-security-advisories/configuring-private-vulnerability-reporting-for-a-repository) so [SECURITY.md](SECURITY.md) has a private form to point at. Do not mark a module repository as a GitHub template unless you intend it to be one.

## Run the checks

This GitHub template is documentation, skills and standard files. Before you change those, run:

```text
npx markdownlint-cli '**/*.md' --config .markdownlint.json
npx markdown-link-check --config .markdown-link-check.json README.md AGENTS.md CONTRIBUTING.md CHANGELOG.md SECURITY.md skills/README.md skills/*/SKILL.md
```

`MODULE_CONTRACT.md` and `CODE_OF_CONDUCT.md` are verbatim copies from ambitious-kit and are listed in `.markdownlintignore`. A placeholder such as `<<MODULE>>` is a fill-in marker, not a broken link.

After you adopt the kit and add module code, run every check before you push. `package.json` lists them under `scripts`. A module has these from its first kit-adopted commit:

| Command | What it checks |
| --- | --- |
| `npm run lint` | The lint rules, and that no tracked file contains an internal plan ID or a path on one person's machine. |
| `npm run tlc` | Every TLA+ model in `spec/` passes the TLC model checker, and every deliberately broken copy in `spec/broken/` breaks the rule listed for it. |
| `npm run verify` | `verify/run`: every feature in `verify/features.json`, each against a fresh temporary `AMBITIOUS_HOME` (the directory a module keeps its data in). |

Also run `typecheck`, `depcruise`, `test`, `build` and `contract` with `npm run <name>` when `package.json` has them. A check that fails means the change is not ready. Fix the cause; never weaken or skip the check.

## Open a pull request

1. Fork the repository, or create a branch if you have write access. Keep one purpose per pull request.
2. Make the smallest change that meets the goal. When behavior changes, change the tests with it, and add or update the entry in `verify/features.json` for anything a user can see.
3. Write commit messages and the pull request in plain language. Everything in this repository is public, so write nothing that only makes sense inside one organisation: no private ticket numbers, no paths on your machine, no names of people or customers.
4. Fill in every section of the pull request template: why, what changed, evidence, and the decisions you made on your own. For a change that is only documentation or skills, say so under evidence and paste the output of the Markdown and link checks above. After the module has `verify/run`, paste that output.
5. A maintainer reviews the pull request and merges it once the checks pass.

## License

This repository is licensed under the Apache License, Version 2.0; see [LICENSE](LICENSE). As section 5 of the license says, anything you submit for inclusion is under the same license, with no additional terms.
