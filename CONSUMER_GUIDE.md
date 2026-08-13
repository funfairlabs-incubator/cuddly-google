# Consumer Guide — Connecting to Your VM, Setting Up Claude Code, and Using GitHub

## Your Credentials

| User | Username | VM |
|---|---|---|
| Person 1 | `connlt1` | `vm-connlt1` |
| Person 2 | `connlt2` | `vm-connlt2` |
| Person 3 | `connlt3` | `vm-connlt3` |

> Passwords are distributed separately. Do not share them.

---

## Part 1 — Connecting to Your VM (Desktop / Laptop)

### What You Need

- An SSH client (see below)
- The **external IP address** of your VM — ask your administrator or find it in the [Google Cloud Console](https://console.cloud.google.com/compute/instances?project=poised-beach-505408-r2)

**SSH clients by platform:**
- **Windows 10 / 11**: SSH is built in — use PowerShell or Windows Terminal. Alternatively use [PuTTY](https://www.putty.org/)
- **macOS**: SSH is built in — use Terminal (`Cmd + Space`, type `Terminal`)
- **Linux**: SSH is built in — use your terminal emulator

### Connecting

#### Windows (PowerShell or Terminal)

```powershell
ssh connlt1@<YOUR_VM_EXTERNAL_IP>
```

Replace `connlt1` with your own username and `<YOUR_VM_EXTERNAL_IP>` with the IP address your administrator gave you. When prompted `Are you sure you want to continue connecting?` type `yes` and press Enter. Then enter your password.

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

### First Login

On first login you will see a Debian welcome message followed by a prompt:

```
Linux vm-connlt1 ...
...
connlt1@vm-connlt1:~$
```

You are now inside your VM.

### Changing Your Password

Change your password immediately after first login:

```bash
passwd
```

You will be prompted for your current password, then asked to enter and confirm a new one. Nothing appears on screen as you type — this is normal.

### Logging Out

```bash
exit
```

Or press `Ctrl+D`.

### Transferring Files

**Upload a file to your VM:**

```bash
scp /path/to/local/file connlt1@<YOUR_VM_EXTERNAL_IP>:~/
```

**Download a file from your VM:**

```bash
scp connlt1@<YOUR_VM_EXTERNAL_IP>:~/remote-file /path/to/local/destination/
```

---

## Part 2 — Connecting to Your VM (iOS and Android)

### Overview

You can connect to your VM from an iPhone or iPad (iOS/iPadOS) or an Android phone or tablet using an SSH client app. The connection details are the same as desktop — you need the VM's external IP address, your username, and your password.

A physical Bluetooth keyboard is strongly recommended for serious work. On-screen keyboards on mobile lack keys that terminals depend on (Tab, Escape, Ctrl sequences). All apps below provide workarounds, but a hardware keyboard removes the limitation entirely.

---

### iOS / iPadOS

#### Option A — Termius (Recommended)

**Cost:** Free to start. Pro subscription required for some features (approx. $10/month billed annually). The free tier is sufficient for basic SSH.

**Install:** Search **Termius** in the App Store, or find it at [apps.apple.com](https://apps.apple.com/us/app/termius-modern-ssh-client/id549039908). Developer: Termius Corporation.

**Connecting:**

1. Open Termius
2. Tap **Hosts** at the bottom of the screen
3. Tap the **+** button to add a new host
4. Fill in the following fields:
   - **Alias**: A label for this connection, e.g. `My VM`
   - **Hostname**: Your VM's external IP address
   - **Port**: `22`
   - **Username**: Your username (e.g. `connlt1`)
   - **Password**: Your password
5. Tap **Save** in the top right corner
6. Tap your new host in the Hosts list to connect
7. If a fingerprint warning appears, tap **Continue** — this is normal on first connection

**Keyboard notes:**
- Termius adds a row of keys above the standard keyboard including `Tab`, `Ctrl`, `Esc`, and arrow keys
- To send `Ctrl+C` (cancel a running command), tap `Ctrl` in the key row then tap `C` on the keyboard
- All standard terminal keys are accessible without an external keyboard

---

#### Option B — Blink Shell (Power Users)

**Cost:** Free with a Blink+ subscription ($19.99/year). 14-day free trial available.

**Install:** Search **Blink Shell** in the App Store. Developer: Blink Shell, Inc.

**Connecting:**

1. Open Blink Shell
2. Press `Cmd+,` (if on an external keyboard) or type `config` at the prompt to open Settings
3. Tap **Hosts**, then tap **+** to add a new host
4. Fill in:
   - **Host Name**: A short alias you will type to connect (e.g. `myvm`) — no spaces
   - **Hostname**: Your VM's external IP address
   - **Port**: `22`
   - **Username**: Your username
   - **Password**: Your password
5. Tap **Save**
6. Back at the terminal prompt, type `ssh myvm` (using the alias you set) and press Enter

**Keyboard notes:**
- Blink has excellent keyboard customisation — go to **Config > Keyboard** to map keys like Caps Lock to Escape
- Without customisation, Escape, Tab, and Ctrl require configuration on the software keyboard
- Blink is best used with a physical Bluetooth keyboard

---

#### Option C — ShellFish (SSH Files)

**Cost:** Free with ads. Pro removes ads and adds features (approx. $14.99/year or $29.99 lifetime).

**Install:** Search **SSH Files** or **ShellFish** in the App Store. Developer: Anders Borum.

**Connecting:**

1. Open SSH Files
2. Tap **+** to add a new server
3. Fill in:
   - **Address**: Your VM's external IP address
   - **Port**: `22`
   - **Username**: Your username
   - **Password**: Your password
4. Tap **Save**
5. Tap the server to connect

**Note:** ShellFish integrates with the iOS Files app, making it easy to browse and open remote files directly in apps like Working Copy, Textastic, or other document editors. The terminal is a secondary feature — use Termius or Blink if you primarily need a shell.

---

### Android

#### Option A — Termius (Recommended)

**Cost:** Free to start. Pro features require a subscription. The free tier covers basic SSH.

**Install:** Search **Termius** in the Google Play Store. Developer: Termius Corporation.

**Connecting:**

1. Open Termius
2. Tap the **SSH** tab
3. Tap **Add Connection** or the **+** button
4. Fill in:
   - **Label**: A name for this connection, e.g. `My VM`
   - **Address**: Your VM's external IP address
   - **Port**: `22`
   - **Username**: Your username
5. Tap the **Password / Key** toggle and select **Password**, then enter your password
6. Tap **Save and Connect**

**Keyboard notes:**
- Termius provides an extended keyboard row with `Ctrl`, `Alt`, `Tab`, `Esc`, and function keys
- If you cannot type `|` (pipe) or `\` (backslash), go to **Settings > Terminal** and disable **Use Option as Meta**
- A Bluetooth keyboard removes all keyboard limitations

---

#### Option B — ConnectBot (Free and Open Source)

**Cost:** Completely free. No account required. No subscription. Open source (Apache 2.0).

**Install:** Search **ConnectBot** in the Google Play Store, or install from [F-Droid](https://f-droid.org/packages/org.connectbot/). Developer: Kenny Root.

**Connecting:**

1. Open ConnectBot
2. In the connection field at the top, type your connection in this exact format:

   ```
   username@ip-address:port
   ```

   For example: `connlt1@34.105.100.200:22`

   If the port is 22, you can omit `:22`: `connlt1@34.105.100.200`

3. Tap the **arrow** or press Enter to connect
4. When asked to accept the host key, tap **Yes**
5. Enter your password when prompted

**Keyboard notes:**
- ConnectBot has very limited special-key support on the software keyboard — Tab, Escape, and function keys are difficult without a physical keyboard
- For basic command entry it works well; for editing files in vim or nano a Bluetooth keyboard is necessary
- ConnectBot is best suited to users who want a completely free, no-account option for straightforward terminal use

---

### Mobile Keyboard Tips (All Apps)

| Key needed | How to type it on mobile |
|---|---|
| `Tab` | Use the app's special key row, or Bluetooth keyboard |
| `Escape` | Use the app's special key row, or Bluetooth keyboard |
| `Ctrl+C` | Tap `Ctrl` in the key row, then tap `C` |
| `Ctrl+D` (logout) | Tap `Ctrl` in the key row, then tap `D` |
| `|` (pipe) | Use the extended keyboard row or long-press relevant key |
| Arrow keys | Available in all recommended apps' key rows |

---

## Part 3 — Setting Up Claude Code

These steps are performed inside your VM over SSH. Complete Part 1 or Part 2 first and ensure you are logged in before continuing.

### Step 1 — Install Prerequisites

Update the package list and install the required tools:

```bash
sudo apt update
sudo apt install -y curl git gnupg
```

When prompted for your password, enter the one you set in Part 1. When asked `Do you want to continue? [Y/n]` press Enter to accept.

Verify each tool installed correctly:

```bash
curl --version
git --version
```

Both commands should print a version number. If either prints `command not found`, the install failed — re-run the `apt install` line above.

### Step 2 — Install Claude Code

Run the official installer:

```bash
curl -fsSL https://claude.ai/install.sh | bash
```

This downloads and installs a pre-built Claude Code binary into `~/.local/bin/`. Wait for it to complete — you will see output ending with an install success message.

### Step 3 — Add Claude Code to Your PATH

The installer places `claude` in `~/.local/bin/`. You need to tell your shell where to find it:

```bash
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

### Step 4 — Verify the Installation

```bash
claude --version
```

This should print a version number such as `2.1.211 (Claude Code)`. If you see `command not found`, go back and repeat Step 3, then try again.

Run the built-in diagnostics:

```bash
claude doctor
```

Review the output. All checks should pass. Note any warnings — they may indicate missing dependencies.

### Step 5 — Configure Git

Claude Code requires Git to be configured with a name and email before it will work. Run:

```bash
git config --global user.name "Your Name"
git config --global user.email "your@email.com"
```

Replace `Your Name` and `your@email.com` with your own details. These are used to identify your commits and are stored in `~/.gitconfig`.

Verify:

```bash
git config --global --list
```

You should see `user.name` and `user.email` in the output.

### Step 6 — Authenticate

You need either a **Claude.ai account** (Pro, Max, Team, or Enterprise) or an **Anthropic Console API key**. Choose one method below.

---

#### Method A — Claude.ai Account Login (Recommended)

This method works over SSH even though there is no browser on the VM. Claude Code will display a URL that you open in a browser **on your local machine**.

1. On the VM, run:

   ```bash
   claude
   ```

2. Claude Code will display a URL in the terminal, for example:

   ```
   Visit this URL to log in:
   https://claude.ai/authorize?code=XXXXXXXXXX
   ```

3. **On your local machine** (not the SSH session), open that URL in any web browser

4. Sign in to your Claude.ai account and approve the access request

5. The browser will show a short authorisation code

6. **Copy that code** and paste it back into the SSH terminal where Claude Code is waiting, then press Enter

7. The terminal will show `Login successful`

Your credentials are stored in `~/.claude/.credentials.json` and will persist across all future SSH sessions automatically.

---

#### Method B — API Key

If you have an Anthropic Console API key (format: `sk-ant-...`):

1. Add the key to your shell profile so it is set automatically on every login:

   ```bash
   echo 'export ANTHROPIC_API_KEY="sk-ant-your-key-here"' >> ~/.bashrc
   source ~/.bashrc
   ```

   Replace `sk-ant-your-key-here` with your actual key.

2. Verify it is set:

   ```bash
   echo $ANTHROPIC_API_KEY
   ```

   Your key should be printed. If it is blank, re-run the `source ~/.bashrc` line.

3. Run Claude Code:

   ```bash
   claude
   ```

   When prompted to approve use of the API key, type `yes` and press Enter.

---

### Step 7 — Start Using Claude Code

Once authenticated, you can start Claude Code in any directory:

```bash
claude
```

This opens an interactive session. Type your request in plain English and press Enter.

To run a one-off command without entering the interactive session:

```bash
claude -p "your question or task here"
```

To exit the interactive session type `/exit` or press `Ctrl+C`.

---

## Part 4 — Using GitHub

GitHub is a platform for storing, sharing, and collaborating on code. Git is the version control tool that runs locally on your VM; GitHub is the remote service where your code is stored. You use Git commands on the VM to push and pull code to and from GitHub.

### Step 1 — Create a GitHub Account

If you do not already have one:

1. Go to [https://github.com](https://github.com)
2. Click **Sign up**
3. Enter your email address, create a password, and choose a username
4. Verify your email address when prompted
5. You now have a free GitHub account

### Step 2 — Create a Personal Access Token (PAT)

GitHub no longer accepts your account password for command-line operations. You must create a Personal Access Token (PAT) and use it as your password when running Git commands on the VM.

1. Sign in to GitHub at [https://github.com](https://github.com)
2. Click your profile picture in the top-right corner, then click **Settings**
3. Scroll down the left sidebar and click **Developer settings**
4. Click **Personal access tokens**, then click **Tokens (classic)**
5. Click **Generate new token**, then click **Generate new token (classic)**
6. Fill in:
   - **Note**: A label to remind you what this token is for, e.g. `VM access`
   - **Expiration**: Choose a duration (90 days is a reasonable starting point)
   - **Select scopes**: Tick **repo** (this gives full access to your repositories)
7. Scroll to the bottom and click **Generate token**
8. **Copy the token immediately** — it will only be shown once. It starts with `ghp_`.

Store this token somewhere safe (e.g. a password manager). If you lose it, you will need to generate a new one.

### Step 3 — Save Your GitHub Credentials on the VM

To avoid typing your token every time you push or pull, configure Git to cache it:

```bash
git config --global credential.helper store
```

The first time you run a Git command that requires authentication (e.g. `git push`), Git will ask for your GitHub username and your token (as the password). After that it will remember them automatically in `~/.git-credentials`.

### Step 4 — Clone a Repository

Cloning downloads a copy of a GitHub repository onto your VM.

1. Go to the repository on GitHub
2. Click the green **Code** button
3. Ensure **HTTPS** is selected
4. Copy the URL shown (it looks like `https://github.com/username/repo-name.git`)

On your VM, run:

```bash
git clone https://github.com/username/repo-name.git
```

Replace the URL with the one you copied. Git will create a folder named `repo-name` in your current directory containing all the files.

Move into the folder:

```bash
cd repo-name
```

### Step 5 — Understand the Basic Workflow

Every time you work on code, the cycle is: **pull → make changes → add → commit → push**.

#### Check the current status

```bash
git status
```

This shows which files have been changed, added, or deleted since your last commit.

#### Pull the latest changes from GitHub

Before starting work, always pull to make sure you have the latest version:

```bash
git pull
```

This downloads any changes made by others and merges them into your local copy.

#### Stage your changes

When you have made changes to files and want to save them, first stage the files you want to include:

```bash
git add filename.txt
```

To stage all changed files at once:

```bash
git add .
```

#### Commit your changes

A commit saves a snapshot of your staged changes with a description:

```bash
git commit -m "A short description of what you changed"
```

The message should describe what you did, for example: `"Fix typo in README"` or `"Add login page"`.

#### Push your changes to GitHub

```bash
git push
```

This uploads your commits to GitHub. If prompted for credentials, enter your GitHub username and your PAT (token) as the password.

### Step 6 — Working with Branches

A branch is an independent copy of the code where you can make changes without affecting the main version. This is good practice — always work on a branch, then merge it into `main` when ready.

#### Create and switch to a new branch

```bash
git checkout -b my-feature-branch
```

Replace `my-feature-branch` with a short descriptive name, e.g. `add-login-page` or `fix-header-bug`. No spaces — use hyphens.

#### See which branch you are on

```bash
git branch
```

The current branch has a `*` next to it.

#### Push a new branch to GitHub

The first time you push a new branch, tell Git where to send it:

```bash
git push -u origin my-feature-branch
```

After this first push, you can use `git push` as normal.

#### Switch back to the main branch

```bash
git checkout main
```

### Step 7 — Create a Pull Request

A pull request (PR) asks someone to review your branch and merge it into `main`. This is done on the GitHub website, not the command line.

1. Push your branch to GitHub (see Step 6 above)
2. Go to your repository on [https://github.com](https://github.com)
3. GitHub will show a yellow banner: **"Your branch had recent pushes"** — click **Compare & pull request**
4. Fill in:
   - **Title**: A short summary of what your branch does
   - **Description**: More detail about the changes if needed
5. Click **Create pull request**
6. If you have collaborators, they can now review, comment, and approve
7. Once approved (or if you are working alone), click **Merge pull request**, then **Confirm merge**

Your changes are now in `main`. Switch back locally and pull:

```bash
git checkout main
git pull
```

### Step 8 — Common Situations

**Undo changes to a file before staging:**

```bash
git restore filename.txt
```

**See what changed in a file:**

```bash
git diff filename.txt
```

**See the history of commits:**

```bash
git log --oneline
```

Press `q` to exit the log view.

**Pull changes from a specific branch:**

```bash
git pull origin branch-name
```

**Delete a local branch after merging:**

```bash
git branch -d my-feature-branch
```

---

## Troubleshooting

| Problem | Likely Cause | Fix |
|---|---|---|
| `Connection refused` on SSH | VM is stopped or SSH not configured | Ask administrator to start the VM |
| `Permission denied` on SSH | Wrong username or password | Double-check credentials |
| `Connection timed out` on SSH | Firewall or wrong IP | Confirm the external IP with your administrator |
| Mobile SSH disconnects frequently | App going to background | Keep the app in the foreground; some apps support keeping the session alive in settings |
| `command not found: claude` | PATH not set | Run `source ~/.bashrc` then try again |
| `curl: command not found` | curl not installed | Run `sudo apt install -y curl` |
| `git: command not found` | git not installed | Run `sudo apt install -y git` |
| Claude Code installer fails | No internet on VM | Ask administrator to check network/firewall |
| Login URL does not work | Browser/account issue | Try a different browser or check you are signed in to claude.ai |
| `permission denied` on `~/.claude/` | Wrong file permissions | Run `chmod 700 ~/.claude/ && chmod 600 ~/.claude/.credentials.json` |
| Claude Code cannot find files | ripgrep missing | Run `sudo apt install -y ripgrep` |
| `git push` asks for password every time | Credential helper not set | Run `git config --global credential.helper store` |
| `git push` rejected | Remote has changes you don't have | Run `git pull` first, then `git push` |
| `error: src refspec main does not match any` | No commits yet | Make at least one commit before pushing |
| Accidentally committed to `main` | Forgot to create a branch | Ask your administrator or a colleague for help before pushing |

---

## Support

Contact your administrator if you are unable to connect or complete setup.
