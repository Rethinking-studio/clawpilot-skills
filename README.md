# ClawPilot Skills

This repository contains a small ClawPilot skill suite for PocketClaw host operations across OpenClaw, Hermes, and cc-connect Coding Agent runtimes.

## Included Skills

- `clawpilot-pair`
  Install or upgrade ClawPilot, verify runtime readiness, and generate a PocketClaw pairing code for OpenClaw, Hermes, or cc-connect.

- `clawpilot-send`
  Send a local or generated file back to PocketClaw with:

  ```bash
  clawpilot send "/absolute/path/to/file"
  ```

- `clawpilot-doctor`
  Diagnose or repair ClawPilot host issues such as status, logs, restart, update, OpenClaw gateway health, Hermes API health, and cc-connect bridge readiness.

- `clawpilot-config`
  Inspect or minimally fix configuration required for pairing and auth across OpenClaw, Hermes, and cc-connect.

## Layout

- `clawpilot-pair/SKILL.md`
- `clawpilot-send/SKILL.md`
- `clawpilot-doctor/SKILL.md`
- `clawpilot-config/SKILL.md`
