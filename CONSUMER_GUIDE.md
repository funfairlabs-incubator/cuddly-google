# Consumer Guide — Connecting to Your VM and Setting Up Claude Code

## Your Credentials

| User | Username | VM |
|---|---|---|
| Person 1 | `connlt1` | `vm-connlt1` |
| Person 2 | `connlt2` | `vm-connlt2` |
| Person 3 | `connlt3` | `vm-connlt3` |

> Passwords are distributed separately. Do not share them.

---

## Part 1 — Connecting to Your VM

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

## Part 2 — Setting Up Claude Code

These steps are performed inside your VM over SSH. Complete Part 1 first and ensure you are logged in before continuing.

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

To exit the interactive session:

```
/exit
```

Or press `Ctrl+C`.

---

## Troubleshooting

| Problem | Likely Cause | Fix |
|---|---|---|
| `Connection refused` on SSH | VM is stopped or SSH not configured | Ask administrator to start the VM |
| `Permission denied` on SSH | Wrong username or password | Double-check credentials |
| `Connection timed out` on SSH | Firewall or wrong IP | Confirm the external IP with your administrator |
| `command not found: claude` | PATH not set | Run `source ~/.bashrc` then try again |
| `curl: command not found` | curl not installed | Run `sudo apt install -y curl` |
| `git: command not found` | git not installed | Run `sudo apt install -y git` |
| Claude Code installer fails | No internet on VM | Ask administrator to check network/firewall |
| Login URL does not work | Browser/account issue | Try a different browser or check you are signed in to claude.ai |
| `permission denied` on `~/.claude/` | Wrong file permissions | Run `chmod 700 ~/.claude/ && chmod 600 ~/.claude/.credentials.json` |
| Claude Code cannot find files | ripgrep missing | Run `sudo apt install -y ripgrep` |

---

## Support

Contact your administrator if you are unable to connect or complete setup.
