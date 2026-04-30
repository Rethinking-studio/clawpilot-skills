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
- Verify gateway auth is usable.
- Verify the local gateway is reachable.
- If auth or gateway reachability fails, stop and report the blocking step with the next command or config fix.

For `hermes`:
- Verify the `hermes` CLI exists.
- Verify `hermes gateway status` works.
- If the gateway service is not installed, tell the user to run:

```bash
hermes gateway install
hermes gateway start
```

- If Hermes API readiness fails, report the exact blocking step and the next command to run.

For `ccconnect`:
- Install or upgrade cc-connect before pairing:

```bash
npm install -g cc-connect
```

- Verify the `cc-connect` command is available:

```bash
cc-connect --help
```

- Do not pair as `ccconnect` until the command is installed and runnable.
- Let `clawpilot pair --runtime ccconnect` prepare the local management API and bridge configuration.
- If cc-connect cannot be installed or started, stop and report that as the blocking step.

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
- `clawpilot pair --runtime ccconnect` expects `cc-connect` to be installed first, then prepares the local cc-connect management API and bridge configuration.
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
