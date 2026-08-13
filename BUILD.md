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
