# ClawPilot Skills

This repository contains a small ClawPilot skill suite for PocketClaw and OpenClaw host operations.

## Included Skills

- `clawpilot-pair`
  Install or upgrade ClawPilot, verify OpenClaw gateway auth, and generate a PocketClaw pairing code.

- `clawpilot-send`
  Send a local or generated file back to PocketClaw with:

  ```bash
  clawpilot send "/absolute/path/to/file"
  ```

- `clawpilot-doctor`
  Diagnose or repair ClawPilot / OpenClaw host issues such as status, logs, restart, update, and self-repair.

- `clawpilot-config`
  Inspect or minimally fix configuration required for pairing and auth.

## Layout

- `clawpilot-pair/SKILL.md`
- `clawpilot-send/SKILL.md`
- `clawpilot-doctor/SKILL.md`
- `clawpilot-config/SKILL.md`
