---
name: github-commit-push
description: Commit workspace changes and push this project to the GitHub repo larissa215/huaminghui over SSH (port 443). Use when the user asks to commit and push this project to GitHub. Do not use for pull requests, other repositories, or non-git file edits.
---

# GitHub Commit & Push

Target repo: `git@github.com:larissa215/huaminghui.git` (branch `main`).
Auth: SSH key `C:/Users/Laris/.ssh/id_ed25519`, already registered on GitHub as `larissa215`.

## Steps

1. Verify auth before changing repo state:

```powershell
ssh -o BatchMode=yes -T git@github.com
```

Success prints `Hi larissa215!`. Port 22 is refused on this network, so `~/.ssh/config` must map `github.com` to `ssh.github.com:443`. If auth fails, stop and report instead of retrying.

2. Initialize the repo if `.git` is missing:

```powershell
git init -b main
git remote add origin git@github.com:larissa215/huaminghui.git
```

If `.git` exists, confirm `git remote -v` points at the same URL and branch is `main`; fix only that, never rewrite unrelated history.

3. Review before staging. Run `git status --short` and check for secrets (`.env`, tokens, credentials). Exclude them with `.gitignore` rather than committing.

4. Stage and commit. Add explicit paths or, for a full project snapshot, `git add -A` after the secret check. Write a concise message stating why the change was made:

```powershell
git commit -m "<type>: <summary>"
```

Do not commit an empty change set; report "nothing to commit" instead.

5. Push and set upstream:

```powershell
git push -u origin main
```

6. Verify with `git status` showing `up to date with origin/main` and report the commit hash and remote URL.

## Rules

- Never force-push, never amend pushed commits, never `git reset --hard` without an explicit user request.
- Never skip hooks or change git config.
- Never commit private keys or credentials; `.ssh` lives outside the repo and stays untracked.
- If the remote already has commits, fetch and integrate instead of forcing.
