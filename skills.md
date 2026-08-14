---
title: Skills & Prompting Guide
layout: default
---

# Skills & Prompting Guide

A practical guide to Claude Code skills — what they do, how to invoke them, and the best prompts to get the most out of each one. Pitched at engineers and architects who want to go beyond the basics.

## Contents

| | |
|---|---|
| [What Are Skills?](#what-are-skills) | How skills differ from chat |
| [Installing the Skills Pack](#installing-the-skills-pack) | One command to get everything |
| [The Skills Catalogue](#the-skills-catalogue) | Each skill with examples |
| [Architect Workflows](#architect-workflows) | Chaining skills for real design work |
| [Built-in Skills](#built-in-skills) | Skills that ship with Claude Code |
| [Writing Your Own Skills](#writing-your-own-skills) | Custom slash commands for your team |

---

## What Are Skills?

A skill is a reusable prompt template bound to a slash command. When you type `/grilling` or `/mattpocock-skills:research`, Claude loads a carefully engineered instruction set designed for that specific type of work — rather than a blank general-purpose chat.

**Why use a skill over just asking?**

| Plain chat | Skill |
|---|---|
| Claude starts from scratch each time | Skill encodes best-practice structure |
| You have to re-explain the mode of working | Skill sets the frame immediately |
| Quality varies with how you phrase it | Skill is consistent and repeatable |
| No shared vocabulary with your team | `/grilling` means the same thing to everyone |

Skills are most valuable for **recurring work patterns**: design reviews, research spikes, debugging sessions, security checks — anything you do more than once.

---

## Installing the Skills Pack

Install the mattpocock skills plugin (covers architecture, design, research, review, and more):

```
/plugin install mattpocock-skills
/reload-plugins
```

You only need to do this once. After reload, all skills are available as slash commands.

**Verify installation:**

```
/skills
```

This opens the skills browser showing everything available with their descriptions.

---

## The Skills Catalogue

### `/mattpocock-skills:grilling` — Stress-Test a Plan

**What it does:** Claude plays devil's advocate, grilling your plan, architecture, or decision with hard questions until the weaknesses surface. It does not let you off easy.

**When to use it:** Before presenting a design, before committing to a vendor, before writing an RFC.

**Invocation pattern:**

```
/mattpocock-skills:grilling

I'm proposing we replace our hub-and-spoke network topology with a full mesh
for our 12 regional offices. Traffic between regions has grown 400% in 18 months
and the hub is becoming a bottleneck. My plan is BGP peering via our cloud provider's
backbone rather than physical MPLS. Grill this.
```

**Example follow-up prompts after initial grilling:**

```
Push harder on the failure modes — what happens if two peering sessions drop simultaneously?
```

```
I haven't addressed latency variance between regions yet. Keep grilling on that.
```

**What good output looks like:** A list of sharp, specific questions targeting your blind spots — not generic advice. Expect challenges on failure modes, cost model assumptions, operational complexity, and rollback plans.

**Pro tip:** Feed it your RFC draft directly. Paste the full document and ask it to grill every section.

---

### `/mattpocock-skills:research` — Investigate a Topic with Sources

**What it does:** Runs a focused research spike against primary sources (official docs, RFCs, vendor documentation), then produces a captured Markdown file in your repo you can reference later.

**When to use it:** Technology evaluation, protocol comparison, vendor shortlisting, understanding a standard you haven't read.

**Invocation pattern:**

```
/mattpocock-skills:research

Research the current state of EVPN-VXLAN as a data centre fabric underlay.
I need to understand: (1) which vendors have mature implementations,
(2) how it compares to traditional STP+VLANs for east-west traffic,
(3) what the operational tooling looks like in 2025.
Output a summary I can share with my team.
```

**Another example — protocol evaluation:**

```
/mattpocock-skills:research

Compare WireGuard vs IPsec IKEv2 for site-to-site VPN between GCP and on-premise.
Focus on: key management, performance at 1Gbps, NAT traversal behaviour,
and Linux kernel support. We're running Debian 12 on both ends.
```

**Pro tip:** Ask for a specific output format if you need it (`output as a comparison table`, `output as a one-page briefing`). The research file gets saved to your project so you can commit and share it.

---

### `/mattpocock-skills:domain-modeling` — Build Shared Vocabulary

**What it does:** Helps you define and sharpen the domain model for your system — the core entities, their relationships, and the ubiquitous language your team should use consistently.

**When to use it:** Onboarding new engineers, writing RFCs, starting a new project, resolving naming conflicts across teams.

**Invocation pattern:**

```
/mattpocock-skills:domain-modeling

We're building a network observability platform. We have inconsistent naming
across teams — some call it a "circuit", some call it a "link", some call it
a "connection". We also have "tenant", "customer", and "org" being used
interchangeably. Help me build a domain model that pins down the right terms
and defines their boundaries.
```

**Another example — formalising existing concepts:**

```
/mattpocock-skills:domain-modeling

In our firewall policy system we have: rules, policies, rule sets, ACLs,
and profiles. Map these to a domain model. Define what each term means,
how they relate, and which terms we should retire to reduce confusion.
```

**What good output looks like:** A glossary of terms with precise definitions, a diagram showing entity relationships, and a list of terms to deprecate. This becomes a living document your team references in PRs and design reviews.

---

### `/mattpocock-skills:diagnosing-bugs` — Structured Fault Finding

**What it does:** Runs you through a disciplined diagnosis loop — forming hypotheses, designing experiments, eliminating causes — rather than jumping straight to guesses.

**When to use it:** Intermittent failures, performance regressions, routing anomalies, any problem where "try things randomly" has already failed.

**Invocation pattern:**

```
/mattpocock-skills:diagnosing-bugs

Intermittent packet loss between vm-connlt1 (10.154.0.2) and vm-connlt3 (10.154.0.4),
both in europe-west2-a. Loss is ~3% on 60-byte ICMP, 0% on 1400-byte. Only happens
between 09:00–11:00 UTC. VMs share a VPC, no firewall rules between them, both on
n2-standard-4. MTU is 1460 (GCP default).
```

**The skill will:**
1. Ask clarifying questions about what you've already tried
2. Form hypotheses ranked by likelihood
3. Suggest specific diagnostic commands
4. Walk you through eliminating each cause

**Pro tip:** Give it everything you already know upfront — what you've tried, what the monitoring shows, what changed recently. The more context, the faster it cuts to the real cause.

---

### `/mattpocock-skills:wizard` — Generate a Guided Setup Script

**What it does:** Produces an interactive bash script that walks a human through steps only they can perform — clicking through a console, entering credentials, approving MFA. The wizard handles the logic; the human handles the human parts.

**When to use it:** Provisioning infrastructure for others, setting up credentials, walking a new team member through a console they've never used, running a one-off migration.

**Invocation pattern:**

```
/mattpocock-skills:wizard

Create a guided wizard for setting up a new engineer on this project.
Steps they need to complete:
1. Accept the GitHub organisation invite
2. Generate a GitHub PAT with repo scope and save it locally
3. SSH into vm-connlt1 and change their default password
4. Clone the cuddly-google repo
5. Install Claude Code and authenticate

For each step: show what they need to do, pause for them to confirm, then verify it worked.
```

**Another example — firewall provisioning:**

```
/mattpocock-skills:wizard

Create a wizard that walks an ops engineer through adding a new office IP range
to our GCP firewall. They need to: get the office external IP, log into GCP console,
find the allow-ssh-connlt rule, add the new CIDR, and test connectivity.
Include verification at each step.
```

---

### `/mattpocock-skills:code-review` — Review Against Standards

**What it does:** Reviews code or infrastructure-as-code changes along two axes simultaneously: **Standards** (does this follow our documented patterns?) and **Spec** (does this do what the issue/ticket asked for?).

**When to use it:** Before opening a PR, reviewing Terraform/Ansible changes, reviewing network config scripts.

**Invocation pattern:**

```
/mattpocock-skills:code-review

Review the changes on this branch against main. We're adding a new
GCP firewall rule for the analytics team. Standards: firewall rules must
use target tags (not target service accounts), CIDR ranges must be /24 or narrower,
and all rules need a description field. Spec: the rule should allow TCP 5432
from the analytics subnet (10.20.0.0/24) only.
```

**Pro tip:** State your standards explicitly in the invocation. If you have a CLAUDE.md or architecture decision records, paste the relevant sections so the review is grounded in your actual standards, not generic best practice.

---

### `/mattpocock-skills:prototype` — Validate a Design Fast

**What it does:** Builds a throwaway proof-of-concept to answer a specific design question — not production code, just enough to see if the idea works.

**When to use it:** Validating a topology before committing, testing a routing approach, checking whether a library does what you think.

**Invocation pattern:**

```
/mattpocock-skills:prototype

I want to validate whether Python's `netmiko` library can reliably parse
the output of `show ip route` across both Cisco IOS and Juniper JunOS.
Build a minimal prototype that connects to a mock device output (hardcoded),
parses the routing table, and outputs a normalised JSON structure.
I just want to know if the parsing approach works — not production quality.
```

---

### `/security-review` — Security Audit Before Merging

**What it does:** Runs a security-focused review of pending changes on the current branch — looking for vulnerabilities, misconfigurations, privilege escalation paths, and exposure risks.

**When to use it:** Before merging any change that touches network config, authentication, firewall rules, or IAM.

**Invocation pattern:**

```
/security-review
```

No additional prompt needed — it reads the current branch diff automatically. But you can add scope:

```
/security-review

Focus on: SSH authentication config, firewall ingress rules, and any hardcoded
credentials or IP ranges. We're running on GCP with a public-facing SSH port.
```

---

## Architect Workflows

These are sequences of skills chained together for common architecture tasks.

### Workflow 1 — Technology Decision

Use this when evaluating two or more options for a technology choice.

```
Step 1:  /mattpocock-skills:research
         Compare [Option A] vs [Option B] for [use case]. Focus on [criteria].

Step 2:  /mattpocock-skills:domain-modeling
         Given the research, what terms and concepts from the chosen technology
         should we adopt as our standard vocabulary?

Step 3:  /mattpocock-skills:grilling
         I've decided to go with [chosen option] because [reasons]. Grill this decision.
```

### Workflow 2 — RFC or Design Document

Use this before writing any significant design document.

```
Step 1:  /mattpocock-skills:grilling
         Here is my proposed design: [paste design]. Grill every section.

Step 2:  Address the weaknesses surfaced by grilling.

Step 3:  /mattpocock-skills:domain-modeling
         Review the terminology in this design. Flag any ambiguous or inconsistent terms.

Step 4:  /security-review  (if the design involves network changes)
```

### Workflow 3 — Incident Investigation

Use this during or after a network incident.

```
Step 1:  /mattpocock-skills:diagnosing-bugs
         [Full incident description: symptoms, timeline, what you've tried]

Step 2:  /mattpocock-skills:research  (if an unfamiliar protocol or behaviour is involved)
         Explain [specific behaviour] in [protocol/system]. Is this expected?

Step 3:  /mattpocock-skills:wizard  (for the remediation steps others need to execute)
         Create a guided runbook for: [remediation steps]
```

### Workflow 4 — Onboarding a New Engineer

```
Step 1:  /mattpocock-skills:domain-modeling
         Explain our network domain model to a new engineer joining the team.
         Cover: [key concepts in your environment]

Step 2:  /mattpocock-skills:wizard
         Create a wizard that walks a new engineer through their first day setup:
         [list of setup steps]
```

---

## Built-in Skills

These ship with Claude Code and don't require plugin installation.

| Skill | Command | Use it for |
|---|---|---|
| **Init** | `/init` | Generate a `CLAUDE.md` for a new project — gives Claude persistent context about your codebase |
| **Review** | `/review` | Review a pull request by number |
| **Security Review** | `/security-review` | Audit pending branch changes for security issues |
| **Update Config** | `/update-config` | Modify `settings.json` — add permissions, configure hooks, set env vars |
| **Simplify** | `/simplify` | Review recently changed code for quality and efficiency |
| **Fewer Prompts** | `/fewer-permission-prompts` | Scan transcripts and auto-allowlist common read-only operations |
| **Schedule** | `/schedule` | Schedule a recurring Claude agent (cron-style) |
| **Loop** | `/loop` | Run a command on a repeating interval |

### `/init` — Start Every New Project With This

The most underused skill. Run it when you start working on any new codebase:

```
/init
```

Claude reads the repo structure and generates a `CLAUDE.md` that tells it how the project works, what conventions to follow, and what to avoid. Every subsequent session in that project benefits from this context automatically.

For a network infrastructure repo:

```
/init

When generating CLAUDE.md, make sure to include:
- Our firewall rule naming conventions
- Which GCP project and zone we're targeting
- The branch protection rules on main
- Where the architecture decision records live
```

---

## Writing Your Own Skills

If you find yourself giving Claude the same framing repeatedly, turn it into a skill.

**Create a skill file:**

```bash
mkdir -p .claude/skills
```

**Example — a network change review skill:**

```bash
cat > .claude/skills/network-change-review.md << 'EOF'
---
name: network-change-review
description: Review a proposed network change against our standards
---

You are reviewing a proposed network infrastructure change.

Check against these standards:
1. All firewall rules must use target tags, not target service accounts
2. Ingress rules must have CIDR ranges of /24 or narrower
3. All rules must have a description field
4. No rules should allow 0.0.0.0/0 except the approved SSH rule (allow-ssh-connlt)
5. Changes must reference a ticket or issue number in the commit message

Output your review as:
- PASS / FAIL / WARN for each standard
- A summary verdict
- Specific line references for any failures
EOF
```

**Invoke it:**

```
/network-change-review

Here is the Terraform for the new analytics team firewall rule: [paste rule]
```

**Share it with your team** by committing the `.claude/skills/` directory to the repo. Anyone who clones it gets the skill automatically.

---

## Quick Reference

| I want to… | Use this skill |
|---|---|
| Stress-test a design decision | `/mattpocock-skills:grilling` |
| Evaluate a technology or protocol | `/mattpocock-skills:research` |
| Define consistent terminology | `/mattpocock-skills:domain-modeling` |
| Diagnose an intermittent fault | `/mattpocock-skills:diagnosing-bugs` |
| Create a runbook for others to follow | `/mattpocock-skills:wizard` |
| Review IaC changes before merging | `/mattpocock-skills:code-review` |
| Check a change for security issues | `/security-review` |
| Bootstrap context for a new project | `/init` |
| Turn a repeated prompt into a command | Write a custom skill in `.claude/skills/` |

---

*This guide covers Claude Code skills as of August 2026. Skills are community-maintained — run `/plugin install mattpocock-skills` to get updates.*
