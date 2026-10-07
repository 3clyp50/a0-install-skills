---
name: a0-install-skills
description: Install, configure, start, and verify Agent Zero directly on a Linux, macOS, or Windows machine without Docker or another container runtime. Use for bare-machine Agent Zero setup and native-runtime troubleshooting; container installations belong to a0-install.
---

# Agent Zero native installation

This is a community-made, unofficial skill, not an official Agent Zero installer.

Make Agent Zero usable on the requested machine, with a reachable WebUI and working local tools. Perform the installation, rather than merely returning commands, when the user asks you to install it and you have access to the target.

Keep this skill checkout separate from the Agent Zero checkout. Use the existing framework and its configuration. Let the installing agent adapt the instructions to the actual machine; do not add a custom launcher, patch core files, or recreate the Docker image's entire package set.

## Choose the platform

- Linux or macOS: read [references/unix.md](references/unix.md).
- Windows: read [references/windows.md](references/windows.md) for PowerShell and dependency compatibility. WSL is an explicit alternative, not a silent replacement for native Windows.
- All platforms: read [references/environment.md](references/environment.md) to write the native environment prompt and understand the remaining runtime assumptions.

## Working defaults

1. Inspect the OS, CPU architecture, existing Python/Git/tools, available port, installation directory, and applicable local instructions. Reuse working prerequisites. Default to a fresh user-owned Agent Zero checkout and a dedicated Python environment; preserve existing instances and user data.
2. Use the user's requested Agent Zero revision. Otherwise inspect the official repository's current default branch, choose a compatible revision, and record its commit. Inspect its installation instructions, `helpers/runtime.py`, path handling, and Code Execution configuration. Do not silently substitute a fork or downgrade requirements.
3. Install requirements and requested native services using the relevant platform guide. Respect the current session's authorization and local dependency-installation rules. Explain once that executed code has the host account's permissions. Run the server as the ordinary user and bind to loopback by default.
4. Create or merge `<agent-zero-directory>/usr/prompts/agent.system.main.environment.md` with the real OS, shell, permissions, paths, and Python environment. This replaces the default Kali/container description; it is not a filesystem or RFC configuration change.
5. Start the framework with local execution rather than development RFC forwarding, using the selected revision's existing options as described in the environment reference. Set **Plugins → Code Execution → Shell Interface → Local TTY**, configure the user's model/API credentials through existing settings, and verify the resulting tools. Preserve authentication and CSRF. Never put credentials in this repository, command arguments, or the final report.

Start the core chat, local terminal/Python execution, file editing, and scheduler first. Add browser, web search, Node.js, speech, and document features according to the requested scope and installed prerequisites. Keep the environment prompt accurate so Agent Zero can discover and use the host's capabilities. Do not blanket-disable plugins or preinstall optional dependencies. Report each requested feature as verified, unavailable, or awaiting user configuration.

## Verify before finishing

- Confirm the actual WebUI URL responds and a model turn completes.
- In a fresh chat, ask Agent Zero to identify its OS/account, run a harmless shell command, calculate something with Python, and create/read/edit a disposable file in its configured work directory. Verify that execution occurred on the intended host, without an RFC target.
- Test project file access and one disposable scheduled task if those features are requested. Check logs for failed RFC calls and unresolved `/a0` paths; prompt text cannot repair either.
- Test each requested optional feature. Do not infer Browser success from installing a browser, or search success from a working model.
- Remove only the disposable test artifacts you created. Preserve the user's existing prompts, settings, projects, and services.

## Finish

Return the actual URL, Agent Zero directory and revision, exact launch/stop instructions, verification results, and material limitations. Add a background service only when requested; an agent terminal session is not durable hosting. For updates, back up user state, check the new revision's native runtime assumptions, and repeat verification before switching the running instance.
