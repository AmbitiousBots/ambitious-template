# Security policy

This page says how to report a security vulnerability in this repository. It is a reporting process only. It is not a statement about how secure the software is, and it makes no compliance or certification claim.

## How to report a vulnerability

Report it privately. Do not open a public issue, pull request or discussion that describes the problem.

1. Open this repository's **Security** tab on GitHub and choose **Report a vulnerability**. This is GitHub's private vulnerability reporting form; only the maintainers of this repository can read what you send. GitHub describes the form in [Privately reporting a security vulnerability](https://docs.github.com/en/code-security/security-advisories/guidance-on-reporting-and-writing-information-about-vulnerabilities/privately-reporting-a-security-vulnerability).
2. If the form is not offered, open an issue that says only that you have a security report to make, with no details, and a maintainer will arrange a private way to continue.

## What to include

- What the problem is and what someone could do with it.
- The release tag or commit you tested, your operating system, and the output of `node --version` (and `python3 --version` for Python code).
- The exact steps, commands and input that show the problem, and the output you got.
- A fix or a workaround, if you know one.

Leave out real keys, passwords and personal data. Use made-up values that show the same problem.

## What happens next

A maintainer replies in the private report, asks for anything that is missing, and works on a fix with you there. Please keep the details private until a fix is released. The fix is published as a new release, and the report is credited to you unless you ask otherwise.

## What to report elsewhere

A bug that has no security effect goes in a normal issue. A vulnerability in a dependency or tool is best reported to that project first; tell us as well if this repository needs to change because of it.
