# Oracle A1 Hunter — AGENTS.md

> Project-level standards. The workspace root `AGENTS.md` also applies.
> (`CLAUDE.md` is a one-line `@AGENTS.md` pointer.)

## Overview
- **Purpose:** A GitHub Actions workflow that continuously retries creating an **Oracle Cloud
  Free Tier A1.Flex** instance (1 OCPU / 6 GB, Ubuntu 24.04, 100 GB boot) in `ap-hyderabad-1`,
  retrying past "Out of host capacity" until it succeeds — runs 24/7 in GitHub's cloud, no PC needed.
- **Stack:** Python (`main.py`) driven by GitHub Actions (`.github/workflows/hunt.yml`).
- **Status:** Active.
- Based on `mohankumarpaluru/oracle-freetier-instance-creation`.

## How it runs
- Primary mode is **GitHub Actions**: cron every 5 min (+ manual runs). Each run rebuilds
  `oci_config` / `oci.env` from encrypted repo secrets, runs `main.py` (~4-min timeout), and on
  success writes `INSTANCE_CREATED`, pings Discord, uploads an artifact, and disables the schedule.
- Controls: Actions tab → *Oracle A1 Hunter* → Disable / Run workflow.
- Change size: edit `ocpus` / `memory_in_gbs` near line ~475 of `main.py`.

## Secrets (important)
- Stored as **GitHub repo secrets** (Settings → Secrets and variables → Actions):
  `OCI_PRIVATE_KEY`, `OCI_USER`, `OCI_FINGERPRINT`, `OCI_TENANCY`, `OCI_REGION`, `OCI_AD`,
  `OCI_SUBNET_ID`, `SSH_PUBLIC_KEY`, `DISCORD_WEBHOOK`.
- **Never commit** `oci.env`, `oci_config`, or any `.pem` private key.

## Git
- Remote: `Surya8991/oracle-a1-hunter`. Account: **Surya8991**.
- Branch model: `main` ← `development` ← `feature/*` / `fix/*`.

## Related
Local-folder counterpart: `oracle-freetier-instance-creation` (runs in WSL). This repo is the
cloud (GitHub Actions) variant.
