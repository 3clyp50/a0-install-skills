# Native environment and runtime

## The prompt override

The correct global override is **`<Agent Zero checkout>/usr/prompts/agent.system.main.environment.md`**. In a checkout actually located at `/a0`, that is `/a0/usr/prompts/agent.system.main.environment.md`. A native checkout elsewhere uses its own `usr/prompts/` directory; do not create a host `/a0` directory merely to store the prompt.

`prompts/agent.system.main.md` includes this exact filename. `Agent.read_prompt()` obtains the search paths from `helpers/subagents.py:get_paths()`, and `helpers/files.py:read_prompt_file()` resolves the included file through the same paths. Global `usr/prompts/` precedes the core `prompts/` directory. Project and agent-profile prompt overrides have higher priority; inspect them if the active agent still reports Kali or Docker.

Write the file with concrete, discovered values, using this as a starting point:

```markdown
## Environment
Agent Zero runs directly on [actual OS and version], without a container.
Host architecture: [actual architecture].
Host account: [actual account and privileges]. Commands run with this account's permissions.
Terminal shell: [actual shell used by Code Execution]. Use its syntax and this OS's package manager.
Agent Zero directory: [actual absolute checkout path].
Work directory: [actual configured work directory].
Framework Python: [actual environment's Python executable].
Python code execution: [actual IPython executable/environment].
Use native paths. Do not assume Kali, root access, /a0, or /opt/venv.
Inspect installed tools before using them and adapt commands to this machine.
Keep task-only dependencies in a task environment when possible.
[List any installed optional services or known unavailable capabilities relevant to this installation.]
```

Replace all bracketed text; do not leave the template as the installed prompt. Preserve useful existing custom instructions by merging them. The file replaces the core environment section, so do not include the original Kali/container section again. Start a fresh chat for verification; existing history may contain the previous environment description.

## Local routing is a separate concern

In the framework inspected for this skill, `runtime.is_dockerized()` tests the `--dockerized` argument, and `is_development()` is its inverse. `call_development_function()` forwards through RFC in development mode and invokes locally otherwise. The existing workaround is:

```bash
python run_ui.py --dockerized=true --host=127.0.0.1 --port=5080
```

This starts a normal host Python process; it does not create or contact a Docker runtime. The flag selects the existing local execution branch. It is a workaround, not a supported native-mode flag: inspect the selected revision before using it. Do not use `--development=false` or `--dockerized=false` as substitutes; these argument parsers do not necessarily interpret the string `false` as false.

Set **Plugins → Code Execution → Shell Interface → Local TTY** explicitly, even if Auto currently selects local execution. An OS environment prompt alone does not change SSH/RFC routing.

Some code still emits `/a0/...` paths through `helpers/files.py:normalize_a0_path()`, including project and code-execution paths. Verify actual file access. If an operation fails, diagnose its path and choose the smallest compatible solution for that host; do not create a global alias over an existing directory or patch core code preemptively. The prompt does not remap filesystem paths.

The flag also enables container-assumed behavior: the WebUI Restart action exits expecting a supervisor, and root-password settings invoke the host's `chpasswd`. Use process-level restart and leave container root-password/self-update controls alone. For a persistent instance, configure the platform's process supervisor when requested.

## Optional capabilities

- Python execution invokes `ipython`; install it in the environment visible to the shell and check its executable path. JavaScript execution invokes `node`.
- Browser uses Patchright Chromium. Install it in the cache expected by that revision and verify a real browser action. The inspected binary finder lists Linux and Windows layouts; check macOS discovery before claiming support. Linux's embedded interactive view also requires Xvfb/Xpra; headless automation and the interactive view are separate capabilities.
- Built-in search expects a JSON-enabled SearXNG service at `http://localhost:55510/search` in the inspected source. Start a native service or configure an available alternative supported by the selected revision. Verify a query, rather than relying on the prompt to supply the missing service.
- Speech, OCR, document conversion, and desktop plugins may require FFmpeg, eSpeak, Tesseract, Poppler, LibreOffice, or Linux desktop services. Install only what the requested features need; let the agent inspect the actual plugin requirements and host tooling.

The framework contracts above were inspected in Agent Zero checkout revision `e3269ed6d151cd7bf46b45ef77dd9ff3c2fdf072`. Recheck them when choosing another revision. Prompt resolution is verified separately; this reference does not certify a full native installation on any platform.
