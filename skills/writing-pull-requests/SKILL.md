---
name: writing-pull-requests
description: Use when creating or updating a pull request, writing a PR title or description, or about to run gh pr create.
---

# Writing Pull Requests

## Overview

A pull request is written for the person who will review it. They did not see the work happen, and they should be able to understand what the PR solves, and decide where to look, in about two minutes. Enough text to explain the problem and the solution well; short enough to read in one pass.

If the repo has a PR template (`.github/pull_request_template.md` or similar), fill that in and apply this skill to each section.

## The Shape

1. **Title:** one line, imperative, says what changes ("Fix duplicate retries in webhook sender").
2. **Problem:** 1-3 sentences. What was wrong or missing, and who it affected. Link the issue or ticket if one exists.
3. **Solution:** a short paragraph or 2-5 bullets on the approach, at the level of ideas. Name the key decision and the alternative you rejected if the reviewer might wonder.
4. **Where to look:** the 1-3 places where the real change is, and anything mechanical the reviewer can skim (renames, moved code, generated files).
5. **Risks / follow-ups:** only if there are any: breaking changes, migrations, deliberately deferred work.

A small PR gets a short description: title, problem, solution. Skip a section that has nothing to say; do not pad it.

## Writing Style

- Short paragraphs and flat bullets. Plain words. Lead with what matters most.
- Explain the *why*; the diff already shows the *what*, line by line.
- Anything long (logs, benchmarks, stack traces) goes in a collapsed `<details>` block.
- Keep attribution lines required by your instructions at the very end.

## Before Creating

Reread the description as the reviewer. If they would need to open the diff to understand the problem, the Problem section is too thin. If they scroll past a wall of text to find what to review, cut it down.
