# SETUP.md — Windows Command Prompt Deployment Guide

Step-by-step instructions for deploying this repository to GitHub Pages using Windows Command Prompt. No GitHub Desktop, no PowerShell, no WSL required.

---

## Prerequisites

Before starting, confirm you have these installed:

- **Git for Windows**: https://git-scm.com/download/win
  - During install, select "Git from the Command Line and also from 3rd-party software"
- **A GitHub account**: https://github.com

Verify git is installed. Open Command Prompt (press `Win + R`, type `cmd`, press Enter) and run:

```
git --version
```

You should see something like `git version 2.43.0`. If you get an error, reinstall Git for Windows.

---

## Step 1 — Configure Git (first time only)

If you have not used git on this machine before, set your identity:

```
git config --global user.name "DrMarioDeSean411-arch"
git config --global user.email "your-email@example.com"
```

Replace the email with the one attached to your GitHub account.

---

## Step 2 — Create the repository on GitHub

1. Go to https://github.com/new
2. Fill in:
   - **Repository name**: `nist-frte-facial-recognition-bias`
   - **Visibility**: Public
   - **Do NOT** check "Add a README file" — leave everything unchecked
3. Click **Create repository**
4. Leave the browser tab open — you will need the repository URL

---

## Step 3 — Set up a local folder

Choose where you want to work. This example uses your Desktop:

```
cd %USERPROFILE%\Desktop
mkdir nist-frte-facial-recognition-bias
cd nist-frte-facial-recognition-bias
```

---

## Step 4 — Copy files into the folder

Copy these three files into `nist-frte-facial-recognition-bias\`:

- `frte_false_positive_calculator.html`
- `README.md`
- `SETUP.md`

You can do this in File Explorer or with Command Prompt if the files are already on your Desktop:

```
copy %USERPROFILE%\Desktop\frte_false_positive_calculator.html .
copy %USERPROFILE%\Desktop\README.md .
copy %USERPROFILE%\Desktop\SETUP.md .
```

Verify all three files are present:

```
dir
```

You should see all three listed.

---

## Step 5 — Initialize git and make the first commit

```
git init
git add .
git commit -m "Initial commit: FRTE false positive calculator and documentation"
```

---

## Step 6 — Connect to GitHub and push

```
git branch -M main
git remote add origin https://github.com/DrMarioDeSean411-arch/nist-frte-facial-recognition-bias.git
git push -u origin main
```

Git will prompt for your GitHub username and password. For the password, use a **Personal Access Token** (PAT), not your account password.

**To create a PAT:**
1. Go to https://github.com/settings/tokens
2. Click **Generate new token (classic)**
3. Give it a name (e.g., `nist-frte-deploy`)
4. Set expiration to 90 days
5. Check the **repo** scope
6. Click **Generate token**
7. Copy the token immediately — you cannot see it again

Paste the token when Command Prompt asks for your password.

---

## Step 7 — Enable GitHub Pages

1. Go to https://github.com/DrMarioDeSean411-arch/nist-frte-facial-recognition-bias
2. Click **Settings** (top navigation bar)
3. Click **Pages** (left sidebar, under "Code and automation")
4. Under **Source**, select **Deploy from a branch**
5. Under **Branch**, select `main` and folder `/` (root)
6. Click **Save**

GitHub Pages will take 1–3 minutes to build. Refresh the Settings → Pages screen until you see:

```
Your site is live at https://DrMarioDeSean411-arch.github.io/nist-frte-facial-recognition-bias/
```

---

## Step 8 — Verify the live calculator

Open a browser and go to:

```
https://DrMarioDeSean411-arch.github.io/nist-frte-facial-recognition-bias/frte_false_positive_calculator.html
```

The calculator should load fully. No login required, no server needed — the file is self-contained.

---

## Making Updates Later

When you modify files and want to push changes:

```
cd %USERPROFILE%\Desktop\nist-frte-facial-recognition-bias
git add .
git commit -m "Brief description of what changed"
git push
```

GitHub Pages will automatically rebuild within 1–2 minutes.

---

## Troubleshooting

**`git push` says "remote: Repository not found"**
Double-check the remote URL:
```
git remote -v
```
If the URL is wrong, reset it:
```
git remote set-url origin https://github.com/DrMarioDeSean411-arch/nist-frte-facial-recognition-bias.git
```

**Authentication failed**
Your Personal Access Token may have expired. Generate a new one at https://github.com/settings/tokens and try again.

**GitHub Pages shows a 404**
Wait 2–3 more minutes and hard-refresh (Ctrl + Shift + R). If it persists, confirm the branch is set to `main` and folder is `/` (root) in Settings → Pages.

**Calculator loads but looks broken**
Confirm `frte_false_positive_calculator.html` is in the root of the repository (not inside a subfolder).
