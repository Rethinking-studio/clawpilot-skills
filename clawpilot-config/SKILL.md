---
name: clawpilot-config
description: Use when the user needs help locating, validating, or minimally fixing ClawPilot or OpenClaw gateway configuration required for pairing, auth, or host-side operations. Explain exactly what is missing and the most direct fix.
---

# ClawPilot Config

Use this skill for ClawPilot and OpenClaw configuration inspection and minimal correction.

## When To Use

Use this skill when the user needs to:

- Find the relevant OpenClaw config file
- Validate gateway token or password settings
- Validate referenced environment variables
- Understand why pairing or host-side operations cannot authenticate

Do not use this skill for file delivery or general diagnostics when configuration is not the actual issue.

## Workflow

1. Locate the relevant config file.
2. Inspect auth-related fields only.
3. If auth is provided via environment variable references, verify the referenced variables exist.
4. Explain exactly what is missing or misconfigured.
5. If the fix is obvious and low-risk, propose the minimal change.

## Output Rules

- Name the exact missing field, file, or environment variable.
- Prefer the shortest direct fix.
- If multiple fixes are possible, recommend one and explain why.

## Do Not

- Do not rewrite unrelated config sections.
- Do not say "config is wrong" without naming the specific field or variable.
- Do not proceed as if auth is valid when it is only referenced but not actually present in the environment.
