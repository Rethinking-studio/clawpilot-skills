---
name: clawpilot-pair
description: Use when the user wants to pair PocketClaw, install or upgrade ClawPilot, verify host runtime readiness, and generate a pairing code for OpenClaw, Hermes, or cc-connect Coding Agent hosts. Follow a strict pairing workflow and output the final pairing code on its own line when successful.
---

# ClawPilot Pair

Use this skill for PocketClaw pairing and first-time host setup.
ClawPilot is a multi-runtime host agent and may pair:

- `openclaw`
- `hermes`
- `ccconnect`

## When To Use

Use this skill when the user wants any of the following:

- Pair PocketClaw with the current host
- Install or upgrade `clawpilot`
- Check whether the host is ready for pairing
- Generate a new pairing code

Do not use this skill for normal chat replies, file delivery, or general diagnostics unrelated to pairing.

## Required Workflow

Follow this order exactly. Do not skip checks.

1. Install or upgrade ClawPilot:

```bash
npm install -g @rethinkingstudio/clawpilot@latest
```

2. Determine the target runtime.
- Use `openclaw`, `hermes`, or `ccconnect`.
- If the user does not specify and the host only has one runtime, use that runtime.
- If multiple runtimes are available and the user did not specify, ask which runtime to pair.

3. Run runtime-specific readiness checks before pairing.

For `openclaw`:
- Verify OpenClaw config can be found.
- Resolve and record OpenClaw's local location before pairing:
  - `which openclaw`
  - `openclaw config file`
  - read the local gateway port from the OpenClaw config, then derive `ws://127.0.0.1:<port>` or `ws://localhost:<port>`.
- Verify gateway auth is usable.
- Verify the local gateway is reachable.
- ClawPilot should persist the resolved location in its runtime config during `clawpilot pair --runtime openclaw`. If you manually inspect or repair the config, preserve these fields when known:
  - `openclawGatewayUrl`
  - `openclawConfigPath`
  - `openclawBinPath`
- If auth or gateway reachability fails, stop and report the blocking step with the next command or config fix.

For `hermes`:
- Verify the `hermes` CLI exists.
- Verify `hermes gateway status` works.
- Verify the Hermes Agent Bridge runtime before pairing. Three green connection indicators are not sufficient unless the Python bridge used for real chat turns is also available.
- Resolve and record Hermes's local location before pairing:
  - `which hermes`
  - the active `HERMES_HOME` or `~/.hermes` / active profile directory
  - the active `.env` path
  - the Hermes Agent Python path. Check configured Hermes paths first when present, then:
    - `$(dirname $(which hermes))/python`
    - `~/.local/hermes-agent/venv/bin/python`
    - `~/.hermes/hermes-agent/venv/bin/python`
  - the local API URL, normally `http://127.0.0.1:8642`
- If the gateway service is not installed, tell the user to run:

```bash
hermes gateway install
hermes gateway start
```

- If Hermes API readiness fails, report the exact blocking step and the next command to run.
- If Hermes Agent Bridge readiness fails with `Hermes agent Python not found`, do not edit ClawPilot source code. Repair the user's Hermes installation or configuration:
  - If the configured Python path is wrong, fix the Hermes config or active profile config.
  - If the Python file exists but is not executable, run `chmod +x <python-path>`.
  - If the venv/runtime is missing or imports such as `hermes_cli`, `hermes_state`, or `run_agent` fail, reinstall or repair the Hermes agent runtime.
  - After repair, restart ClawPilot and rerun `clawpilot status`.
- ClawPilot should persist the resolved location in its runtime config during `clawpilot pair --runtime hermes`. If you manually inspect or repair the config, preserve these fields when known:
  - `hermesApiBaseUrl`
  - `hermesApiKey`
  - `hermesHome`
  - `hermesEnvPath`
  - `hermesBinPath`
  - `hermesAgentPythonPath`

For `ccconnect`:
- Install or upgrade cc-connect before pairing:

```bash
npm install -g cc-connect@latest
```

- Verify the `cc-connect` command is available:

```bash
cc-connect --help
```

- Prepare cc-connect config for PocketClaw:

```bash
clawpilot prepare-ccconnect
```

- `clawpilot prepare-ccconnect` writes `~/.cc-connect/config.toml` with a PocketClaw placeholder channel, Management API, and Bridge settings.
- Do not replace the placeholder channel with a dummy platform, and do not ask the user to choose Feishu, Telegram, Discord, WeCom, or Weixin for PocketClaw pairing.
- Make sure at least one project is configured with the intended coding agent and `work_dir`.
- Verify the selected coding agent is installed and runnable before pairing. For example:
  - Claude Code: `claude --version`, then a minimal `claude -p "test"` if safe for the user's environment.
  - Codex: `codex --version`.
  - Gemini: `gemini --version`.
  - If the agent type is unclear, ask the user which coding agent they want cc-connect to launch. Only default to `claudecode` when the user has not chosen another agent.
- Validate `~/.cc-connect/config.toml` before installing the daemon:
  - `[[projects]]` exists.
  - `[projects.agent] type` matches the coding agent the user actually has.
  - `[projects.agent.options] work_dir` exists and is writable.
  - Do not leave `work_dir = "/"`; prefer the user's chosen workspace, `$HOME/Desktop`, or another writable user directory.
  - `[management] enabled = true` with a port and token.
  - `[bridge] enabled = true` with a port and token.
- Install cc-connect as a daemon for stable PocketClaw pairing. Do not start it with `cc-connect &`, shell background jobs, or `terminal(background=true)`, because those processes can die when the parent shell session ends.
- After installing or upgrading cc-connect, or after changing `~/.cc-connect/config.toml`, clear any old daemon and reinstall it with the explicit config path:

```bash
cc-connect daemon stop || true
cc-connect daemon uninstall || true
cc-connect daemon install --config ~/.cc-connect/config.toml
cc-connect daemon restart
cc-connect daemon status
```

- Verify the daemon exposes the Bridge port before pairing:

```bash
lsof -i :9810
```

- If cc-connect crashes or status is unclear, inspect daemon logs instead of guessing:

```bash
cc-connect daemon logs -f
```

- Do not pair as `ccconnect` until `cc-connect` is installed, runnable, configured, daemonized, and the Management API and Bridge settings are present.
- Let `clawpilot pair --runtime ccconnect` validate the local cc-connect Management API and Bridge settings needed by PocketClaw. It must not start cc-connect itself.
- If cc-connect cannot be installed, configured, daemonized, or started, stop and report that as the blocking step.

4. Only after readiness checks pass, generate the pairing code:

```bash
clawpilot pair --runtime openclaw
```

or

```bash
clawpilot pair --runtime hermes
```

or

```bash
clawpilot pair --runtime ccconnect
```

Notes:
- `clawpilot pair --runtime hermes` will prepare the local Hermes API automatically if possible.
- `clawpilot pair --runtime ccconnect` expects `cc-connect` to be installed, configured by `clawpilot prepare-ccconnect` when needed, and running through its daemon first, then validates the local cc-connect Management API and Bridge configuration.
- Use `--code-only` only when the user explicitly wants the code without QR output.

## Output Rules

- If successful, put the final pairing code on its own line.
- Keep explanations brief and action-oriented.
- If blocked, say what failed, what was checked, and the next command or config change needed.

## Do Not

- Do not invent ClawPilot flags or unsupported commands.
- Do not skip runtime readiness checks and jump straight to pairing.
- Do not return a vague failure such as "configuration error" without naming the missing item.
- Do not force OpenClaw-specific checks when the user is pairing Hermes or ccconnect.
- Do not modify unrelated host settings.
