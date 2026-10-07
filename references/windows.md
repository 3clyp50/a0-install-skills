# Windows and PowerShell

Read [environment.md](environment.md). Native Windows means the framework and tools run as Windows processes. WSL is a separate Linux alternative without Docker; offer it only if native dependencies are incompatible and the user accepts that alternative.

## Check native compatibility first

Inspect Windows/CPU architecture, Git, installed Python versions (`py -0p` when available), and the chosen revision's requirements. Check pinned native packages such as FAISS and document-processing dependencies for Windows wheel/build availability. Windows guidance is not a claim that every requirement or plugin has been ported. If installation fails, report the exact incompatible package; do not silently replace it with an unofficial binary, omit it, or switch to WSL.

The inspected code uses `powershell.exe` for Local TTY and `pywinpty` for terminal sessions. PowerShell 7 may be used to install/start the framework, but that does not change the Code Execution shell to `pwsh`.

## Install and start

These examples are PowerShell, with a chosen Python 3.12 installation and prerequisites already available:

```powershell
$a0Root = Join-Path $HOME 'agent-zero-native'
git clone https://github.com/agent0ai/agent-zero.git $a0Root
if ($LASTEXITCODE -ne 0) { throw 'Clone failed; inspect the destination before retrying.' }
Set-Location $a0Root
# Select the requested/verified revision before installing requirements.
py -3.12 -m venv .venv
if ($LASTEXITCODE -ne 0) { throw 'Python environment creation failed.' }
$a0Python = Join-Path $a0Root '.venv\Scripts\python.exe'
& $a0Python -m pip install -r requirements.txt
if ($LASTEXITCODE -ne 0) { throw 'Resolve the dependency failure before starting Agent Zero.' }
& $a0Python -m pip install ipython
if ($LASTEXITCODE -ne 0) { throw 'IPython installation failed.' }
& $a0Python -m pip check
if ($LASTEXITCODE -ne 0) { throw 'Resolve dependency conflicts before continuing.' }
```

Inspect an existing destination rather than cloning over it. Activation is optional when using the exact Python executable; the child shell still needs the environment's `Scripts` directory on PATH so it can find `ipython`:

```powershell
$env:PATH = (Join-Path $a0Root '.venv\Scripts') + [IO.Path]::PathSeparator + $env:PATH
New-Item -ItemType Directory -Force (Join-Path $a0Root 'usr\prompts') | Out-Null
# Write agent.system.main.environment.md using actual Windows paths and host facts.
& $a0Python run_ui.py --dockerized=true --host=127.0.0.1 --port=5080
```

Read the actual startup URL, select **Local TTY**, configure model credentials, and verify the tasks in `SKILL.md`. Use native PowerShell commands for terminal tests; check file creation both in the configured work directory and a project. Investigate `/a0` failures separately from prompt wording.

For requested Browser automation, install the selected revision's Chromium cache using the same Python:

```powershell
$env:PLAYWRIGHT_BROWSERS_PATH = Join-Path $a0Root 'tmp\playwright'
& $a0Python -m patchright install chromium --no-shell
if ($LASTEXITCODE -ne 0) { throw 'Chromium installation failed.' }
```

Verify an actual Browser action. Linux's Xvfb/Xpra desktop viewer and Debian package-install hooks are not native Windows services. Treat SearXNG, speech, and document tooling as separate requested capabilities with their own Windows compatibility checks.

Stop a foreground server with Ctrl+C. Use Task Scheduler or an existing Windows service runner only when the user requests persistent operation, with the exact interpreter, PATH, and working directory. Verify restart using the service runner rather than relying on the container-oriented WebUI Restart action.

## If the user chooses WSL

Use an existing supported Linux distro when available; otherwise follow the user's authorization for installing one. Install Agent Zero inside that distro's Linux filesystem and follow [unix.md](unix.md), without installing Docker. State clearly that commands run in the WSL Linux environment. Verify localhost reachability from Windows and the lifetime of the distro/server before calling the installation persistent.
