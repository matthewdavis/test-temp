---
title: Quartz Site Setup Guide
---

These steps give you your own notes website at `https://<your-username>.github.io/olicashmoney/`, rebuilt automatically every time you push changes. Setup takes about 20 minutes.

## Before you start

- A GitHub account.
- [Git](https://git-scm.com/downloads) and [Node.js](https://nodejs.org/) version 22 or newer (24 recommended). Check with `node -v`.

### On Windows 10

Run every command in this guide in **PowerShell** (Start menu → type "PowerShell"). Install everything in one go:

```powershell
winget install --id Git.Git -e
winget install --id OpenJS.NodeJS.LTS -e
```

Then close PowerShell and open a new window so it picks up the new tools. If `winget` isn't recognized, install **App Installer** from the Microsoft Store first, or download the installers from the links above.

Run these once as well:

```powershell
Set-ExecutionPolicy -Scope CurrentUser RemoteSigned
git config --global core.longpaths true
```

The first lets PowerShell run `npx`. The second stops Git from failing on Quartz's long file paths. For editing files, [VS Code](https://code.visualstudio.com/) (`winget install --id Microsoft.VisualStudioCode -e`) works better than Notepad.

## Setup

### 1. Create your repo from the template

1. Go to [matthewdavis/test-temp](https://github.com/matthewdavis/test-temp) and click **Use this template → Create a new repository**.
2. Name it `olicashmoney` and make it **Public**. Free GitHub accounts can only publish public repos to Pages.
3. GitHub starts a "Deploy Quartz site to GitHub Pages" run right away. It will fail because Pages isn't turned on yet. That's expected; you'll rerun it in step 2.

### 2. Turn on GitHub Pages

In your new repo, go to **Settings → Pages → Source** and pick **GitHub Actions**.

Then go to **Actions → Deploy Quartz site to GitHub Pages → Run workflow** to rerun the deploy.

### 3. Download it and set your site's address

```shell
git clone https://github.com/<your-username>/olicashmoney.git
cd olicashmoney
npm ci
```

Open `quartz.config.yaml` and change these two lines:

```yaml
pageTitle: My Notes                              # whatever you like
baseUrl: <your-username>.github.io/olicashmoney   # no https://, no trailing slash
```

### 4. Preview on your computer

```shell
npx quartz plugin install
npx quartz build --serve
```

Open http://localhost:8080. Your notes live in the `content/` folder, and `content/index.md` is your homepage, so replace its text with your own. Press Ctrl+C to stop the preview.

On Windows, a firewall prompt may appear the first time you run the preview. Click **Allow** (private networks is enough).

### 5. Publish

```shell
npx quartz sync
```

This commits your changes and pushes them. About a minute later the site is live at `https://<your-username>.github.io/olicashmoney/`. You can watch the deploy in the **Actions** tab.

## Day to day

Add or edit `.md` files in `content/`, check them with `npx quartz build --serve`, then publish with `npx quartz sync`. You can also write in [Obsidian](https://obsidian.md/) (`winget install --id Obsidian.Obsidian -e` on Windows) by opening `content/` as a vault.

## If something goes wrong

| What you see | Fix |
| --- | --- |
| A run called **pages-build-deployment** fails with a "Liquid syntax error" | Pages is set to "Deploy from a branch". Change the source to **GitHub Actions** (step 2). |
| "not allowed to deploy to github-pages due to environment protection rules" | Go to **Settings → Environments**, delete `github-pages`, then rerun the workflow. |
| The site loads but links, styling or social preview images are broken | `baseUrl` in `quartz.config.yaml` is wrong. It must match your actual site address. |
| `npm ci` fails or Quartz complains about the Node version | Install Node 22 or newer. |
| Pages settings say you need to upgrade | The repo is private. Make it public in **Settings → General**. |
| Warning: "No quartz.lock.json found" | Harmless. Ignore it. |
| Windows: "running scripts is disabled on this system" | Run `Set-ExecutionPolicy -Scope CurrentUser RemoteSigned`, then try again. |
| Windows: `git` or `node` "is not recognized" | Close PowerShell and open a new window after installing. |
| Windows: "Filename too long" during clone or sync | Run `git config --global core.longpaths true`. |
| Windows: `quartz sync` asks you to sign in to GitHub | Sign in through the browser window Git opens. It only asks once. |
