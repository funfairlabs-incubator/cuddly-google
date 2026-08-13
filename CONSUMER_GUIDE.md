# Consumer Guide

## Before You Start — Choose Your Setup

Everything in this guide ends up in the same place: a Linux terminal with Claude Code and Git installed, connected to GitHub. The only difference is *where* that terminal lives.

| I am working on… | Go to… |
|---|---|
| My own **Mac** | [Part 1A — macOS Local Setup](#part-1a--macos-local-setup) |
| My own **Windows PC** | [Part 1B — Windows Local Setup (WSL2)](#part-1b--windows-local-setup-wsl2) |
| My own **Linux machine** | [Part 1C — Linux Local Setup](#part-1c--linux-local-setup) |
| The **shared test VM** provided to me | [Part 1D — Connecting to the Test VM](#part-1d--connecting-to-the-test-vm) |

> Parts 2 (Claude Code) and 3 (GitHub) are shared — follow them whichever path you took.

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

## Part 3 — Using GitHub

GitHub stores your code remotely. Git (running on your machine or VM) is the tool that syncs changes between your local files and GitHub.

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

Always work on a branch — never directly on `main`.

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
| `git push` asks for token every time | Credential helper not set | `git config --global credential.helper store` |
| `git push` rejected | Remote has commits you don't have | `git pull` first, then `git push` |
| `error: src refspec main does not match any` | No commits yet | Make at least one commit before pushing |
| Accidentally committed to `main` | Forgot to branch | Ask your administrator before pushing |

---

## Support

Contact your administrator if you are unable to connect or complete setup.
