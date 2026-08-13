# Build Guide — Cuddly-Google Infrastructure

## Overview

Three Debian 12 virtual machines provisioned on Google Compute Engine (GCE) for the `poised-beach-505408-r2` project. Each VM is dedicated to a single user and configured for password-based SSH authentication.

## Infrastructure Specification

| Resource | Value |
|---|---|
| GCP Project | `poised-beach-505408-r2` |
| Region | `europe-west2` (London) |
| Zone | `europe-west2-a` |
| Machine Type | `n2-standard-4` (4 vCPU, 16 GB RAM) |
| OS | Debian 12 (Bookworm) |
| Boot Disk | 20 GB standard persistent disk |
| Network | `default` VPC |

## Virtual Machines

| VM Name | Assigned User | Zone |
|---|---|---|
| `vm-connlt1` | `connlt1` | `europe-west2-a` |
| `vm-connlt2` | `connlt2` | `europe-west2-a` |
| `vm-connlt3` | `connlt3` | `europe-west2-a` |

## Firewall Rules

A firewall rule named `allow-ssh-connlt` was created on the `default` network:

| Setting | Value |
|---|---|
| Direction | INGRESS |
| Protocol / Port | TCP 22 (SSH) |
| Source ranges | `0.0.0.0/0` (any IP) |
| Target tag | `ssh-access` |
| Priority | 1000 |

The tag `ssh-access` is applied to all three VMs so only tagged instances are governed by this rule.

## User Account Setup

After VM creation, run the following on each VM to create the OS user and enable password authentication:

```bash
# vm-connlt1
gcloud compute ssh vm-connlt1 --zone=europe-west2-a --project=poised-beach-505408-r2 --command="
  sudo useradd -m -s /bin/bash connlt1 &&
  echo 'connlt1:AppleBananaCarrot1' | sudo chpasswd &&
  sudo sed -i 's/^#\?PasswordAuthentication.*/PasswordAuthentication yes/' /etc/ssh/sshd_config &&
  sudo systemctl restart sshd
"

# vm-connlt2
gcloud compute ssh vm-connlt2 --zone=europe-west2-a --project=poised-beach-505408-r2 --command="
  sudo useradd -m -s /bin/bash connlt2 &&
  echo 'connlt2:DragonElephantFrog2' | sudo chpasswd &&
  sudo sed -i 's/^#\?PasswordAuthentication.*/PasswordAuthentication yes/' /etc/ssh/sshd_config &&
  sudo systemctl restart sshd
"

# vm-connlt3
gcloud compute ssh vm-connlt3 --zone=europe-west2-a --project=poised-beach-505408-r2 --command="
  sudo useradd -m -s /bin/bash connlt3 &&
  echo 'connlt3:GripHandInsect3' | sudo chpasswd &&
  sudo sed -i 's/^#\?PasswordAuthentication.*/PasswordAuthentication yes/' /etc/ssh/sshd_config &&
  sudo systemctl restart sshd
"
```

## Branching and Deployment Strategy

The `main` branch is the production branch. It serves the GitHub Pages site directly. A direct push to `main` will immediately update the live page, so it is protected.

### Branch Protection Rules (main)

| Rule | Setting |
|---|---|
| Direct pushes | **Blocked** — PRs required |
| Required approvals | **1** — a second person must approve |
| Stale review dismissal | **Enabled** — approvals reset if new commits are pushed |
| Last-push approval | **Enabled** — the person who pushed the final commit cannot self-approve |
| Force pushes | **Blocked** |
| Branch deletion | **Blocked** |
| Enforce for admins | Off — repo owner can bypass in emergencies |

### Working Workflow

No one pushes directly to `main`. All changes follow this flow:

```
main (protected)
  └── your-feature-branch  ← all work happens here
        └── Pull Request → reviewed → merged into main
```

**Step by step:**

```bash
# 1. Always start from the latest main
git checkout main
git pull

# 2. Create a feature branch — name it descriptively
git checkout -b fix/typo-in-part-3
# or
git checkout -b add/docker-section

# 3. Make your changes, then commit
git add .
git commit -m "Fix typo in Part 3 hooks section"

# 4. Push your branch (not main) to GitHub
git push -u origin fix/typo-in-part-3

# 5. Open a Pull Request on GitHub
# Go to the repo → "Compare & pull request" banner → fill in title and description

# 6. Wait for approval, then merge
# GitHub will run the Pages build automatically once merged
```

### Branch Naming Conventions

| Prefix | Use for |
|---|---|
| `fix/` | Bug fixes and corrections |
| `add/` | New sections or features |
| `update/` | Changes to existing content |
| `infra/` | VM, firewall, or GCP changes |

### Modifying the Protection Rules

To change branch protection settings (requires repo admin access):

```bash
# View current rules
gh api repos/funfairlabs-incubator/cuddly-google/branches/main/protection \
  --jq '{force_pushes:.allow_force_pushes.enabled, approvals:.required_pull_request_reviews.required_approving_review_count}'

# Update — e.g. change required approvals to 2
gh api repos/funfairlabs-incubator/cuddly-google/branches/main/protection \
  --method PUT \
  -H "Accept: application/vnd.github+json" \
  --field enforce_admins=false \
  --field allow_force_pushes=false \
  --field allow_deletions=false \
  --field required_conversation_resolution=true \
  --field 'required_status_checks=null' \
  --field 'restrictions=null' \
  --field 'required_pull_request_reviews[required_approving_review_count]=2' \
  --field 'required_pull_request_reviews[dismiss_stale_reviews]=true' \
  --field 'required_pull_request_reviews[require_last_push_approval]=true'
```

Or via the GitHub UI: **Settings → Branches → Edit** next to the `main` rule.

---

## Rebuild / Teardown

To delete all VMs:

```bash
gcloud compute instances delete vm-connlt1 vm-connlt2 vm-connlt3 \
  --zone=europe-west2-a \
  --project=poised-beach-505408-r2
```

To delete the firewall rule:

```bash
gcloud compute firewall-rules delete allow-ssh-connlt \
  --project=poised-beach-505408-r2
```

To recreate VMs from scratch, re-run the `create_instance` calls via the GCE MCP connector or the `gcloud` equivalents:

```bash
for vm in vm-connlt1 vm-connlt2 vm-connlt3; do
  gcloud compute instances create $vm \
    --project=poised-beach-505408-r2 \
    --zone=europe-west2-a \
    --machine-type=n2-standard-4 \
    --image-family=debian-12 \
    --image-project=debian-cloud \
    --tags=ssh-access
done
```
