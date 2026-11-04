# CauseMic GitHub access (SSH key in 1Password)

**For:** any CauseMic team member who needs to clone and push to repos in the **CauseMic-Hub** GitHub organization, on **Mac, Windows or Linux**.
**Time:** about 20–30 minutes.
**First repo:** `CauseMic-Hub/south-baltimore-gateway-partnership-SBGP` (SBGP).

> **Tested on:** Mac (macOS, 1Password 8). **Windows and Linux steps are written from 1Password's and GitHub's documented setup but haven't yet been run end to end at CauseMic.** If you're the first on Windows or Linux, tell Jen what differed so this guide can be updated.

---

## How to use this guide

Upload or paste this file into Claude and say: **"Walk me through this, one step at a time."**

You'll run every command yourself, in a terminal window. Claude explains each step, tells you what you should see, and helps if something looks different.

---

## Instructions for Claude (guiding this setup)

You're walking a CauseMic team member through this setup. They may not be comfortable with a terminal. Follow these rules:

1. **Ask their operating system first** (Step 1), then **show only the instructions for that system.** Where a step has **Mac / Windows / Linux** sections, show only theirs. Never show all three.
2. **One step at a time.** Show only the current step. Wait until they confirm it's done, or paste the output, before moving on. Don't preview later steps.
3. **Every command gets an "Expect:" line.** Say exactly what should appear, including "nothing prints" when that's normal. Silent commands look broken to people new to the terminal, and they often run them again.
4. **Check pasted output against the Expect line.** If it matches, say so and move on. If it doesn't, use the troubleshooting notes in that step before going further.
5. **Never ask for secrets.** Don't ask for, or accept, their passphrase, their private key (the file without `.pub`), or any password. If they paste a private key into the chat, tell them to delete the key and start Step 3 again with a new one.
6. **Fill in their details.** In Step 1 you collect their GitHub username, work email and name. Then replace `<github-username>`, `<work-email>` and `<full-name>` in every command before showing it. Never show a placeholder they'd have to edit.
7. **Add, never replace.** Nothing in this guide should delete or overwrite an existing file or setting. If a check shows something already exists (a key, an SSH config entry, a Git setting), stop and work out with them how to add alongside it.
8. **One command per code block,** so each can be copied on its own.
9. **Use their system's words:** ⌘ on Mac and Ctrl on Windows and Linux; Touch ID, Windows Hello, or the system password for the 1Password approval prompt.
10. Keep the tone plain and encouraging. Explain *why* only when it helps them get through the step.

---

## Step 1: Gather your details and check prerequisites

Claude asks for:

- **Which computer you're using:** Mac, Windows, or Linux.
- **Your GitHub username.** You'll use your own GitHub account; CauseMic doesn't create one for you.
- **Your CauseMic email** (e.g. name@causemic.com).
- **Your full name,** as you want it to appear on your work in GitHub.
- **Do you also do Git work for another company or for personal projects on this computer?** (Yes/No.) Step 9 depends on this answer.

Confirm these before going on:

- [ ] You've **accepted the invite** to the CauseMic-Hub organization on GitHub. Check the email from GitHub, or visit github.com/CauseMic-Hub while signed in.
- [ ] You can open the SBGP repo in your browser: github.com/CauseMic-Hub/south-baltimore-gateway-partnership-SBGP
- [ ] The **1Password desktop app** is installed and you're signed in. The browser extension alone isn't enough.
- [ ] **Git is installed:**
  - **Mac:** nothing to do. If a command below says Git is missing, macOS offers to install it; accept.
  - **Windows:** install **Git for Windows** from git-scm.com (the default options are fine).
  - **Linux:** install `git` and `openssh-client` with your package manager if they aren't already there.
- [ ] You know how to open a terminal:
  - **Mac:** ⌘ Space, type *Terminal*, press Enter.
  - **Windows:** Start menu → type *PowerShell* → open **Windows PowerShell** (or **Terminal**). Use PowerShell for every Windows command in this guide.
  - **Linux:** your usual terminal app.

**Troubleshooting:**
- "404" on the SBGP repo means the invite hasn't been accepted or access hasn't been granted. Ask Jen.

---

## Step 2: Look at what's already on your computer (read-only)

These commands only *look*. They don't change anything. Paste the output to Claude.

**Mac / Linux:**
```bash
ls -la ~/.ssh
```
**Expect:** a list of files, or `No such file or directory`. Both are fine.

