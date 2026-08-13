# Consumer Guide — Connecting to Your VM

This guide explains how to connect to your dedicated virtual machine using SSH.

## Your Credentials

| User | Username | VM |
|---|---|---|
| Person 1 | `connlt1` | `vm-connlt1` |
| Person 2 | `connlt2` | `vm-connlt2` |
| Person 3 | `connlt3` | `vm-connlt3` |

> Passwords are distributed separately. Do not share them.

## Prerequisites

You need an SSH client:
- **Windows**: Use [PuTTY](https://www.putty.org/), Windows Terminal, or PowerShell (SSH built-in from Windows 10+)
- **macOS / Linux**: SSH is built in — use Terminal

You also need the **external IP address** of your VM. Ask your administrator or find it in the [Google Cloud Console](https://console.cloud.google.com/compute/instances?project=poised-beach-505408-r2).

## Connecting via SSH

### Windows (PowerShell / Terminal)

```powershell
ssh connlt1@<YOUR_VM_EXTERNAL_IP>
```

Replace `connlt1` with your own username and `<YOUR_VM_EXTERNAL_IP>` with the IP address provided by your administrator.

When prompted, enter your password.

### macOS / Linux (Terminal)

```bash
ssh connlt1@<YOUR_VM_EXTERNAL_IP>
```

When prompted, enter your password.

### PuTTY (Windows GUI)

1. Open PuTTY
2. Enter your VM's external IP in the **Host Name** field
3. Set **Port** to `22`
4. Click **Open**
5. Log in with your username and password when prompted

## First Login

On first login you'll see a Debian welcome message. You are now inside your VM shell.

```
Linux vm-connlt1 ...
...
connlt1@vm-connlt1:~$
```

## Changing Your Password

You are strongly encouraged to change your password after first login:

```bash
passwd
```

Follow the prompts to set a new password.

## Transferring Files

### Upload a file to your VM

```bash
scp /path/to/local/file connlt1@<YOUR_VM_EXTERNAL_IP>:~/
```

### Download a file from your VM

```bash
scp connlt1@<YOUR_VM_EXTERNAL_IP>:~/remote-file /path/to/local/destination/
```

## Logging Out

Type `exit` or press `Ctrl+D` to end your SSH session.

## Troubleshooting

| Problem | Likely Cause | Fix |
|---|---|---|
| `Connection refused` | VM is stopped or SSH not yet configured | Ask administrator to start VM and run setup |
| `Permission denied` | Wrong username or password | Double-check credentials |
| `Connection timed out` | Firewall issue or wrong IP | Confirm the external IP with your administrator |

## Support

Contact your administrator if you are unable to connect.
