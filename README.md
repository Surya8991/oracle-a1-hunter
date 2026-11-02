# Oracle A1 Hunter 🏹

A GitHub Actions workflow that continuously tries to create an **Oracle Cloud Free Tier A1.Flex** instance (`1 OCPU / 6 GB`, Ubuntu 24.04, 100 GB boot) in **ap-hyderabad-1**, retrying past `Out of host capacity` until it succeeds — **24/7 in GitHub's cloud, no PC required**.

Based on [mohankumarpaluru/oracle-freetier-instance-creation](https://github.com/mohankumarpaluru/oracle-freetier-instance-creation).

## How it works
- The workflow (`.github/workflows/hunt.yml`) runs on a **cron every 5 minutes** (GitHub's minimum) and also supports manual runs.
- Each run rebuilds `oci_config` / `oci.env` from encrypted **repo secrets**, then runs `main.py` under a ~4-minute timeout (≈5 launch attempts per run).
- On success it writes `INSTANCE_CREATED`, **pings Discord**, uploads the details as an artifact, and **disables the schedule** so it stops.

## Required secrets
Set these under **Settings → Secrets and variables → Actions**:

| Secret | What it is |
|---|---|
| `OCI_PRIVATE_KEY` | Contents of your OCI API private key (`.pem`) |
| `OCI_USER` | `ocid1.user.oc1..…` |
| `OCI_FINGERPRINT` | API key fingerprint |
| `OCI_TENANCY` | `ocid1.tenancy.oc1..…` |
| `OCI_REGION` | e.g. `ap-hyderabad-1` |
| `OCI_AD` | Availability domain, e.g. `AD-1` |
| `OCI_SUBNET_ID` | `ocid1.subnet.oc1.…` |
| `SSH_PUBLIC_KEY` | The public key to authorize on the instance |
| `DISCORD_WEBHOOK` | Discord webhook URL for the success alert |

## Controls
- **Pause:** Actions tab → *Oracle A1 Hunter* → `⋯` → Disable workflow.
- **Resume / test now:** Actions tab → *Oracle A1 Hunter* → **Run workflow**.
- **Change size:** edit `ocpus` / `memory_in_gbs` on line ~475 of `main.py`.

> ⚠️ Scheduled runs can be delayed/skipped under GitHub load, and get auto-disabled after 60 days of repo inactivity — just re-enable.
