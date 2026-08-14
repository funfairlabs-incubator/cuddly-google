# Consumer Guide

## Contents

**Getting set up — choose your device**

| | |
|---|---|
| [macOS](#part-1a--macos-local-setup) | Xcode CLT, Homebrew, Apple Silicon notes |
| [Windows (WSL2)](#part-1b--windows-local-setup-wsl2) | Install WSL2, create a Linux user account |
| [Linux](#part-1c--linux-local-setup) | Minimal — just install curl, git, gnupg |
| [Test VM (SSH)](#part-1d--connecting-to-the-test-vm) | Desktop and mobile SSH connection |
| [Languages & Runtimes](#part-1e--installing-languages-and-runtimes) | Node.js via nvm, Python, virtual environments |

**Core workflow — same for everyone**

| | |
|---|---|
| [Setting Up Claude Code](#part-2--setting-up-claude-code) | Install, authenticate, first session |
| [Hooks](#hooks) | Auto-run scripts at lifecycle events |
| [Skills](#skills) | Custom `/slash-commands` |
| [Plugins](#plugins) | Installable bundles from a marketplace |
| [MCP Servers](#mcp-servers) | Connect Claude to GitHub, Slack, databases… |
| [GitHub & Branching](#part-4--using-github) | PRs, branch protection, daily workflow |

**Reference**

| | |
|---|---|
| [Skills & Prompting Guide](skills) | Skills catalogue, examples, and architect workflows |
| [Troubleshooting](#troubleshooting) | Common errors and fixes |

---

## Before You Start — Choose Your Setup

Everything in this guide ends up in the same place: a Linux terminal with Claude Code and Git installed, connected to GitHub. The only difference is *where* that terminal lives.

| I am working on… | Go to… |
|---|---|
| My own **Mac** | [Part 1A — macOS Local Setup](#part-1a--macos-local-setup) |
| My own **Windows PC** | [Part 1B — Windows Local Setup (WSL2)](#part-1b--windows-local-setup-wsl2) |
| My own **Linux machine** | [Part 1C — Linux Local Setup](#part-1c--linux-local-setup) |
| The **shared test VM** provided to me | [Part 1D — Connecting to the Test VM](#part-1d--connecting-to-the-test-vm) |
| I need **Node.js, npm, or Python** | [Part 1E — Languages and Runtimes](#part-1e--installing-languages-and-runtimes) |

> Parts 2, 3, and 4 are shared — follow them whichever path you took.
>
> **Part 2** sets up Claude Code · **Part 3** covers hooks, skills, plugins, and MCP servers · **Part 4** covers GitHub

---

## Part 1A — macOS Local Setup

### Do I Need Homebrew?

**Not immediately.** macOS already includes enough to connect to a VM and run a terminal. But for installing developer tools locally, Homebrew is the standard way to do it on a Mac and you will likely want it.

Here is what you need and where it comes from:

| Tool | How to get it | Required? |
|---|---|---|
| SSH | Built into macOS | Yes — already there |
| Git | Xcode Command Line Tools | Yes — see Step 1 |
| Homebrew | Install script (Step 2) | Recommended |
| Node.js / other tools | Via Homebrew | Only if needed later |

### Step 1 — Xcode Command Line Tools

This installs Git, Make, and other essential developer tools that Apple bundles separately from macOS.

1. Open **Terminal** (`Cmd + Space`, type `Terminal`, press Enter)

2. Run:

   ```bash
   xcode-select --install
   ```

3. A dialog will appear asking to install the Command Line Developer Tools — click **Install**

4. Wait for the download and installation to complete (this can take 5–15 minutes depending on your connection)

5. Verify it worked:

   ```bash
   git --version
   xcode-select -p
   ```

   `git --version` should print something like `git version 2.x.x`. `xcode-select -p` should print `/Library/Developer/CommandLineTools`.

### Step 2 — Homebrew (Recommended)

Homebrew is a package manager for macOS. It lets you install command-line tools with a single command (e.g. `brew install node`). It is not required to follow this guide, but most developers end up installing it.

1. In Terminal, paste this and press Enter:

   ```bash
   /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
   ```

2. When prompted for your Mac login password, enter it and press Enter — nothing appears on screen as you type, this is normal

3. Follow the on-screen instructions. The install takes a few minutes.

4. **Apple Silicon Macs only (M1 / M2 / M3 / M4)** — after the installer finishes, run these two lines to add Homebrew to your PATH:

   ```bash
   echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.zprofile
   eval "$(/opt/homebrew/bin/brew shellenv)"
   ```

   Intel Macs do not need this step — Homebrew installs to `/usr/local` which is already in the PATH.

5. Verify:

   ```bash
   brew --version
   ```

   Should print something like `Homebrew 4.x.x`.

### Step 3 — Verify Your Terminal Is Ready

```bash
git --version
ssh -V
```

Both should print version numbers. If either says `command not found`, go back and repeat Step 1.

You are now ready — continue to [Part 2 — Setting Up Claude Code](#part-2--setting-up-claude-code).

---

## Part 1B — Windows Local Setup (WSL2)

Claude Code runs on Linux. On Windows, the standard approach is to install **WSL2** (Windows Subsystem for Linux), which gives you a full Linux environment running inside Windows — no virtual machine, no dual boot, no separate computer needed.

> **WSL2 requires Windows 10 version 2004 or later, or Windows 11.** To check: press `Win + R`, type `winver`, press Enter. You will see your version number.

### Step 1 — Install WSL2

1. Click the **Start** menu, search for **PowerShell**, right-click it, and select **Run as administrator**

2. In the PowerShell window, run:

   ```powershell
   wsl --install
   ```

   This installs WSL2 and Ubuntu (the default Linux distribution) in one step.

3. When it finishes, **restart your computer**

### Step 2 — Set Up Your Linux User Account

1. After restarting, **Ubuntu** will open automatically (if it does not, search for `Ubuntu` in the Start menu and open it)

2. Wait for the first-time setup to complete — it will say `Installing, this may take a few minutes...`

3. You will be prompted to create a username — choose a simple lowercase name with no spaces (e.g. `tobias`)

4. You will be prompted for a password — enter one and confirm it. Nothing appears on screen as you type — this is normal. **This is your Linux password, separate from your Windows login.**

5. You will see a Linux prompt:

   ```
   tobias@DESKTOP-XXXXX:~$
   ```

   You are now inside a Linux environment on your Windows machine.

### Step 3 — Install Dependencies

Inside the Ubuntu terminal, run:

```bash
sudo apt update
sudo apt install -y curl git gnupg
```

When prompted for your password, enter the one you just created.

Verify:

```bash
git --version
curl --version
```

Both should print version numbers.

### Step 4 — Opening WSL in Future Sessions

- Search for **Ubuntu** (or **WSL**) in the Start menu
- Or open **Windows Terminal** (install it from the Microsoft Store if you don't have it) and click the dropdown arrow next to the `+` tab button — Ubuntu will appear as an option

You are now ready — continue to [Part 2 — Setting Up Claude Code](#part-2--setting-up-claude-code).

---

## Part 1C — Linux Local Setup

If you are already on a Linux machine (Ubuntu, Debian, or similar), you need very little setup.

### Step 1 — Install Dependencies

```bash
sudo apt update
sudo apt install -y curl git gnupg
```

Verify:

```bash
git --version
curl --version
```

Both should print version numbers.

You are now ready — continue to [Part 2 — Setting Up Claude Code](#part-2--setting-up-claude-code).

---

## Part 1D — Connecting to the Test VM

This section is for users who have been given access to one of the shared VMs rather than setting up locally.

<div class="mermaid">
flowchart LR
    subgraph DEV["Your Device"]
        D1["💻 Desktop\nWindows · Mac · Linux"]
        D2["📱 Mobile\niOS · Android"]
    end

    NET["🌐 Internet\nport 22 — SSH"]

    subgraph GCP["☁️ Google Cloud — europe-west2-a"]
        FW["🔒 Firewall\nallow-ssh-connlt\ntcp:22 open"]
        subgraph VMS["Virtual Machines — one per user"]
            V1["vm-connlt1\nuser: connlt1"]
            V2["vm-connlt2\nuser: connlt2"]
            V3["vm-connlt3\nuser: connlt3"]
        end
    end

    D1 -->|"ssh connlt1@IP\nor PuTTY"| NET
    D2 -->|"Termius · Blink\nConnectBot"| NET
    NET --> FW
    FW -->|"your VM"| V1
    FW -.->|"other users"| V2
    FW -.->|"other users"| V3

    style FW fill:#ffebee,stroke:#e53935
    style V1 fill:#e8f5e9,stroke:#43a047,color:#1b5e20
    style V2 fill:#e8f5e9,stroke:#43a047,color:#1b5e20
    style V3 fill:#e8f5e9,stroke:#43a047,color:#1b5e20
</div>

### Your Credentials

| User | Username | VM |
|---|---|---|
| Person 1 | `connlt1` | `vm-connlt1` |
| Person 2 | `connlt2` | `vm-connlt2` |
| Person 3 | `connlt3` | `vm-connlt3` |

> Passwords are distributed separately. Do not share them.

You also need the **external IP address** of your VM — ask your administrator or find it in the [Google Cloud Console](https://console.cloud.google.com/compute/instances?project=poised-beach-505408-r2).

---

### Connecting from a Desktop or Laptop

#### Windows (PowerShell or Terminal)

```powershell
ssh connlt1@<YOUR_VM_EXTERNAL_IP>
```

Replace `connlt1` with your username and `<YOUR_VM_EXTERNAL_IP>` with the IP your administrator gave you. When prompted `Are you sure you want to continue connecting?` type `yes` and press Enter. Then enter your password.

#### macOS / Linux (Terminal)

```bash
ssh connlt1@<YOUR_VM_EXTERNAL_IP>
```

When prompted `Are you sure you want to continue connecting?` type `yes` and press Enter. Then enter your password.

#### PuTTY (Windows GUI)

1. Open PuTTY
2. In the **Host Name** field enter your VM's external IP address
3. Ensure **Port** is set to `22` and **Connection type** is `SSH`
4. Click **Open**
5. If a security alert appears, click **Accept**
6. At the `login as:` prompt type your username and press Enter
7. At the `password:` prompt type your password and press Enter (nothing will appear as you type — this is normal)

---

### Connecting from iOS or Android

A physical Bluetooth keyboard is strongly recommended for serious work — on-screen keyboards lack keys (Tab, Escape, Ctrl) that terminals depend on.

#### iOS / iPadOS

**Option A — Termius (Recommended)**

Cost: Free to start. Pro subscription for advanced features (~$10/month billed annually).
Install: Search **Termius** in the App Store. Developer: Termius Corporation.

1. Open Termius → tap **Hosts** → tap **+**
2. Fill in: Alias (any label), Hostname (VM IP), Port `22`, Username, Password
3. Tap **Save** then tap the host to connect
4. Tap **Continue** if a fingerprint warning appears — normal on first connection

Termius adds a key row above the keyboard with `Tab`, `Ctrl`, `Esc`, and arrows — no external keyboard needed for basic use.

---

**Option B — Blink Shell (Power Users)**

Cost: Blink+ subscription ($19.99/year, 14-day free trial).
Install: Search **Blink Shell** in the App Store.

1. Type `config` at the prompt (or press `Cmd+,` with an external keyboard) → tap **Hosts** → tap **+**
2. Fill in: Host Name (short alias, no spaces), Hostname (VM IP), Port `22`, Username, Password
3. Tap **Save**
4. At the terminal, type `ssh myvm` (your alias) to connect

Best used with a physical Bluetooth keyboard. Go to **Config > Keyboard** to map Caps Lock to Escape.

---

**Option C — ShellFish (SSH Files)**

Cost: Free with ads. Pro removes ads (~$14.99/year or $29.99 lifetime).
Install: Search **SSH Files** in the App Store. Developer: Anders Borum.

1. Tap **+** → fill in Address (VM IP), Port `22`, Username, Password → tap **Save**
2. Tap the server to connect

Best for file browsing alongside a terminal — integrates with the iOS Files app.

---

#### Android

**Option A — Termius (Recommended)**

Cost: Free to start. Pro subscription for advanced features.
Install: Search **Termius** in the Google Play Store.

1. Tap **SSH** tab → tap **Add Connection** → tap **+**
2. Fill in: Label, Address (VM IP), Port `22`, Username
3. Toggle to **Password** and enter your password
4. Tap **Save and Connect**

Provides an extended keyboard row with `Ctrl`, `Alt`, `Tab`, `Esc`, and function keys. If `|` or `\` won't type, go to **Settings > Terminal** and disable **Use Option as Meta**.

---

**Option B — ConnectBot (Free and Open Source)**

Cost: Completely free. No account required.
Install: Search **ConnectBot** in the Play Store, or install from [F-Droid](https://f-droid.org/packages/org.connectbot/).

1. In the connection field, type: `username@ip-address` (e.g. `connlt1@34.105.100.200`)
2. Tap the arrow or press Enter
3. Tap **Yes** to accept the host key
4. Enter your password

Limited software keyboard support — a Bluetooth keyboard is recommended for anything beyond basic commands.

> **Note:** JuiceSSH was removed from the Play Store in December 2025 and is no longer maintained. Do not install it.

---

#### Mobile Keyboard Quick Reference

| Key | How to send it |
|---|---|
| `Tab` | App's special key row, or Bluetooth keyboard |
| `Escape` | App's special key row, or Bluetooth keyboard |
| `Ctrl+C` (cancel) | Tap `Ctrl` in key row, then tap `C` |
| `Ctrl+D` (logout) | Tap `Ctrl` in key row, then tap `D` |
| `\|` (pipe) | Extended keyboard row or long-press |
| Arrow keys | All recommended apps' key rows |

---

### First Login (VM)

On first login you will see a Debian welcome message:

```
Linux vm-connlt1 ...
...
connlt1@vm-connlt1:~$
```

**Change your password immediately:**

```bash
passwd
```

Enter your current password, then enter and confirm a new one. Nothing appears on screen as you type — this is normal.

### Logging Out

```bash
exit
```

Or press `Ctrl+D`.

### Transferring Files

**Upload from your machine to the VM:**

```bash
scp /path/to/local/file connlt1@<YOUR_VM_EXTERNAL_IP>:~/
```

**Download from the VM to your machine:**

```bash
scp connlt1@<YOUR_VM_EXTERNAL_IP>:~/remote-file /path/to/local/destination/
```

You are now connected — continue to [Part 2 — Setting Up Claude Code](#part-2--setting-up-claude-code).

---

## Part 1E — Installing Languages and Runtimes

> **Do you need this now?** If you are only setting up Claude Code and Git, you can skip this section and return when a project requires a specific language. If you are unsure, skip it for now.

Before installing anything, check what is already there:

```bash
node --version      # Node.js
npm --version       # npm
python3 --version   # Python 3
```

If a command prints a version number it is already installed. `command not found` means it needs installing.

---

### Node.js and npm

Node.js is a JavaScript runtime. npm is its package manager and is bundled with Node.js automatically.

**The recommended approach on all platforms is nvm (Node Version Manager).** It lets you install and switch between Node.js versions without affecting the rest of your system, and the commands are identical on macOS, Linux, WSL2, and the test VM.

#### Step 1 — Install nvm

Run this in your terminal (macOS, Linux, WSL2, or the test VM):

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.6/install.sh | bash
```

> Check [github.com/nvm-sh/nvm/releases](https://github.com/nvm-sh/nvm/releases) to confirm v0.40.6 is still the latest before running.

#### Step 2 — Reload Your Shell

**On Linux, WSL2, or the test VM (bash):**

```bash
source ~/.bashrc
```

**On macOS (zsh — the default shell since macOS Catalina):**

```bash
source ~/.zshrc
```

Verify nvm loaded:

```bash
nvm --version
```

Should print a version number. If it says `command not found`, re-run the source command above and try again.

#### Step 3 — Install Node.js LTS

LTS (Long Term Support) is the stable, production-recommended version:

```bash
nvm install lts/*
nvm use lts/*
nvm alias default lts/*
```

The last line makes LTS your default so it is used automatically in new terminal sessions.

#### Step 4 — Verify

```bash
node --version
npm --version
```

Should print version numbers (e.g. `v24.x.x` and `10.x.x`).

---

#### Alternative for macOS: Homebrew

If you already have Homebrew (Part 1A Step 2) and prefer not to use nvm:

```bash
brew install node
node --version
npm --version
```

This installs the current release. It does not allow switching versions — use nvm if you expect to need multiple Node.js versions.

---

### Python

#### Linux, WSL2, and the Test VM (Debian / Ubuntu)

Python 3 is pre-installed on Debian 12 and Ubuntu. What you likely need to add is pip and venv:

```bash
sudo apt update
sudo apt install -y python3-pip python3-venv
```

Verify:

```bash
python3 --version
python3 -m pip --version
```

> **Important — Debian 12 / Ubuntu 24.04 and newer:** These systems enforce [PEP 668](https://peps.python.org/pep-0668/), which means running `pip install` directly will be blocked with an error about "externally managed environment". This is intentional — always install packages inside a virtual environment (see below) rather than system-wide.

---

#### macOS

Python 3 is included with Xcode Command Line Tools, but it is typically an older version. For development, install a current version via Homebrew:

```bash
brew install python@3.13
```

Verify:

```bash
python3 --version
python3 -m pip --version
```

---

#### Virtual Environments — What They Are and Why You Need Them

**The problem Python virtual environments solve:**

When you install a Python package (e.g. `requests`, `flask`, `numpy`), it goes into a single shared location on your machine. If Project A needs version 1 of a library and Project B needs version 2, they conflict — you can only have one version installed at a time. On Debian 12 and Ubuntu 24.04, this is enforced even further: the system will outright refuse to let you install packages globally with an error like *"error: externally-managed-environment"*.

A virtual environment solves this by giving each project its own private copy of Python and its own separate folder for packages. Project A and Project B never interact.

**Think of it like this:** Your system Python is a shared kitchen in an office. A virtual environment is your own lunchbox — what you put in yours does not affect anyone else's, and you can bring exactly what you need for each day (project).

---

**Step 1 — Create a virtual environment**

Navigate into your project folder first:

```bash
cd my-project
```

Then create the virtual environment:

```bash
python3 -m venv venv
```

This creates a folder called `venv/` inside your project. It contains a private copy of Python and a place to store packages. You only do this **once per project**.

You can call it anything — `venv` is just a convention. You will see it everywhere.

---

**Step 2 — Activate it**

Before you can use the virtual environment, you must activate it for your current terminal session:

**macOS / Linux / WSL2 / test VM:**

```bash
source venv/bin/activate
```

You will see your prompt change — `(venv)` appears at the start:

```
(venv) connlt1@vm-connlt1:~/my-project$
```

That `(venv)` prefix is your confirmation that the virtual environment is active. Any packages you install now go into this environment only, not into the system.

> **Important:** You must activate the virtual environment every time you open a new terminal and want to work on this project. It does not stay active between sessions.

---

**Step 3 — Install packages**

With the environment active, install packages using:

```bash
python3 -m pip install requests
```

Replace `requests` with whatever package you need. Multiple packages can be listed:

```bash
python3 -m pip install requests flask numpy
```

To install everything a project requires (when a `requirements.txt` file exists):

```bash
python3 -m pip install -r requirements.txt
```

> **Why `python3 -m pip` and not just `pip`?** Using `python3 -m pip` makes absolutely certain you are using the pip that belongs to your active environment. `pip` and `pip3` are shortcuts that can sometimes point to the wrong place on systems with multiple Python versions.

---

**Step 4 — Save your project's dependencies**

When you want to record exactly which packages your project depends on (so others can reproduce your setup):

```bash
python3 -m pip freeze > requirements.txt
```

This creates or overwrites `requirements.txt` with a list of every installed package and its version. Commit this file to Git — it is how collaborators (and Claude Code) know what to install.

---

**Step 5 — Deactivate when done**

When you are finished working:

```bash
deactivate
```

The `(venv)` prefix disappears and you are back to the system Python. Your packages are still saved inside the `venv/` folder — they are not deleted.

---

**Step 6 — Reactivate next time**

The next time you open a terminal and want to work on this project:

```bash
cd my-project
source venv/bin/activate
```

That is all. Your packages are still there from before.

---

**What to add to .gitignore**

The `venv/` folder can be hundreds of megabytes and should never be committed to Git. Add it to your `.gitignore` file:

```bash
echo "venv/" >> .gitignore
```

Anyone who clones your project runs `python3 -m venv venv && source venv/bin/activate && python3 -m pip install -r requirements.txt` to recreate it.

---

**Complete workflow summary**

```bash
# First time (once per project)
cd my-project
python3 -m venv venv
source venv/bin/activate
python3 -m pip install flask requests     # whatever you need
python3 -m pip freeze > requirements.txt  # save dependencies
echo "venv/" >> .gitignore

# Every subsequent session
cd my-project
source venv/bin/activate
# ... do your work ...
deactivate
```

---

#### Advanced: pyenv (Optional)

If you need to work with multiple Python versions simultaneously (e.g. testing a library against Python 3.11, 3.12, and 3.13), consider **pyenv** — the Python equivalent of nvm.

**macOS:**

```bash
brew install pyenv
```

**Linux / WSL2 / test VM:**

```bash
curl https://pyenv.run | bash
```

Then add to your shell profile (follow the instructions printed after install) and reload your shell. Usage:

```bash
pyenv install 3.13.0     # install a specific version
pyenv global 3.13.0      # set as default
pyenv versions           # list all installed versions
```

For most users starting out, the system Python + venv is all that is needed. Come back to pyenv when a project specifically requires it.

---

### Quick Reference — Runtimes by Platform

| | macOS | Windows (WSL2) | Linux / Test VM |
|---|---|---|---|
| **Node.js (recommended)** | nvm | nvm | nvm |
| **Node.js (alternative)** | `brew install node` | — | — |
| **nvm install** | `curl -o- .../install.sh \| bash` | `curl -o- .../install.sh \| bash` | `curl -o- .../install.sh \| bash` |
| **Node LTS** | `nvm install lts/*` | `nvm install lts/*` | `nvm install lts/*` |
| **Python 3** | `brew install python@3.13` | pre-installed | pre-installed |
| **pip / venv** | included with brew python | `apt install python3-pip python3-venv` | `apt install python3-pip python3-venv` |
| **Virtual env** | `python3 -m venv venv` | `python3 -m venv venv` | `python3 -m venv venv` |
| **Activate venv** | `source venv/bin/activate` | `source venv/bin/activate` | `source venv/bin/activate` |

---

## Part 2 — Setting Up Claude Code

These steps apply whether you are on your own machine (Parts 1A–1C) or connected to the test VM (Part 1D). Run them in your Linux terminal.

### Step 1 — Install Prerequisites

If you followed Part 1A (macOS), you already have these. Everyone else, run:

```bash
sudo apt update
sudo apt install -y curl git gnupg
```

Verify:

```bash
curl --version
git --version
```

Both should print version numbers. If either says `command not found`, re-run the install line above.

### Step 2 — Install Claude Code

```bash
curl -fsSL https://claude.ai/install.sh | bash
```

This downloads and installs a pre-built Claude Code binary into `~/.local/bin/`. Wait for the install success message.

### Step 3 — Add Claude Code to Your PATH

```bash
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

> **macOS note:** If you are using zsh (the default shell on macOS), substitute `~/.zshrc` for `~/.bashrc` in the command above.

### Step 4 — Verify the Installation

```bash
claude --version
```

Should print a version number such as `2.1.211 (Claude Code)`. If you see `command not found`, re-run Step 3.

```bash
claude doctor
```

All checks should pass. Note any warnings — they may indicate missing dependencies.

### Step 5 — Configure Git

Claude Code requires Git to be configured with a name and email before it will work:

```bash
git config --global user.name "Your Name"
git config --global user.email "your@email.com"
```

Verify:

```bash
git config --global --list
```

You should see `user.name` and `user.email` in the output.

### Step 6 — Authenticate Claude Code

You need either a **Claude.ai account** (Pro, Max, Team, or Enterprise) or an **Anthropic Console API key**.

---

#### Method A — Claude.ai Account Login (Recommended)

Works without a browser on the machine — Claude Code shows a URL you open elsewhere.

1. Run:

   ```bash
   claude
   ```

2. Claude Code displays a URL:

   ```
   Visit this URL to log in:
   https://claude.ai/authorize?code=XXXXXXXXXX
   ```

3. Open that URL in a browser **on any device** (your phone is fine)

4. Sign in to your Claude.ai account and approve the request

5. Copy the short authorisation code the browser shows

6. Paste it back into the terminal and press Enter

7. Terminal shows `Login successful`

Credentials are stored in `~/.claude/.credentials.json` and persist across sessions automatically.

---

#### Method B — API Key

If you have an Anthropic Console API key (starts with `sk-ant-`):

```bash
echo 'export ANTHROPIC_API_KEY="sk-ant-your-key-here"' >> ~/.bashrc
source ~/.bashrc
```

Verify it is set:

```bash
echo $ANTHROPIC_API_KEY
```

Then run `claude` and approve the key when prompted.

---

### Step 7 — Start Using Claude Code

```bash
claude
```

Opens an interactive session. Type your request in plain English and press Enter.

One-off command without entering the session:

```bash
claude -p "your question or task here"
```

Exit the session: type `/exit` or press `Ctrl+C`.

---

## Part 2B — Prompting Claude Code Effectively

Claude Code understands plain English, but the quality of what it produces is directly shaped by how clearly you communicate. This section covers how to write prompts that get the right result first time, and how to build up persistent context so you don't have to repeat yourself.

---

### The Prompt That Built This Setup

Below is the prompt used to create the entire infrastructure you are using right now — the VMs, firewall rules, GitHub repo, documentation, GitHub Pages site, and branch protection. You can use it as a template for similar projects. Replace the values in `[brackets]` with your own.

```
Set up a developer infrastructure for a small team of 3 people.

## Google Cloud Project
Authenticate via the Google Compute Engine MCP connector.
Project ID: [your-gcp-project-id]

## Virtual Machines
Create 3 VMs:
- Machine type: n2-standard-4 (4 vCPU, 16 GB RAM)
- Zone: [your-zone] (e.g. europe-west2-a)
- OS: Debian 12
- Name and assign one VM per user:
  - vm-[user1] → username: [user1]
  - vm-[user2] → username: [user2]
  - vm-[user3] → username: [user3]
- I will provide passwords separately after the VMs are created.

## Firewall
On the default VPC network:
- Create a firewall rule allowing TCP 22 (SSH) from 0.0.0.0/0
- Target instances tagged ssh-access
- Apply the tag to all three VMs

## GitHub Repository
Repository URL: [https://github.com/your-org/your-repo.git]
Use the PAT I will provide.

Push the following files:
1. README.md — quick reference table of VMs and users
2. BUILD.md — admin guide: VM specs, firewall config, user account
   setup commands, branching strategy, rebuild and teardown instructions
3. CONSUMER_GUIDE.md — end-user guide covering:
   - SSH from Windows, macOS, Linux
   - SSH from iOS and Android (Termius, Blink, ConnectBot)
   - Claude Code install and authentication
   - Python venv from scratch, assuming no prior knowledge
   - Node.js and npm via nvm
   - GitHub basics: account, PAT, clone, add/commit/push, branches, PRs
   - Claude Code extensions: hooks, skills, plugins, MCP servers
   Include diagrams for: git workflow, SSH network path, hooks lifecycle,
   MCP architecture, skills vs plugins containment.

## GitHub Pages
- Enable GitHub Pages from the main branch root
- Use jekyll-theme-cayman
- Make the repo public
- The consumer guide should be the homepage (index.md with front matter)
- Add Mermaid.js via _includes/head-custom.html for diagram rendering

## Branch Protection on main
- Block direct pushes (PRs required)
- Require 1 approving review
- Dismiss stale reviews on new commits
- Require last-push approval
- Block force pushes and branch deletion
```

**Why this works:** The prompt specifies outcomes, not steps. It tells Claude *what* to create and *what properties it should have*, then trusts Claude to figure out the order of operations, which tools to use, and how to handle errors. Passwords are deliberately excluded — always provide sensitive credentials separately, never in the initial prompt.

---

### Prompting Strategies

#### 1 — Be specific about the outcome, not the steps

Tell Claude what you want the end result to look like. Claude will work out the how.

| Instead of this | Say this |
|---|---|
| "Can you help me with my VMs?" | "Create 3 Debian 12 VMs in europe-west2-a with n2-standard-4 and assign an external IP to each" |
| "Fix the code" | "The `/login` route returns a 500 when the password field is empty. Fix the root cause and confirm the fix with a test" |
| "Update the docs" | "Add a section to CONSUMER_GUIDE.md explaining Python virtual environments — assume the reader has never used Python before" |

The clearer the target, the less back-and-forth is needed.

---

#### 2 — Give context: explain *why*, not just *what*

Claude makes better decisions when it understands the constraint or motivation behind a request.

```
# Less effective
Add error handling to the upload function.

# More effective
Add error handling to the upload function. This runs on a VM with
limited disk space — if the upload exceeds 50 MB or the disk is
more than 80% full, reject it with a clear error message rather
than letting it fail silently.
```

The second prompt tells Claude *why* the error handling matters, which shapes the implementation.

---

#### 3 — Use CLAUDE.md for facts Claude should know every session

CLAUDE.md is a file Claude reads automatically at the start of every session. Anything you find yourself repeating across sessions belongs there.

Create it at the root of your project:

```bash
touch CLAUDE.md
```

Example content:

```markdown
# Project: Cuddly-Google

## Infrastructure
- GCP project: poised-beach-505408-r2
- Zone: europe-west2-a
- VMs: vm-connlt1, vm-connlt2, vm-connlt3 (n2-standard-4, Debian 12)
- GitHub Pages URL: https://funfairlabs-incubator.github.io/cuddly-google/

## Rules
- Never push directly to main — always use a branch and PR
- Never include passwords or secrets in committed files
- Run `git status` before any commit to check what is staged
- All documentation changes go to both index.md and CONSUMER_GUIDE.md

## Style
- Write for readers with no technical background
- Use plain English, numbered steps, and concrete examples
- Keep troubleshooting rows short: problem | likely cause | fix
```

Or let Claude generate a starting point for you:

```
/init
```

---

#### 4 — Plan before executing on complex changes

For anything that touches multiple files or has real consequences (infrastructure changes, large refactors), ask Claude to plan first without making edits:

```
Read the codebase and tell me how you would add X.
List the files you would change and what you would do in each.
Do not make any edits yet.
```

Review the plan, correct it if needed, then say:

```
That looks right. Go ahead.
```

This prevents Claude from going deep in the wrong direction.

---

#### 5 — Ask Claude to think harder on complex problems

For genuinely difficult problems — architecture decisions, tricky bugs, security reviews — tell Claude to reason carefully:

```
Think carefully about the security implications before making changes.

Reason through this step by step before writing any code.

This is a complex infrastructure change — consider failure modes
before proposing a solution.
```

Reserve this for problems that warrant it. On routine tasks, it adds latency without benefit.

---

#### 6 — Break long tasks into checkpoints

For large tasks, give Claude one clear goal at a time. Finish each before moving to the next.

```
Step 1: Create the three VMs and confirm they are running.
        Stop there and tell me the result.

[Claude confirms]

Step 2: Now create the firewall rule and tag the VMs.
        Stop and confirm.

[Claude confirms]

Step 3: Now set up the GitHub repo and push the docs.
```

This makes it easy to catch problems early and keeps the session focused.

---

#### 7 — Compact long sessions before starting new work

Each thing Claude reads or writes adds to the session context. For long sessions, run:

```
/compact
```

Claude summarises everything it has done so far and continues with a clean, efficient context. Do this at natural stopping points — after a feature is complete, before starting a new section of work.

---

#### 8 — Common patterns by task type

**Infrastructure:**
```
I want to [outcome]. The environment is [details].
Verify the current state first, then make the change,
then confirm it worked.
```

**Debugging:**
```
[Error message or symptom]. The command to reproduce it is [command].
Find the root cause and fix it. Do not just catch the error — fix why it happens.
```

**Documentation:**
```
Read [existing examples] to understand the style and structure,
then write a new section on [topic] for readers who have never
encountered [concept] before.
```

**Code generation:**
```
Add [feature] to [file]. Follow the same pattern as [existing example].
Run [test command] after making the change to confirm nothing broke.
```

---

#### 9 — What not to do

| Avoid | Why |
|---|---|
| Vague requests ("improve this", "make it better") | Claude cannot infer your standard — be specific |
| Asking Claude to guess passwords or credentials | Always provide sensitive values separately and deliberately |
| Very long CLAUDE.md files (over 200 lines) | Longer files reduce how reliably Claude follows them |
| Forcing Claude to verify every small step | Claude self-corrects — constant verification requests slow things down |
| Asking for suggestions when you want action | Say "do this" not "what do you think about doing this" |
| Continuing a session for hours without compacting | Context fills up — compact at checkpoints for better performance |

---

## Part 3 — Extending Claude Code

Claude Code can be extended beyond its built-in behaviour in four ways. Each adds different capabilities, and they can be combined.

| Feature | What it does | How you set it up |
|---|---|---|
| **Hooks** | Run your own scripts automatically at lifecycle events | Edit `settings.json` |
| **Skills** | Add custom `/slash-commands` to Claude | Create a `SKILL.md` file |
| **Plugins** | Install bundles of skills, hooks, and tools | `/plugin install` |
| **MCP Servers** | Connect Claude to external services (GitHub, Slack, databases…) | `claude mcp add` |

A plugin *contains* skills, hooks, and MCP servers. A standalone skill is just a single markdown file. The diagram below shows the containment relationship:

<div class="mermaid">
flowchart TD
    subgraph PLUGIN["📦 Plugin — installed via /plugin install\nVersioned · shareable · lives in a marketplace"]
        PL_SK["⚡ Skills\n/plugin-name:skill-name"]
        PL_HK["🪝 Hooks\nauto-fire at lifecycle events"]
        PL_MCP["🔌 MCP Server configs\nauto-connected on install"]
        PL_AG["🤖 Agents\nspecialised sub-assistants"]
    end

    STANDALONE["⚡ Standalone Skill\n.claude/skills/my-skill/SKILL.md\nA single markdown file — /my-skill"]

    CC["🤖 Claude Code"]

    STANDALONE -->|"you type /my-skill"| CC
    PLUGIN -->|"all components load\nautomatically on install"| CC

    style PLUGIN fill:#f3e5f5,stroke:#7b1fa2,color:#4a148c
    style STANDALONE fill:#e3f2fd,stroke:#1976d2,color:#0d47a1
    style CC fill:#e8f5e9,stroke:#43a047,color:#1b5e20
</div>

---

### Hooks

A hook is a shell command (or HTTP request, or LLM prompt) that Claude Code runs automatically at a specific point — before a tool runs, after a file is written, when the session ends, and so on. You do not invoke hooks manually; they fire on their own.

**Common uses:**
- Block dangerous commands before they execute
- Auto-run a linter every time Claude edits a file
- Send a desktop notification when Claude is waiting for your input
- Log every bash command Claude runs

The diagram below shows exactly when each hook fires and which ones can block Claude from proceeding:

<div class="mermaid">
flowchart TD
    U["👤 You submit a prompt"]
    UPS["🪝 UserPromptSubmit hook\nfires before Claude reads your message"]
    AI1["🤖 Claude\nplans what to do"]
    PRE["🪝 PreToolUse hook\nfires before every tool call"]
    DEC{"Allowed?"}
    TOOL["🔧 Tool executes\nBash · Edit · Write · etc."]
    POST["🪝 PostToolUse hook\nClaude sees the result"]
    MORE{"More\nsteps?"}
    STOP["🤖 Claude finishes\nits turn"]
    STOPHOOK["🪝 Stop hook\nruns after Claude is done"]

    BLK1["🚫 Blocked\nprompt never processed"]
    BLK2["🚫 Blocked\ntool never runs"]

    U --> UPS
    UPS -->|"exit 0 — proceed"| AI1
    UPS -->|"exit 2 — block"| BLK1
    AI1 --> PRE
    PRE --> DEC
    DEC -->|"exit 0 — allowed"| TOOL
    DEC -->|"exit 2 — blocked"| BLK2
    TOOL --> POST
    POST --> MORE
    MORE -->|"yes — keep going"| AI1
    MORE -->|"no"| STOP
    STOP --> STOPHOOK

    style UPS fill:#e65100,stroke:#bf360c,color:#fff
    style PRE fill:#e65100,stroke:#bf360c,color:#fff
    style POST fill:#e65100,stroke:#bf360c,color:#fff
    style STOPHOOK fill:#e65100,stroke:#bf360c,color:#fff
    style DEC fill:#e3f2fd,stroke:#1565c0
    style BLK1 fill:#c62828,stroke:#b71c1c,color:#fff
    style BLK2 fill:#c62828,stroke:#b71c1c,color:#fff
</div>

**How hooks are configured**

Hooks live in a `settings.json` file. There are two locations:

- `~/.claude/settings.json` — applies to all your projects
- `.claude/settings.json` — applies to this project only (can be committed to Git)

Open or create the file and add a `hooks` block. The structure has three levels: the event name, a matcher (which tool to watch), and the action to take:

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "./.claude/hooks/check.sh",
            "timeout": 30
          }
        ]
      }
    ]
  }
}
```

**Supported events:**

| Event | When it fires |
|---|---|
| `PreToolUse` | Before any tool runs (can block the action) |
| `PostToolUse` | After a tool succeeds |
| `UserPromptSubmit` | When you press Enter on a prompt (can block) |
| `Stop` | When Claude finishes a turn |
| `SessionStart` | When a Claude Code session opens |
| `SessionEnd` | When a session closes |
| `FileChanged` | When a watched file changes on disk |

**Matcher values:** `"Bash"`, `"Write"`, `"Edit"`, `"Write|Edit"`, `"*"` (matches all tools)

**Handler types:**

```json
{ "type": "command", "command": "/path/to/script.sh", "timeout": 30 }
{ "type": "http", "url": "http://localhost:8080/hook", "timeout": 10 }
```

**Exit codes:** `0` = success, `2` = block the action (prevent Claude from proceeding), anything else = non-blocking error.

**Three real examples:**

Block `rm -rf` before it runs — create `.claude/hooks/block-rm.sh`:

```bash
#!/bin/bash
if echo "$CLAUDE_TOOL_INPUT" | grep -q "rm -rf"; then
  echo "Blocked: rm -rf is not allowed"
  exit 2
fi
```

Then add to `.claude/settings.json`:

```json
{
  "hooks": {
    "PreToolUse": [{ "matcher": "Bash", "hooks": [{ "type": "command", "command": "./.claude/hooks/block-rm.sh" }] }]
  }
}
```

Auto-lint after every file write:

```json
{
  "hooks": {
    "PostToolUse": [{ "matcher": "Write|Edit", "hooks": [{ "type": "command", "command": "npm", "args": ["run", "lint:fix"], "timeout": 30 }] }]
  }
}
```

Desktop notification when Claude stops (macOS):

```json
{
  "hooks": {
    "Stop": [{ "matcher": "*", "hooks": [{ "type": "command", "command": "osascript", "args": ["-e", "display notification \"Claude is waiting\" with title \"Claude Code\""] }] }]
  }
}
```

**View all configured hooks inside a session:**

```
/hooks
```

**Disable all hooks temporarily** — add to `settings.json`:

```json
{ "disableAllHooks": true }
```

---

### Skills

A skill is a custom slash command you create for your project. Type `/my-skill` in Claude Code and it executes a set of instructions you wrote. Skills are plain markdown files — no code required.

**Common uses:**
- `/review` — run your team's standard code review checklist
- `/deploy staging` — walk through your deployment steps
- `/standup` — summarise what changed since yesterday

**Creating a skill**

Create the folder and file:

```bash
mkdir -p .claude/skills/review
```

Create `.claude/skills/review/SKILL.md`:

```markdown
---
description: Review code for quality, security, and best practices
---

Review the current code changes for:
- Logic errors and edge cases
- Security vulnerabilities (injection, exposed secrets, unsafe inputs)
- Missing error handling
- Test coverage gaps
- Readability and naming

Give a verdict: approve, needs changes, or reject.
```

**The frontmatter (the `---` block) is required.** The `description` field is what appears in `/help`.

**Invoking a skill:**

```
/review
```

With arguments (referenced as `$ARGUMENTS` in the skill file):

```
/deploy production
```

Skill file using arguments:

```markdown
---
description: Deploy to a named environment
---

Deploy the application to the "$ARGUMENTS" environment.
Run pre-deploy checks, confirm nothing is broken, then deploy.
```

**Where skills live:**

- `.claude/skills/skill-name/SKILL.md` — project scope (commit to Git)
- `~/.claude/skills/skill-name/SKILL.md` — user scope (all projects)

**View all available skills:**

```
/help
```

Scroll to the Custom commands section.

**Remove a skill:**

```bash
rm -rf .claude/skills/review
```

---

### Plugins

A plugin is a shareable, versioned package that bundles multiple skills, hooks, MCP servers, and agents into a single installable unit. Where a skill is a single command you write yourself, a plugin is a collection you install from a marketplace.

**Install a plugin:**

```
/plugin install github@claude-plugins-official
```

You will be asked to choose a scope:
- `user` — available in all your projects
- `project` — added to `.claude/settings.json` (shared with teammates)
- `local` — added to `.claude/settings.local.json` (only you, not committed)

**Browse and install interactively:**

```
/plugin
```

Go to the **Discover** tab. Find a plugin and press Enter to install.

**Useful plugin commands:**

```bash
/plugin list              # see what's installed
/plugin disable name      # turn off without uninstalling
/plugin enable name       # turn back on
/plugin uninstall name    # remove completely
```

Plugin-bundled skills are namespaced to avoid clashes:

```
/github:create-pr
/security-guidance:scan
```

**Where plugin config is stored:**

- `~/.claude/settings.json` — user-scope plugins
- `.claude/settings.json` — project-scope plugins (commit this)
- `.claude/settings.local.json` — local-scope plugins (do not commit)

---

### MCP Servers

MCP (Model Context Protocol) servers connect Claude Code to external services and tools — GitHub, Slack, databases, browser automation, and anything else that implements the standard. Once connected, Claude can use the server's tools automatically without you having to copy-paste data in.

Claude Code never talks directly to GitHub or Slack. An MCP server always sits in between, translating Claude's requests into the service's own API:

<div class="mermaid">
flowchart LR
    subgraph CC["🤖 Claude Code"]
        AI["Claude\nthe AI"]
    end

    subgraph MCP["MCP Servers — run separately, one per service"]
        GH_M["GitHub MCP"]
        SL_M["Slack MCP"]
        PW_M["Playwright MCP"]
        DB_M["Supabase MCP"]
    end

    subgraph EXT["External Services"]
        GH_E["GitHub\nRepos · PRs · Issues"]
        SL_E["Slack\nMessages · Channels"]
        PW_E["Browser\nWeb pages · UIs"]
        DB_E["Database\nTables · Rows"]
    end

    AI -->|"MCP protocol\ntool calls"| GH_M
    AI -->|"MCP protocol\ntool calls"| SL_M
    AI -->|"MCP protocol\ntool calls"| PW_M
    AI -->|"MCP protocol\ntool calls"| DB_M

    GH_M <-->|"GitHub REST API"| GH_E
    SL_M <-->|"Slack API"| SL_E
    PW_M <-->|"browser automation"| PW_E
    DB_M <-->|"SQL / REST"| DB_E

    style CC fill:#e8f5e9,stroke:#43a047
    style MCP fill:#f3e5f5,stroke:#7b1fa2
    style EXT fill:#e8eaf6,stroke:#3949ab
</div>

**Add an MCP server:**

Hosted service (HTTP):

```bash
claude mcp add --transport http claude-docs https://code.claude.com/docs/mcp
```

Local process (stdio — runs a command on your machine):

```bash
claude mcp add playwright -- npx -y @playwright/mcp@latest
```

**Check connection status:**

```bash
claude mcp list
```

Shows `✔ Connected`, `! Needs authentication`, or `✘ Failed to connect` for each server.

**Real examples:**

**GitHub** — create PRs, read issues, manage repos:

```bash
claude mcp add --transport http github https://mcp.github.com/mcp \
  --header "Authorization: Bearer ghp_YOUR_PAT_HERE"
```

Replace `ghp_YOUR_PAT_HERE` with a GitHub Personal Access Token (see Part 4 Step 2 for how to create one).

**Playwright** — automate a browser, test web UIs:

```bash
claude mcp add playwright -- npx -y @playwright/mcp@latest
```

**Slack** — read and send messages:

```bash
claude mcp add --transport http slack https://mcp.slack.com/mcp
```

After adding, run `/mcp`, select `slack`, choose **Authenticate**, and sign in via browser.

**Share MCP servers with your team**

Add at project scope so everyone who clones the repo gets the same servers:

```bash
claude mcp add --scope project --transport http claude-docs https://code.claude.com/docs/mcp
```

This writes to `.mcp.json` in the project root. Commit that file:

```bash
git add .mcp.json
git commit -m "Add Claude docs MCP server"
```

Teammates will see a prompt asking them to approve the server when they open the project.

**Useful MCP commands:**

```bash
claude mcp list                  # show all servers and their status
claude mcp get server-name       # show config for one server
claude mcp remove server-name    # remove a server
```

Inside a session:

```
/mcp
```

Shows all servers, their connection status, and the tools each one provides.

---

### Config File Reference

| File | Scope | Commit to Git? | Contains |
|---|---|---|---|
| `~/.claude/settings.json` | Your user, all projects | No | Hooks, plugins, preferences |
| `.claude/settings.json` | This project, all teammates | Yes | Hooks, plugins, shared config |
| `.claude/settings.local.json` | This project, only you | No | Personal overrides |
| `.claude/skills/` | This project | Yes | Skill definitions |
| `~/.claude/skills/` | Your user, all projects | No | Personal skills |
| `.mcp.json` | This project, all teammates | Yes | MCP server definitions |
| `~/.claude.json` | Your user, all projects | No | User MCP servers, plugins |

---

## Part 4 — Using GitHub

GitHub stores your code remotely. Git (running on your machine or VM) is the tool that syncs changes between your local files and GitHub.

### Branch Protection — Why You Cannot Push Directly to main

The `main` branch is protected. It serves the live GitHub Pages site, so a direct push would immediately change what everyone sees. To prevent accidental or untested changes going live, the following rules are enforced by GitHub:

| Rule | Effect |
|---|---|
| Direct pushes blocked | You must open a Pull Request — `git push origin main` will be rejected |
| 1 approval required | Someone else must review and approve your PR before it can merge |
| Stale review dismissal | If you push new commits to an open PR, existing approvals are reset |
| Force pushes blocked | `git push --force` to main is always rejected |
| Branch deletion blocked | `main` cannot be deleted |

**Your workflow is always:** create a branch → push the branch → open a PR → get approved → merge.

<div class="mermaid">
flowchart TD
    MAIN["🟢 main\nProtected · serves the live site\nDirect pushes blocked"]

    subgraph WORK["Your working area"]
        BRANCH["🔀 your-feature-branch\ngit checkout -b fix/my-change"]
        COMMITS["📝 Commits\ngit add · git commit"]
        PUSH["⬆️ git push origin your-branch"]
    end

    PR["📋 Pull Request\nOpened on GitHub\nRequires 1 approval"]
    REVIEW["👀 Reviewer approves"]
    MERGE["✅ Merge into main\nPages site rebuilds automatically"]

    MAIN -->|"git checkout -b fix/my-change\nalways branch from latest main"| BRANCH
    BRANCH --> COMMITS --> PUSH --> PR --> REVIEW --> MERGE --> MAIN

    DIRECT["❌ git push origin main\nRejected by GitHub"]
    MAIN -.->|"direct push attempt"| DIRECT

    style MAIN fill:#28a745,color:#fff,stroke:#1e7e34
    style BRANCH fill:#0366d6,color:#fff,stroke:#024fa0
    style DIRECT fill:#c62828,stroke:#b71c1c,color:#fff
    style PR fill:#e65100,stroke:#bf360c,color:#fff
    style REVIEW fill:#e65100,stroke:#bf360c,color:#fff
    style MERGE fill:#e8f5e9,stroke:#43a047
</div>

### How It All Fits Together

Before the step-by-step, here is the full picture — including where Claude Code fits in.

<div class="mermaid">
flowchart TD
    subgraph GH["☁️  GitHub — Remote"]
        MAIN["🟢  main\nStable · reviewed · production-ready"]
        BRANCH["🔀  feature branch\nYour work in progress"]
    end

    subgraph LOCAL["🖥️  Your Machine or VM — Local Working Directory"]
        FILES["📁  Files on disk\nThis is what you and Claude Code both edit"]
        YOU["✏️  You\nEdit files manually\nin the terminal"]
        CC["🤖  Claude Code\nEdits files automatically\non your behalf — same files, same result"]
    end

    MAIN -->|"① git checkout -b my-branch\nCreate a branch to work on"| BRANCH
    BRANCH -->|"② git pull\nDownload the branch to your machine"| FILES
    YOU -->|makes edits to| FILES
    CC -->|makes edits to| FILES
    FILES -->|"③ git add · git commit\nSnapshot your changes"| STAGED["📦  Committed changes\nReady to upload"]
    STAGED -->|"④ git push\nUpload to GitHub"| BRANCH
    BRANCH -->|"⑤ Pull Request → Merge\nPropose and accept the changes"| MAIN

    style MAIN fill:#28a745,color:#fff,stroke:#1e7e34
    style BRANCH fill:#0366d6,color:#fff,stroke:#024fa0
    style FILES fill:#fff8e1,color:#333,stroke:#f9a825
    style YOU fill:#fff3cd,color:#856404,stroke:#ffc107
    style CC fill:#d1ecf1,color:#0c5460,stroke:#17a2b8
    style STAGED fill:#e8f5e9,color:#2e7d32,stroke:#43a047
</div>

**The key insight:** It does not matter whether *you* edited the files or *Claude Code* edited them — both paths produce exactly the same result. Git tracks what changed in the files, not who or what changed them. The `add → commit → push` workflow is identical either way.

**What lives where:**
- `main` on GitHub is the single source of truth — always stable
- A feature branch is your personal scratch space — safe to experiment
- Your machine or VM is where the actual files live and get changed
- `git push` / `git pull` are the bridge between your local files and GitHub

---

### Step 1 — Create a GitHub Account

If you do not already have one:

1. Go to [https://github.com](https://github.com)
2. Click **Sign up**
3. Enter your email address, create a password, and choose a username
4. Verify your email address when prompted

### Step 2 — Create a Personal Access Token (PAT)

GitHub no longer accepts your account password for command-line Git operations. You must use a Personal Access Token instead.

1. Sign in to [https://github.com](https://github.com)
2. Click your profile picture → **Settings**
3. Scroll the left sidebar → **Developer settings**
4. Click **Personal access tokens** → **Tokens (classic)**
5. Click **Generate new token** → **Generate new token (classic)**
6. Fill in:
   - **Note**: e.g. `My machine` or `VM access`
   - **Expiration**: 90 days is a reasonable starting point
   - **Select scopes**: tick **repo**
7. Click **Generate token**
8. **Copy the token immediately** — it is only shown once. It starts with `ghp_`.

Store it in a password manager. If you lose it, generate a new one.

### Step 3 — Cache Your Credentials

So you are not asked for your token on every push:

```bash
git config --global credential.helper store
```

The first time you push, enter your GitHub username and paste your token as the password. Git will remember it from then on.

### Step 4 — Clone a Repository

1. Go to the repo on GitHub
2. Click the green **Code** button → **HTTPS** → copy the URL
3. On your machine or VM:

   ```bash
   git clone https://github.com/username/repo-name.git
   cd repo-name
   ```

### Step 5 — The Daily Workflow

Every session follows the same cycle: **pull → edit → add → commit → push**.

```bash
git pull                        # Get the latest changes from GitHub
# ... make your edits ...
git status                      # See what changed
git add filename.txt            # Stage a specific file
git add .                       # Or stage everything
git commit -m "What you did"    # Save a snapshot with a description
git push                        # Upload to GitHub
```

### Step 6 — Branches

Because `main` is protected, working on a branch is not optional — it is the only way to make changes. A direct push to `main` will be rejected by GitHub.

**Branch naming conventions:**

| Prefix | Use for | Example |
|---|---|---|
| `fix/` | Corrections and bug fixes | `fix/typo-in-part-3` |
| `add/` | New content or features | `add/docker-section` |
| `update/` | Changes to existing content | `update/venv-explanation` |
| `infra/` | VM or infrastructure changes | `infra/add-fourth-vm` |

```bash
git checkout -b my-feature      # Create and switch to a new branch
git branch                      # See which branch you are on (* = current)
git push -u origin my-feature   # Push the branch to GitHub the first time
git checkout main               # Switch back to main
git branch -d my-feature        # Delete the branch locally after merging
```

### Step 7 — Pull Requests

A Pull Request (PR) proposes merging your branch into `main`. Done on the GitHub website.

1. Push your branch
2. Go to the repo on GitHub
3. Click **Compare & pull request** in the yellow banner
4. Fill in a title and description → click **Create pull request**
5. Once approved, click **Merge pull request** → **Confirm merge**
6. Locally:

   ```bash
   git checkout main
   git pull
   ```

### Step 8 — Other Useful Commands

```bash
git diff filename.txt           # See exactly what changed in a file
git log --oneline               # See the commit history (press q to exit)
git restore filename.txt        # Undo unsaved changes to a file
git pull origin branch-name     # Pull a specific branch
```

---

## Troubleshooting

| Problem | Likely Cause | Fix |
|---|---|---|
| `ssh: command not found` on macOS | Rare — re-install Xcode CLT | `xcode-select --install` |
| `brew: command not found` | Homebrew not in PATH | Apple Silicon: run the `eval "$(/opt/homebrew/bin/brew shellenv)"` line again |
| WSL won't install | Windows version too old | Check you are on Windows 10 2004+ or Windows 11 |
| WSL opens but has no internet | DNS issue | Run `echo "nameserver 8.8.8.8" | sudo tee /etc/resolv.conf` inside WSL |
| `Connection refused` on SSH | VM is stopped | Ask administrator to start the VM |
| `Permission denied` on SSH | Wrong username or password | Double-check credentials |
| `Connection timed out` on SSH | Firewall or wrong IP | Confirm external IP with your administrator |
| Mobile SSH disconnects | App going to background | Keep the app in foreground; enable keepalive in app settings |
| `command not found: claude` | PATH not set | Run `source ~/.bashrc` (or `source ~/.zshrc` on macOS) |
| `curl: command not found` | curl not installed | `sudo apt install -y curl` |
| `git: command not found` | git not installed | `sudo apt install -y git` (Linux/WSL) or `xcode-select --install` (macOS) |
| Claude Code installer fails | No internet | Check network; ask administrator if on VM |
| Login URL does not work | Not signed in to claude.ai | Open URL in a browser where you are signed in |
| `permission denied` on `~/.claude/` | Wrong file permissions | `chmod 700 ~/.claude/ && chmod 600 ~/.claude/.credentials.json` |
| Claude Code cannot find files | ripgrep missing | `sudo apt install -y ripgrep` |
| `nvm: command not found` | Shell not reloaded after nvm install | Run `source ~/.bashrc` (or `~/.zshrc` on macOS) |
| `node: command not found` after nvm install | nvm loaded but no version active | Run `nvm use lts/*` |
| `npm: command not found` | npm not installed | Comes with Node.js — reinstall via `nvm install lts/*` |
| `pip install` blocked: "externally managed environment" | Debian/Ubuntu PEP 668 enforcement | Use a virtual environment: `python3 -m venv venv && source venv/bin/activate` |
| `python3 -m venv` fails | venv not installed | `sudo apt install -y python3-venv` |
| `pip3: command not found` | pip not installed | `sudo apt install -y python3-pip` |
| `git push` asks for token every time | Credential helper not set | `git config --global credential.helper store` |
| `git push` rejected | Remote has commits you don't have | `git pull` first, then `git push` |
| `error: src refspec main does not match any` | No commits yet | Make at least one commit before pushing |
| `git push` rejected: "protected branch" | Tried to push directly to main | Create a branch, push that, and open a PR instead |
| PR cannot be merged: "review required" | No approval yet | Ask a teammate to review and approve the PR |
| PR approval dismissed after new commit | Stale review rule fired | Re-request review after your latest commit |
| Accidentally committed to `main` locally | Forgot to branch first | Run `git checkout -b fix/my-change` — your commits move to the new branch |

---

## Support

Contact your administrator if you are unable to connect or complete setup.
