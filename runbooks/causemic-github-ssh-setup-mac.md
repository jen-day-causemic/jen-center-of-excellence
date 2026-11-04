# CauseMic GitHub access on a Mac (SSH key in 1Password)

**For:** CauseMic team members on a MacBook who need to clone and push to repos in the **CauseMic-Hub** GitHub organization.
**Time:** about 20–30 minutes.
**First repo:** `CauseMic-Hub/south-baltimore-gateway-partnership-SBGP` (SBGP).

---

## How to use this guide

Upload or paste this file into Claude and say: **"Walk me through this, one step at a time."**

You'll run every command yourself in the Mac **Terminal** app. Claude explains each step, tells you what you should see, and helps if something looks different.

---

## Instructions for Claude (guiding this setup)

You're walking a CauseMic team member through this setup. They may not be comfortable with the Terminal. Follow these rules:

1. **One step at a time.** Show only the current step. Wait until they confirm it's done, or paste the output, before moving on. Don't preview later steps.
2. **Every command gets an "Expect:" line.** Say exactly what should appear, including "nothing prints" when that's normal. Silent commands look broken to people new to the Terminal, and they often run them again.
3. **Check pasted output against the Expect line.** If it matches, say so and move on. If it doesn't, use the troubleshooting notes in that step before going further.
4. **Never ask for secrets.** Don't ask for, or accept, their passphrase, their private key (the file without `.pub`), or any password. If they paste a private key into the chat, tell them to delete the key and start Step 3 again with a new one.
5. **Fill in their details.** In Step 1 you collect their GitHub username, work email and name. Then replace `<github-username>`, `<work-email>` and `<full-name>` in every command before showing it. Never show a placeholder they'd have to edit.
6. **Add, never replace.** Nothing in this guide should delete or overwrite an existing file or setting. If a check shows something already exists (a key, an SSH config entry, a Git setting), stop and work out with them how to add alongside it.
7. **One command per code block,** so each can be copied on its own.
8. Keep the tone plain and encouraging. Explain *why* only when it helps them get through the step.

---

## Step 1: Gather your details and check prerequisites

Claude asks for:

- **Your GitHub username.** You'll use your own GitHub account; CauseMic doesn't create one for you.
- **Your CauseMic email** (e.g. name@causemic.com).
- **Your full name,** as you want it to appear on your work in GitHub.
- **Do you also do Git work for another company or for personal projects on this Mac?** (Yes/No.) Step 9 depends on this answer.

Confirm these before going on:

- [ ] You've **accepted the invite** to the CauseMic-Hub organization on GitHub. Check the email from GitHub, or visit github.com/CauseMic-Hub while signed in.
- [ ] You can open the SBGP repo in your browser: github.com/CauseMic-Hub/south-baltimore-gateway-partnership-SBGP
- [ ] The **1Password desktop app** is installed and you're signed in. The browser extension alone isn't enough.
- [ ] You know how to open **Terminal**: press ⌘ Space, type *Terminal*, press Enter.

**Troubleshooting:**
- If you see "404" on the SBGP repo, the invite hasn't been accepted or access hasn't been granted. Ask Jen.

---

## Step 2: Look at what's already on your Mac (read-only)

These commands only *look*. They don't change anything. Paste the output to Claude.

```bash
ls -la ~/.ssh
```
**Expect:** either a list of files, or `No such file or directory`. Both are fine.

```bash
cat ~/.ssh/config
```
**Expect:** either some text, or `No such file or directory`. Both are fine.

```bash
git config --global user.email
```
**Expect:** an email address, or nothing at all. Both are fine.

**Claude, note for later:**
- If a file named `id_ed25519_causemic` already exists, don't overwrite it in Step 3. Ask whether they've done this before, and pick a different file name if needed.
- If `~/.ssh/config` already has a `Host github-causemic` section, skip adding it in Step 6 and check what's there instead.
- If `~/.ssh/config` has a `Host *` section mentioning 1Password (`IdentityAgent`), that's fine. Note it for Step 7.
- The global email tells you what Git uses today. Step 9 adds a CauseMic-specific email without changing it.

---

## Step 3: Create your CauseMic SSH key

An SSH key is how your Mac proves to GitHub that it's you, without a password. It comes in two halves:
- a **private** half that never leaves your Mac and 1Password;
- a **public** half that you give to GitHub.

```bash
ssh-keygen -t ed25519 -C "<work-email>" -f ~/.ssh/id_ed25519_causemic
```

It asks three things:

1. **"Enter passphrase"**: make one up and type it. **Nothing appears as you type. That's normal.** Press Enter.
2. **"Enter same passphrase again"**: type it again and press Enter.
3. **If it asks "Overwrite (y/n)?", type `n`** and tell Claude. That means a key with this name already exists.

