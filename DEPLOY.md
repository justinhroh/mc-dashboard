# Deploying the MC Dashboard to GitHub Pages

This is a one-time setup. After that, refreshing the dashboard each week is a single command (see "Weekly refresh" below).

> Your GitHub username: **`justinhroh`**. All commands and URLs below are pre-filled for you.

---

## 1. Create the repo

1. Go to <https://github.com/new>
2. Repository name: `mc-dashboard`
3. Visibility: **Public** (required for free GitHub Pages — but the data is encrypted, so this is fine)
4. Do NOT initialize with a README (we already have files)
5. Click **Create repository**

GitHub will show you setup commands. Ignore them — use the steps below instead.

---

## 2. Upload the files

Open Terminal on your Mac and run:

```bash
cd "/Users/justinhroch/Documents/Claude/Projects/MC Group Agent/mc-dashboard"

git init
git add index.html data.json
git commit -m "Initial dashboard"
git branch -M main
git remote add origin https://github.com/justinhroh/mc-dashboard.git
git push -u origin main
```

You'll be prompted to authenticate. The easiest way:

- If you've used `gh` (GitHub CLI) before, run `gh auth login` first.
- Otherwise, use a Personal Access Token as your password when prompted: <https://github.com/settings/tokens?type=beta> → Generate new token → repository access: `mc-dashboard` → permissions: Contents = read & write.

> ⚠️ **Important:** We are intentionally NOT pushing `data_plain.json` (which has prayer requests in plain text) or `build_data.py`. A `.gitignore` file is included to prevent that.

---

## 3. Turn on GitHub Pages

1. Go to <https://github.com/justinhroh/mc-dashboard/settings/pages>
2. Under **Source**, select: **Deploy from a branch**
3. Branch: `main`, folder: `/ (root)`
4. Click **Save**
5. Wait ~1 minute. The page will show: *"Your site is live at https://justinhroh.github.io/mc-dashboard/"*

**Your live URL will be: <https://justinhroh.github.io/mc-dashboard/>**

---

## 4. Test it

1. Open <https://justinhroh.github.io/mc-dashboard/> on your phone
2. Enter the shared passphrase
3. Confirm prayer requests, calendar, and sermon all appear
4. Share the URL + passphrase with the MC group (text, Slack DM, in person — your call)

> **Tip:** Tell the group to "Add to Home Screen" on their phone so it opens like an app.

---

## Weekly refresh

When prayer requests change in Slack, or a new sermon goes up, ask Claude in this chat:

> *"Refresh the MC dashboard."*

Claude will:
1. Pull the latest #prayer messages
2. Fetch the latest sermon from austinstone.org
3. Update `data_plain.json`
4. Re-encrypt to `data.json`
5. Tell you to run two git commands to push the update:

```bash
cd "/Users/justinhroch/Documents/Claude/Projects/MC Group Agent/mc-dashboard"
git add data.json && git commit -m "weekly refresh" && git push
```

The site updates within ~30 seconds of the push. Members can pull-to-refresh on their phones.

---

## If you forget the passphrase

You can pick a new one and re-encrypt:

```bash
cd "/Users/justinhroch/Documents/Claude/Projects/MC Group Agent/mc-dashboard"
python3 build_data.py --password "your-new-passphrase"
git add data.json && git commit -m "rotate passphrase" && git push
```

Then share the new passphrase with the group.

---

## File reference

| File | Purpose | In git? |
|---|---|---|
| `index.html` | The dashboard page | yes |
| `data.json` | Encrypted prayer + sermon + calendar | yes |
| `data_plain.json` | Plaintext source (for editing/review) | **NO** (gitignored) |
| `build_data.py` | Encrypts plaintext → data.json | **NO** (gitignored) |
| `.gitignore` | Tells git to skip the two above | yes |
| `DEPLOY.md` | This file | optional |
