---
name: syncing-superpowers-fork
description: Use when the user wants to bring upstream superpowers changes into their fork, check what is new in obra/superpowers, review upstream commits, or decide which upstream changes to adopt or skip in ~/projects/superpowers.
---

# Syncing the superpowers fork

## Overview

`~/projects/superpowers` is a fork: `origin` = `Janmuixi/superpowers`, `upstream` = `obra/superpowers`. `main` mirrors upstream; `custom` carries the user's changes. The tag `upstream-synced` marks the last upstream commit the user has already decided on (adopted **or** skipped). The user chooses what comes in. Never merge or rebase upstream wholesale.

## Steps

1. **Prepare.** `cd ~/projects/superpowers`. `git status --short` must be empty; if not, stop and tell the user. Then `git fetch upstream --tags`.
2. **List what is new.** Range is `upstream-synced..upstream/main`. If empty, say there is nothing new and stop.
   - `git log --reverse --format='%h %ad %s' --date=short upstream-synced..upstream/main`
   - Per-commit files: `git show --stat --format= <sha>`
3. **Find overlaps.** Files the user changed: `git diff --name-only upstream-synced custom`. Mark any new upstream commit touching one of them as **overlaps with your changes**; those can conflict.
4. **Present in chat.** Group commits by theme or release (read release notes with `gh api repos/obra/superpowers/releases` when tags exist). For each group show: number, one-line plain-language summary, files touched, overlap flag. Put commits touching `skills/` first and mark docs, tests, and other-harness fixes as low relevance. Show the actual diff (`git show <sha>`) when the user asks, or for small skill changes by default.
5. **Let the user choose.** Ask which groups to adopt, using AskUserQuestion with multiSelect (at most 4 options per question; split across questions or use a numbered list in chat when there are more). Include "show me the diff" as a way out. Unchosen groups are skipped.
6. **Apply only the chosen ones.** `git checkout custom`, then `git cherry-pick -x <sha>...` in chronological order. On conflict, show the conflicted hunks and resolve with the user; never pick a side silently.
7. **Report, then confirm.** Show `git log --oneline upstream-synced..custom` for what landed and list what was skipped. After the user confirms:
   - `git tag -f upstream-synced upstream/main` (advances over skipped commits too, so they are not offered again)
   - `git checkout main && git merge --ff-only upstream/main && git checkout custom`
   - `git push origin custom main && git push -f origin upstream-synced`

## Rules

- Do not use `git merge upstream/main` or `git rebase upstream/main` on `custom`.
- Do not push before the user confirms step 7.
- If a skipped change is later wanted, it is still in `git log` of `upstream/main`; cherry-pick it by hash.
- Release-sized changes that touch the same files as the user's edits: show both diffs side by side before choosing.