```bash
cat ~/.ssh/config
```
**Expect:** some text, or `No such file or directory`. Both are fine.

**Windows (PowerShell):**
```powershell
Get-ChildItem $HOME\.ssh -Force
```
**Expect:** a list of files, or an error saying the path doesn't exist. Both are fine.

```powershell
Get-Content $HOME\.ssh\config
```
**Expect:** some text, or an error saying the path doesn't exist. Both are fine.

**All systems:**
```bash
git config --global user.email
```
**Expect:** an email address, or nothing at all. Both are fine.

**Claude, note for later:**
- If `id_ed25519_causemic` already exists, don't overwrite it in Step 3. Ask whether they've done this before, and pick a different file name if needed.
- If the SSH config already has a `Host github-causemic` section, skip adding it in Step 6 and check what's there instead.
- If the SSH config has a `Host *` section mentioning 1Password (`IdentityAgent`), that's fine. Note it for Step 7.
- The global email tells you what Git uses today. Step 9 adds a CauseMic-specific email without changing it.

---

## Step 3: Create your CauseMic SSH key

An SSH key is how your computer proves to GitHub that it's you, without a password. It comes in two halves:
- a **private** half that never leaves your computer and 1Password;
- a **public** half that you give to GitHub.

**Mac / Linux:**
```bash
ssh-keygen -t ed25519 -C "<work-email>" -f ~/.ssh/id_ed25519_causemic
```

