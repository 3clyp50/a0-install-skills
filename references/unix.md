# Linux and macOS

Use the same Git/Python setup on both systems, adapting native prerequisite installation to the host. Read [environment.md](environment.md) for the prompt override and runtime caveats.

## Prepare the machine

Discover the distribution/macOS version and architecture, installed Git, a compatible Python, and the user's installation directory. Python 3.12 is the known framework baseline in the inspected source; check the selected revision's requirements and wheel availability. Reuse a compatible existing environment when appropriate. On Linux, use the actual distribution's package manager. On macOS, reuse the user's existing Python/package manager rather than installing another manager automatically.

Use a fresh checkout unless the user explicitly wants an existing installation reused. Example Bash commands after choosing a compatible revision and prerequisites:

```bash
a0_root="$HOME/agent-zero-native"
git clone https://github.com/agent0ai/agent-zero.git "$a0_root"
cd "$a0_root"
# Select the requested/verified revision here before installing requirements.
python3.12 -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
python -m pip install ipython
python -m pip check
```

If the directory already exists, inspect it instead of rerunning clone or overwriting files. Capture dependency errors and resolve the actual missing library or wheel. Do not rewrite pinned requirements merely to make installation appear successful.

## Describe the host and start

Discover the environment's Python/IPython paths and the shell used by Code Execution (currently `/bin/bash` on Linux/macOS). Create or merge the prompt at:

```bash
mkdir -p "$a0_root/usr/prompts"
# Write agent.system.main.environment.md using the concrete host facts in environment.md.
```

Start from the checkout with the environment activated and a free, explicitly chosen port:

```bash
cd "$a0_root"
source .venv/bin/activate
python run_ui.py --dockerized=true --host=127.0.0.1 --port=5080
```

Read the actual startup output, open the reported URL, select **Local TTY** in Code Execution plugin settings, and configure the user's model credentials through normal settings. Verify chat, shell, Python, and file access as instructed in `SKILL.md`; resolve any remaining `/a0` assumptions from actual failures.

## Add requested capabilities

For the inspected Browser plugin, the repository's development guide uses:

```bash
PLAYWRIGHT_BROWSERS_PATH="$a0_root/tmp/playwright" python -m patchright install chromium --no-shell
```

Install Chromium's system libraries if the launch reports they are missing. On macOS, check the actual binary-discovery code before proceeding; the inspected framework does not list a macOS binary layout. Do not promise browser support from the download alone.

For web search, inspect `helpers/searxng.py` and set up a native JSON-enabled SearXNG service in its own environment if requested. For other capabilities, inspect their plugin requirements and install the relevant native tools. Reuse host services; do not run the image's `/ins/` package scripts on the host.

Stop a foreground server with Ctrl+C. If the user requests durable background operation, use the host's existing service manager: usually systemd on Linux and launchd on macOS. Set the working directory, environment/PATH, and exact Python executable; verify stop/start and restart behavior without depending on this chat's terminal.