**Expect:** "Your identification has been saved in /Users/…/.ssh/id_ed25519_causemic", then "Your public key has been saved in …id_ed25519_causemic.pub", then a fingerprint line starting `SHA256:` and a small box of random characters. You can ignore the box.

**Remember the passphrase.** 1Password asks for it once in the next step.

---

## Step 4: Save the key in 1Password

This puts the private key in your **own** 1Password, so 1Password can unlock it with Touch ID.

1. In the 1Password app: **New Item → SSH Key**.
2. Choose **Import** (it may say "Add Private Key" → "Import a Key File").
3. When the file picker opens, press **⌘ ⇧ G** (Command + Shift + G), type `~/.ssh`, and press Enter.
   - The `.ssh` folder is hidden. ⌘ ⇧ G is the easy way in. Alternatively, ⌘ ⇧ . (period) shows hidden files.
4. Select **`id_ed25519_causemic`**: the one **without** `.pub`. Click Open.
5. Enter the passphrase from Step 3.
6. Name the item **GitHub – CauseMic**.
7. Save it in your **private / employee vault**, **not** a shared vault. This key proves you are you, and teammates should never use it.

**Expect:** a new SSH Key item in 1Password, showing a public key and a fingerprint (`SHA256:…`).

---

## Step 5: Add the public key to GitHub

1. In your browser, **make sure you're signed in to GitHub as `<github-username>`**.
2. Click your profile photo (top right) → **Settings** → **SSH and GPG keys** (left sidebar) → **New SSH key**.
3. **Title:** `<full-name> Mac – CauseMic`
4. **Key type:** Authentication Key.
5. **Key:** click into the box.
   - If 1Password offers to fill it, choose the **GitHub – CauseMic** item. It may fill the key without the email at the end. That's fine: the email is just a label.
   - Or, to copy it manually, run this in Terminal, then paste with ⌘V:
     ```bash
     pbcopy < ~/.ssh/id_ed25519_causemic.pub
     ```
     **Expect:** nothing prints. The key is now on your clipboard.
6. Click **Add SSH key**. GitHub may ask you to confirm with your password or a code.

**Check that it's the same key:**
```bash
ssh-keygen -lf ~/.ssh/id_ed25519_causemic.pub
```
**Expect:** one line like `256 SHA256:AbC123… <work-email> (ED25519)`.

The `SHA256:…` part should **match** the fingerprint GitHub now shows for the key, and the one in 1Password. If GitHub's is different, 1Password may have filled a different key. Delete it on GitHub and paste using the `pbcopy` method instead.

---

## Step 6: Give the key a nickname on your Mac

This tells your Mac: *"When I connect to `github-causemic`, go to GitHub and ask 1Password for the CauseMic key, and only that key."*

**Run this block exactly once.** It adds to the file (creating it if needed), and running it twice adds a duplicate.

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

**Check it:**
```bash
cat ~/.ssh/config
```
**Expect:** the `Host github-causemic` section appears **once**. If it appears more than once, tell Claude, who will help you remove the extras.

---

## Step 7: Turn on the 1Password SSH agent

1. 1Password → **Settings** (⌘ ,) → **Developer**.
2. Turn on **"Use the SSH agent."**
3. If it asks **"Allow 1Password to save SSH key names to disk?"**, choose **Use Key Names**. Only the item *title* is saved, not the key, and the Touch ID prompt then tells you which key is being used.
4. If it offers to **"Edit automatically"** or shows a config **snippet** to copy, **skip it.** Step 6 already pointed only the CauseMic connection at 1Password. The snippet would route *every* SSH connection on your Mac through 1Password.

**Expect:** the setting shows as on. There's nothing to run in Terminal for this step.

---

## Step 8: Test the connection

```bash
ssh -T git@github-causemic
```

**Expect, in order:**
1. **The first time only:** "The authenticity of host 'github.com' can't be established… ED25519 key fingerprint is SHA256:+DiY3wvvV6TuJJhbpZisF/zLDA0zPMSvHdkr4UvCOqU. Are you sure you want to continue connecting (yes/no)?"
   - If the fingerprint matches exactly, type `yes` and press Enter. (That's GitHub's published fingerprint.)
   - If it **doesn't** match, type `no` and tell Claude.
2. **A 1Password prompt** asking to allow the key **GitHub – CauseMic**. Approve with Touch ID.
3. **"Hi <github-username>! You've successfully authenticated, but GitHub does not provide shell access."** The second half is normal. **This line means success.**

**Troubleshooting:**
- `Permission denied (publickey)`:
  - The key isn't on GitHub, or it's on a different GitHub account. Recheck Step 5 while signed in as `<github-username>`.
  - Or the 1Password agent is off (Step 7).
  - Or the key isn't in your private vault. 1Password's agent only serves keys from certain vaults by default; check Settings → Developer.
