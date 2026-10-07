# Native installation skill repository

## Purpose

Own agent guidance for bare-machine Agent Zero installations.

## Ownership

- `SKILL.md` is the entry point; `references/` owns platform setup and the native environment prompt.
- `README.md` owns public onboarding and the community/unofficial status.

## Local Contracts

- Keep macOS/Linux guidance shared and Windows instructions in PowerShell.
- Prefer existing framework options and prompt overrides over custom launchers or core patches.
- Distinguish environment prompt instructions from actual routing/path configuration.
- Preserve authentication, CSRF, existing prompts, and user data.
- No dependency installations, host services, credentials, or user data belong in this repository.
- Do not claim platform or feature verification that was not performed.
- Never add Ponytail comments to code.

## Work Guidance

Inspect the selected Agent Zero revision's routing, path, plugin, and prompt interfaces when updating guidance. Test in a disposable checkout, not an existing user's installation.

## Verification

Run the skill-creator frontmatter validator and check local reference links. Verify prompt precedence using the framework's actual prompt resolver. Report platform installation as unverified until tested on that system.

## Child DOX Index

No child contracts.
