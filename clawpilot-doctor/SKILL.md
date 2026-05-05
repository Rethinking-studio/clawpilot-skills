---
name: clawpilot-doctor
description: Use when the user wants to diagnose or repair ClawPilot host issues for OpenClaw, Hermes, or cc-connect Coding Agent hosts, including status checks, logs, restart, update, and self-repair. Focus on the concrete blocking issue and the next corrective action.
---

# ClawPilot Doctor

Use this skill for ClawPilot host troubleshooting across supported runtimes:

- `openclaw`
- `hermes`
- `ccconnect`

## When To Use

Use this skill when the user asks to:

- Check ClawPilot or Gateway status
- Read logs
- Restart the Gateway
- Update OpenClaw
- Run repair or diagnostics
- Diagnose Hermes API or cc-connect bridge readiness

Do not use this skill to generate a pairing code or send files back to PocketClaw.

## Preferred Actions

Use the smallest action that answers the question or fixes the issue:

- `clawpilot status`
- `clawpilot restart`
- Host commands already exposed through the current OpenClaw or relay setup
- Confirmed repair commands such as OpenClaw self-repair paths

When the host has multiple runtimes, evaluate them separately:

- `OpenClaw`
  - relay
  - local gateway
- `Hermes`
  - relay
  - Hermes API
- `ccconnect`
  - relay
  - cc-connect Management API
  - cc-connect Bridge WebSocket

For Hermes-specific failures, prefer checking:

```bash
curl http://127.0.0.1:8642/health
```

and the current `~/.hermes/.env` values for:

- `API_SERVER_ENABLED`
- `API_SERVER_KEY`

For ccconnect-specific failures, prefer checking:

```bash
command -v cc-connect
cc-connect --help
cc-connect daemon status
clawpilot status
```

Then inspect both the cc-connect app config and the current ClawPilot runtime config:

- `~/.cc-connect/config.toml`
  - configured `[[projects]]`
  - `[projects.agent]` type and `work_dir`
  - `[projects.agent.options] work_dir` exists and is writable
  - `work_dir` is not `/`
  - `[management]` enabled/port/token
  - `[bridge]` enabled/port/token
- `~/.clawai/runtimes/ccconnect.json`
- management API URL/token
- bridge URL/token
- selected coding agent command availability, such as `claude`, `codex`, or `gemini`

If `cc-connect` is missing, install it before retrying pairing:

```bash
npm install -g cc-connect@latest
```

If cc-connect is installed but not configured, use the official AI-agent guide before pairing:

```text
Follow https://raw.githubusercontent.com/chenhg5/cc-connect/refs/heads/main/INSTALL.md to install and configure cc-connect.
```

For PocketClaw ccconnect pairing, treat the cc-connect daemon as the stable runtime path. The official guide configures cc-connect but does not install the daemon automatically. Do not recommend `cc-connect &`, shell background jobs, or `terminal(background=true)` as a stable fix.

After installing/upgrading cc-connect or changing `~/.cc-connect/config.toml`, clear the old service and reinstall it with the explicit config path:

```bash
cc-connect daemon stop || true
cc-connect daemon uninstall || true
cc-connect daemon install --config ~/.cc-connect/config.toml
cc-connect daemon status
```

If the daemon fails, inspect logs before guessing:

```bash
cc-connect daemon logs -f
```

If the Bridge is expected on the default port, verify it is listening:

```bash
lsof -i :9810
```

## Output Rules

- Report what you checked.
- Report what failed or passed.
- If blocked, give the next command to run.
- Keep the result concrete and operational, not theoretical.

## Do Not

- Do not jump into pairing unless the user explicitly wants pairing.
- Do not modify unrelated config when a status or log check is enough.
- Do not claim a repair is complete without verifying the result.