- **No 1Password prompt appears:** make sure the 1Password app is open and unlocked, then try again.
- **"Hi" shows a different username:** the key was added to the wrong GitHub account. Remove it there and add it to the right one (Step 5).
- `Could not resolve hostname github-causemic`: the Step 6 section is missing or mistyped. Run `cat ~/.ssh/config` and compare.

---

## Step 9: Make your work show up under your CauseMic email

Git stamps every change you save ("commit") with a name and email. This step sets your CauseMic name and email for CauseMic folders only.

### 9a. Choose your CauseMic folder

Recommended: a folder just for CauseMic repos.

```bash
mkdir -p ~/CauseMic
```
**Expect:** nothing prints. If the folder already exists, nothing happens; that's fine.

*(Claude: if they already keep work in a different folder and want to use that, use their path everywhere below instead of `~/CauseMic/`. Keep the trailing slash.)*

### 9b. Back up your current Git settings

```bash
cp ~/.gitconfig ~/.gitconfig.bak-before-causemic
```
**Expect:** nothing prints. If you see `No such file or directory`, there were no settings to back up. That's fine; continue.

### 9c. Create your CauseMic identity file

```bash
git config --file ~/.gitconfig-causemic user.name "<full-name>"
```
**Expect:** nothing prints.

```bash
git config --file ~/.gitconfig-causemic user.email "<work-email>"
```
**Expect:** nothing prints.

### 9d. Use it inside the CauseMic folder

```bash
git config --global includeIf."gitdir:~/CauseMic/".path ~/.gitconfig-causemic
```
**Expect:** nothing prints.

This *adds* a rule. It doesn't change your existing Git settings. Outside `~/CauseMic/`, Git keeps working exactly as before. That's what keeps CauseMic work separate from any other work on this Mac.

*(Claude: if they answered "No" to other Git work in Step 1 and Step 2 showed no global email, this step is still the recommended setup, so do it the same way.)*

---

## Step 10: Copy the SBGP repo to your Mac ("clone")

```bash
cd ~/CauseMic
```
**Expect:** nothing prints. Your Terminal prompt may now show `CauseMic`.

```bash
git clone git@github-causemic:CauseMic-Hub/south-baltimore-gateway-partnership-SBGP.git
```
**Expect:**
1. `Cloning into 'south-baltimore-gateway-partnership-SBGP'...`
2. A **1Password Touch ID prompt**. Approve it.
3. Lines like `Receiving objects: 100% … done.`

**Always copy CauseMic repos using an address that starts with `git@github-causemic:`**, not the `https://github.com/...` address GitHub shows by default. On GitHub, the green **Code** button → **SSH** tab shows `git@github.com:CauseMic-Hub/...`. Replace `github.com` with `github-causemic`.

**Troubleshooting:**
- `Repository not found`: access to the repo hasn't been granted, or the invite isn't accepted yet. Ask Jen.
- `Permission denied (publickey)`: go back to Step 8.

---

## Step 11: Final checks (this proves it all works)

```bash
cd ~/CauseMic/south-baltimore-gateway-partnership-SBGP
```
**Expect:** nothing prints.

```bash
git config user.email
```
**Expect:** exactly `<work-email>`.

```bash
git remote -v
```
**Expect:** two lines, both containing `git@github-causemic:CauseMic-Hub/south-baltimore-gateway-partnership-SBGP.git`, one ending `(fetch)` and one ending `(push)`.

If they do other Git work on this Mac, also check that it's unchanged:

```bash
cd ~
```
**Expect:** nothing prints.

```bash
git config --global user.email
```
**Expect:** the same email Step 2 showed (or nothing, if it showed nothing). It should **not** have changed.

**🎉 You're set up.** From now on:
- open CauseMic repos from inside `~/CauseMic/`;
- clone new CauseMic repos with the `git@github-causemic:` address;
- expect a Touch ID prompt whenever your Mac talks to GitHub for CauseMic. **Only approve it when you're the one doing something with GitHub.**

---

## Good to know

- **Nothing secret is in this guide.** Your key's private half lives only in `~/.ssh` and your 1Password.
- **Commands that print nothing usually worked.** Problems show up as messages starting with `error:`, `fatal:`, or `Permission denied`.
- **Pull requests:** open them in the GitHub website for now.
- **If you change Macs:** 1Password already has your key. On the new Mac, Steps 6–11 are what you'd redo, plus Step 2's checks.

## How to undo (if ever needed)

- Remove the `Host github-causemic` section from `~/.ssh/config`.
- Remove the `includeIf` section from `~/.gitconfig` (or restore `~/.gitconfig.bak-before-causemic`), and delete `~/.gitconfig-causemic`.
- Remove the key from GitHub (Settings → SSH and GPG keys) and from 1Password.
