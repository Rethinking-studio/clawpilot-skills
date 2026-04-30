---
name: clawpilot-config
description: Use when the user needs help locating, validating, or minimally fixing ClawPilot host configuration required for pairing, auth, or host-side operations for OpenClaw, Hermes, or cc-connect Coding Agent hosts. Explain exactly what is missing and the most direct fix.
---

# ClawPilot Config

Use this skill for ClawPilot host configuration inspection and minimal correction.
Support:

- OpenClaw host config
- Hermes local API config
- cc-connect runtime config

## When To Use

Use this skill when the user needs to:

- Find the relevant OpenClaw config file
- Validate gateway token or password settings
- Validate referenced environment variables
- Understand why pairing or host-side operations cannot authenticate
- Validate Hermes API server settings in `~/.hermes/.env`
- Check `API_SERVER_ENABLED` / `API_SERVER_KEY`
- Validate cc-connect runtime settings in `~/.clawai/runtimes/ccconnect.json`
- Check whether `cc-connect` is installed and runnable

Do not use this skill for file delivery or general diagnostics when configuration is not the actual issue.

## Workflow

1. Identify the runtime in question: `openclaw`, `hermes`, or `ccconnect`.
2. Locate the relevant config file.
3. Inspect only the fields relevant to the failure.
4. If auth is provided via environment variable references, verify the referenced variables exist.
5. Explain exactly what is missing or misconfigured.
6. If the fix is obvious and low-risk, propose the minimal change.

Runtime-specific scope:

For `openclaw`:
- OpenClaw config path
- gateway token/password
- referenced env vars

For `hermes`:
- `~/.hermes/.env`
- `API_SERVER_ENABLED`
- `API_SERVER_KEY`
- local API base URL assumptions

For `ccconnect`:
- `~/.clawai/runtimes/ccconnect.json`
- `ccconnectManagementUrl`
- `ccconnectManagementToken`
- `ccconnectBridgeUrl`
- `ccconnectBridgeToken`
- `cc-connect` command availability

## Output Rules

- Name the exact missing field, file, or environment variable.
- Prefer the shortest direct fix.
- If multiple fixes are possible, recommend one and explain why.

## Do Not

- Do not rewrite unrelated config sections.
- Do not say "config is wrong" without naming the specific field or variable.
- Do not proceed as if auth is valid when it is only referenced but not actually present in the environment.