**Windows (PowerShell):**
```powershell
ssh-keygen -t ed25519 -C "<work-email>" -f "$HOME\.ssh\id_ed25519_causemic"
```
*(If Windows says the `.ssh` folder doesn't exist, first run `New-Item -ItemType Directory -Force "$HOME\.ssh"`. **Expect:** a line describing the new folder.)*

It asks three things:

1. **"Enter passphrase"**: make one up and type it. **Nothing appears as you type. That's normal.** Press Enter.
2. **"Enter same passphrase again"**: type it again and press Enter.
3. **If it asks "Overwrite (y/n)?", type `n`** and tell Claude. That means a key with this name already exists.

**Expect:** "Your identification has been saved in …id_ed25519_causemic", then "Your public key has been saved in …id_ed25519_causemic.pub", then a fingerprint line starting `SHA256:` and a small box of random characters. You can ignore the box.

**Remember the passphrase.** 1Password asks for it once in the next step.

---

## Step 4: Save the key in 1Password

This puts the private key in your **own** 1Password, so 1Password can unlock it with your fingerprint, face or system password.

1. In the 1Password app: **New Item → SSH Key**.
2. Choose **Import** (it may say "Add Private Key" → "Import a Key File").
3. Go to your `.ssh` folder in the file picker:
   - **Mac:** press **⌘ ⇧ G**, type `~/.ssh`, press Enter. (The folder is hidden; ⌘ ⇧ . also shows hidden files.)
   - **Windows:** type `%USERPROFILE%\.ssh` in the file picker's address bar and press Enter.
   - **Linux:** press **Ctrl H** to show hidden files, then open `.ssh` in your home folder.
4. Select **`id_ed25519_causemic`**: the one **without** `.pub`. Open it.
5. Enter the passphrase from Step 3.
6. Name the item **GitHub – CauseMic**.
7. Save it in your **private / employee vault**, **not** a shared vault. This key proves you are you, and teammates should never use it.

**Expect:** a new SSH Key item in 1Password, showing a public key and a fingerprint (`SHA256:…`).

---

## Step 5: Add the public key to GitHub

1. In your browser, **make sure you're signed in to GitHub as `<github-username>`**.
2. Click your profile photo (top right) → **Settings** → **SSH and GPG keys** (left sidebar) → **New SSH key**.
3. **Title:** `<full-name> – CauseMic`
4. **Key type:** Authentication Key.
5. **Key:** click into the box.
   - If 1Password offers to fill it, choose the **GitHub – CauseMic** item. It may fill the key without the email at the end. That's fine: the email is just a label.
   - Or copy it from the terminal, then paste (⌘V or Ctrl V):
     - **Mac:**
       ```bash
       pbcopy < ~/.ssh/id_ed25519_causemic.pub
       ```
       **Expect:** nothing prints. The key is on your clipboard.
     - **Windows:**
       ```powershell
       Get-Content "$HOME\.ssh\id_ed25519_causemic.pub" | Set-Clipboard
       ```
       **Expect:** nothing prints. The key is on your clipboard.
     - **Linux:**
       ```bash
       cat ~/.ssh/id_ed25519_causemic.pub
       ```
       **Expect:** one line starting `ssh-ed25519`. Select and copy the whole line.
6. Click **Add SSH key**. GitHub may ask you to confirm with your password or a code.

**Check that it's the same key:**

- **Mac / Linux:**
  ```bash
  ssh-keygen -lf ~/.ssh/id_ed25519_causemic.pub
  ```
- **Windows:**
  ```powershell
  ssh-keygen -lf "$HOME\.ssh\id_ed25519_causemic.pub"
  ```

**Expect:** one line like `256 SHA256:AbC123… <work-email> (ED25519)`.

The `SHA256:…` part should **match** the fingerprint GitHub now shows for the key, and the one in 1Password. If GitHub's is different, 1Password may have filled a different key. Delete it on GitHub and paste using the copy method instead.

---

## Step 6: Give the key a nickname on your computer

This tells your computer: *"When I connect to `github-causemic`, go to GitHub and use the CauseMic key from 1Password, and only that key."*

**Run the block for your system exactly once.** It adds to the file (creating it if needed), and running it twice adds a duplicate.

**Mac:**
```bash
cat >> ~/.ssh/config <<'EOF'

# CauseMic GitHub - key served by 1Password
Host github-causemic
  HostName github.com
  User git
  IdentityAgent "~/Library/Group Containers/2BUA8C4S2C.com.1password/t/agent.sock"
  IdentityFile ~/.ssh/id_ed25519_causemic.pub
  IdentitiesOnly yes
EOF
```
**Expect:** nothing prints. That's success.

```bash
chmod 600 ~/.ssh/config
```
**Expect:** nothing prints. (This makes the file readable only by you.)

**Linux:**
```bash
cat >> ~/.ssh/config <<'EOF'

# CauseMic GitHub - key served by 1Password
Host github-causemic
  HostName github.com
  User git
  IdentityAgent ~/.1password/agent.sock
  IdentityFile ~/.ssh/id_ed25519_causemic.pub
  IdentitiesOnly yes
EOF
```
**Expect:** nothing prints. That's success.

```bash
chmod 600 ~/.ssh/config
```
**Expect:** nothing prints.

**Windows (PowerShell):**
```powershell
Add-Content -Path "$HOME\.ssh\config" -Value @"

# CauseMic GitHub - key served by 1Password
Host github-causemic
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519_causemic.pub
  IdentitiesOnly yes
"@
```
**Expect:** nothing prints. That's success.

*(On Windows there's no `IdentityAgent` line. 1Password takes over Windows' built-in SSH agent instead; see Step 7.)*

**Check it:**
- **Mac / Linux:** `cat ~/.ssh/config`
- **Windows:** `Get-Content "$HOME\.ssh\config"`

**Expect:** the `Host github-causemic` section appears **once**. If it appears more than once, tell Claude, who will help you remove the extras.

---

## Step 7: Turn on the 1Password SSH agent

**All systems:**

1. 1Password → **Settings** → **Developer**.
2. Turn on **"Use the SSH agent."**
3. If it asks **"Allow 1Password to save SSH key names to disk?"**, choose **Use Key Names**. Only the item *title* is saved, not the key, and the approval prompt then tells you which key is being used.
4. If it offers to **"Edit automatically"** or shows a config **snippet** to copy, **skip it.** Step 6 already pointed only the CauseMic connection at 1Password. The snippet would route *every* SSH connection on your computer through 1Password.

**Windows only, before the agent works:** Windows has its own built-in SSH agent, and it must be turned off so 1Password can take its place. 1Password shows this when you turn on the agent. If it doesn't:

1. Start menu → type *Services* → open it.
2. Find **OpenSSH Authentication Agent** → right-click → **Properties**.
3. Set **Startup type** to **Disabled**, click **Stop** if it's running, then **OK**.
4. Turn 1Password's **Use the SSH agent** off and on again.

**Windows only, so Git uses that agent:** Git for Windows comes with its own copy of SSH, which doesn't talk to 1Password. Point CauseMic Git work at Windows' SSH instead. This only applies inside your CauseMic folder; it's set up in Step 9 (9c).

**Expect:** the setting shows as on. There's nothing to run in the terminal for this step, apart from the Windows service change.

---

## Step 8: Test the connection

**All systems:**
```bash
ssh -T git@github-causemic
```

**Expect, in order:**
1. **The first time only:** "The authenticity of host 'github.com' can't be established… ED25519 key fingerprint is SHA256:+DiY3wvvV6TuJJhbpZisF/zLDA0zPMSvHdkr4UvCOqU. Are you sure you want to continue connecting (yes/no)?"
   - If the fingerprint matches exactly, type `yes` and press Enter. (That's GitHub's published fingerprint.)
   - If it **doesn't** match, type `no` and tell Claude.
2. **A 1Password prompt** asking to allow the key **GitHub – CauseMic**. Approve it with Touch ID, Windows Hello, or your system password.
3. **"Hi <github-username>! You've successfully authenticated, but GitHub does not provide shell access."** The second half is normal. **This line means success.**

**Troubleshooting:**
- `Permission denied (publickey)`:
  - The key isn't on GitHub, or it's on a different GitHub account. Recheck Step 5 while signed in as `<github-username>`.
  - Or the 1Password agent is off (Step 7). On Windows, check that the OpenSSH Authentication Agent service is disabled.
  - Or the key isn't in your private vault. 1Password's agent only serves keys from certain vaults by default; check Settings → Developer.
- **No 1Password prompt appears:** make sure the 1Password app is open and unlocked, then try again.
- **"Hi" shows a different username:** the key was added to the wrong GitHub account. Remove it there and add it to the right one (Step 5).
- `Could not resolve hostname github-causemic`: the Step 6 section is missing or mistyped. Show the config file (Step 6 check) and compare.
- **Windows: `ssh` isn't recognized:** Windows' built-in OpenSSH client is off. Settings → System → Optional features → add **OpenSSH Client**, then open a new PowerShell window.

---

## Step 9: Make your work show up under your CauseMic email

Git stamps every change you save ("commit") with a name and email. This step sets your CauseMic name and email for your CauseMic folder only.

### 9a. Create your CauseMic folder

Recommended: a folder just for CauseMic repos, named `CauseMic` in your home folder.

**Mac / Linux:**
```bash
mkdir -p ~/CauseMic
```
**Expect:** nothing prints. If the folder already exists, nothing happens; that's fine.

**Windows:**
```powershell
New-Item -ItemType Directory -Force "$HOME\CauseMic"
```
**Expect:** a short table describing the folder (Mode, LastWriteTime, Name `CauseMic`).

*(Claude: if they already keep work in a different folder and want to use that, use their path everywhere below instead. Keep the trailing slash in the Git rule in 9d.)*

### 9b. Back up your current Git settings

**Mac / Linux:**
```bash
cp ~/.gitconfig ~/.gitconfig.bak-before-causemic
```
**Expect:** nothing prints. If you see `No such file or directory`, there were no settings to back up; continue.

**Windows:**
```powershell
Copy-Item "$HOME\.gitconfig" "$HOME\.gitconfig.bak-before-causemic"
```
**Expect:** nothing prints. If it says the path doesn't exist, there were no settings to back up; continue.

### 9c. Create your CauseMic identity file

**Mac / Linux:**
```bash
git config --file ~/.gitconfig-causemic user.name "<full-name>"
```
**Expect:** nothing prints.

```bash
git config --file ~/.gitconfig-causemic user.email "<work-email>"
```
**Expect:** nothing prints.

**Windows:**
```powershell
git config --file "$HOME\.gitconfig-causemic" user.name "<full-name>"
```
**Expect:** nothing prints.

```powershell
git config --file "$HOME\.gitconfig-causemic" user.email "<work-email>"
```
**Expect:** nothing prints.

Also tell CauseMic Git work to use Windows' SSH (the one 1Password works with):
```powershell
git config --file "$HOME\.gitconfig-causemic" core.sshCommand "C:/Windows/System32/OpenSSH/ssh.exe"
```
**Expect:** nothing prints.

### 9d. Use it inside the CauseMic folder

**Mac / Linux:**
```bash
git config --global includeIf."gitdir:~/CauseMic/".path ~/.gitconfig-causemic
```
**Expect:** nothing prints.

**Windows:**
```powershell
git config --global includeIf."gitdir/i:~/CauseMic/".path ~/.gitconfig-causemic
```
**Expect:** nothing prints. (`gitdir/i` ignores upper/lower case, which Windows folder names need.)

This *adds* a rule. It doesn't change your existing Git settings. Outside your CauseMic folder, Git keeps working exactly as before. That's what keeps CauseMic work separate from any other work on this computer.

*(Claude: if they answered "No" to other Git work in Step 1 and Step 2 showed no global email, this is still the recommended setup, so do it the same way.)*

---

## Step 10: Copy the SBGP repo to your computer ("clone")

**Mac / Linux:**
```bash
cd ~/CauseMic
```
**Windows:**
```powershell
cd "$HOME\CauseMic"
```
**Expect:** nothing prints. Your prompt may now show `CauseMic`.

**Mac / Linux:**
```bash
git clone git@github-causemic:CauseMic-Hub/south-baltimore-gateway-partnership-SBGP.git
```

**Windows:** the CauseMic folder rule from Step 9 only applies *inside* a repo, so the first clone names Windows' SSH directly:
```powershell
git -c core.sshCommand="C:/Windows/System32/OpenSSH/ssh.exe" clone git@github-causemic:CauseMic-Hub/south-baltimore-gateway-partnership-SBGP.git
```

**Expect:**
1. `Cloning into 'south-baltimore-gateway-partnership-SBGP'...`
2. A **1Password approval prompt**. Approve it.
3. Lines like `Receiving objects: 100% … done.`

**Always copy CauseMic repos using an address that starts with `git@github-causemic:`**, not the `https://github.com/...` address GitHub shows by default. On GitHub, the green **Code** button → **SSH** tab shows `git@github.com:CauseMic-Hub/...`. Replace `github.com` with `github-causemic`.

**Troubleshooting:**
- `Repository not found`: access to the repo hasn't been granted, or the invite isn't accepted yet. Ask Jen.
- `Permission denied (publickey)`: go back to Step 8. On Windows, also check the clone command includes the `-c core.sshCommand=…` part.

---

## Step 11: Final checks (this proves it all works)

**Mac / Linux:**
```bash
cd ~/CauseMic/south-baltimore-gateway-partnership-SBGP
```
**Windows:**
```powershell
cd "$HOME\CauseMic\south-baltimore-gateway-partnership-SBGP"
```
**Expect:** nothing prints.

**All systems:**
```bash
git config user.email
```
**Expect:** exactly `<work-email>`.

```bash
git remote -v
```
**Expect:** two lines, both containing `git@github-causemic:CauseMic-Hub/south-baltimore-gateway-partnership-SBGP.git`, one ending `(fetch)` and one ending `(push)`.

**Windows only:**
```powershell
git config core.sshCommand
```
**Expect:** `C:/Windows/System32/OpenSSH/ssh.exe`.

If they do other Git work on this computer, also check that it's unchanged:

**Mac / Linux:**
```bash
cd ~
```
**Windows:**
```powershell
cd $HOME
```
**Expect:** nothing prints.

```bash
git config --global user.email
```
**Expect:** the same email Step 2 showed (or nothing, if it showed nothing). It should **not** have changed.

**🎉 You're set up.** From now on:
- open CauseMic repos from inside your CauseMic folder;
- clone new CauseMic repos with the `git@github-causemic:` address (on Windows, the first clone of each repo needs the `-c core.sshCommand=…` part from Step 10);
- expect a 1Password approval prompt whenever your computer talks to GitHub for CauseMic. **Only approve it when you're the one doing something with GitHub.**

---

## Good to know

- **Nothing secret is in this guide.** Your key's private half lives only in your `.ssh` folder and your 1Password.
- **Commands that print nothing usually worked.** Problems show up as messages starting with `error:`, `fatal:`, or `Permission denied`.
- **Pull requests:** open them in the GitHub website for now.
- **If you change computers:** 1Password already has your key. On the new computer, Steps 6–11 are what you'd redo, plus Step 2's checks.

## How to undo (if ever needed)

- Remove the `Host github-causemic` section from your SSH config file (`.ssh/config` in your home folder).
- Remove the `includeIf` section from your Git settings file (`.gitconfig` in your home folder), or restore `.gitconfig.bak-before-causemic`, and delete `.gitconfig-causemic`.
- Remove the key from GitHub (Settings → SSH and GPG keys) and from 1Password.
- **Windows:** if you disabled the OpenSSH Authentication Agent service only for this setup and want it back, set its Startup type to Manual or Automatic.
