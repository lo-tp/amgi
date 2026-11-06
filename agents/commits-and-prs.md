# Commits and PRs

Reached from `AGENTS.md` → *Commits*.

## The rule, in full

A `docs:` commit holds documentation and nothing else. A code commit holds code
and nothing else. Documentation here means `Documentation/**`, `CONTEXT.md`, the
per-package `AGENTS.md` files, and `agents/**`. Subjects follow the Conventional
Commits convention in `CONTRIBUTING.md`.

Why the separation exists: a PR that should carry only the code becomes a
selection over whole commits, which `git log --grep` answers. Nothing has to be
unstaged, re-split, or reviewed by path.

## Scratch that never reaches a PR

Notes a flow produces for itself — a draft PRD, a run report, a council's output,
a diagnosis log — go under `.agents/`, which `.gitignore` already covers. They
join `Documentation/` only when a person decides they belong there, and then in a
`docs:` commit. Nothing half-finished crosses into the tracked tree on its way
through.

## A code-only PR

Build the PR branch from a list, not from a split.

```bash
# the commits to take: everything on your branch that is not a docs commit
git log --reverse --format=%H --grep='^docs:' --invert-grep <target>..<branch> > /tmp/take.txt
git switch -c feat/short-name <target>
git cherry-pick $(cat /tmp/take.txt)
```

`<target>` is what the PR lands on, `<branch>` is the one carrying both kinds.
The docs commits stay where they are: excluded, not reverted, so no revert commit
pollutes the PR and nothing is lost locally.

The tidier shape never needs the recipe: keep docs work on your own branch, and
cut the code branch from the PR's target. Then there is no docs commit to
exclude.

This repo's `origin` is the fork. If the PR targets the upstream repo, add it as
a remote — and use the SSH form (`git@github.com:antigluten/amgi.git`): HTTPS to
`github.com` times out on this machine, `api.github.com` and SSH work.

## When a docs commit belongs with the code

An ADR that explains the code in the same PR belongs *with* it. Commit it last,
so including or excluding it stays a whole-commit decision.

## Check a commit is what it claims

```bash
git show --name-only --format= <sha> | grep -Ev '^(Documentation|agents)/|AGENTS\.md$|CONTEXT\.md$'
```

Empty output means that commit is docs-only.
