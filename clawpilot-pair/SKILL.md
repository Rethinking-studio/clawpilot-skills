---
name: clawpilot-pair
description: Use when the user wants to pair PocketClaw, install or upgrade ClawPilot, verify OpenClaw gateway auth, and generate a pairing code. Follow a strict pairing workflow and output the final 8-digit pairing code on its own line when successful.
---

# ClawPilot Pair

Use this skill for PocketClaw pairing and first-time host setup.

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
npm install -g @rethinkingstudio/clawpilot
```

2. Verify OpenClaw config can be found.
- Check common config paths before concluding it is missing.
- If config is missing, report which paths were checked and what file is expected.

3. Verify Gateway auth is usable.
- Check whether config already contains a usable `token` or `password`.
- If config uses environment variable references such as `${OPENCLAW_GATEWAY_TOKEN}`, verify the environment variable is actually present.
- If auth is missing, say exactly what is missing and the most direct fix.

4. Verify the local Gateway is reachable.
- If the service is down or auth still fails, stop and report the blocking step with the next command to run.

5. Only after all checks pass, generate the pairing code:

```bash
clawpilot pair
```

## Output Rules

- If successful, put the final 8-digit pairing code on its own line.
- Keep explanations brief and action-oriented.
- If blocked, say what failed, what was checked, and the next command or config change needed.

## Do Not

- Do not invent ClawPilot flags or unsupported commands.
- Do not skip auth validation and jump straight to pairing.
- Do not return a vague failure such as "configuration error" without naming the missing item.
- Do not modify unrelated OpenClaw settings.
