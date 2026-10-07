# a0-install-skills

An agent skill for setting up Agent Zero directly on a machine, without Docker or another container runtime.

**Community-made and unofficial.** This is a personal community project, not an official Agent Zero installer or a guarantee of native platform support.

## Use it

Clone this repository on the target machine:

```bash
git clone https://github.com/3clyp50/a0-install-skills.git
```

Ask your coding agent:

> Read `a0-install-skills/SKILL.md` and use it to install, configure, and verify Agent Zero on this machine without a container runtime.

The skill includes shared Linux/macOS guidance, separate PowerShell guidance for Windows, and an environment prompt override so Agent Zero can understand the actual host. It uses existing framework options and lets the installing agent adapt to the machine.

Start with [SKILL.md](SKILL.md). Platform details live in [references/unix.md](references/unix.md) and [references/windows.md](references/windows.md); prompt configuration and runtime caveats live in [references/environment.md](references/environment.md).

## Status

Native setup currently relies on framework workarounds and requires verification of paths, dependencies, and requested tools. Prompt resolution has been checked against the framework; complete installations across Linux, macOS, and Windows have not been certified. Native commands run with the host account's permissions.

Feedback and reports from real installations are welcome through GitHub issues.
