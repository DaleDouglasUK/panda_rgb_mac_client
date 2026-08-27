# CLAUDE.md

Operating rules for this repository, for AI agents (Claude Code) and human contributors
alike. Read this before making changes.

This repository publishes the compiled macOS client. The source lives in a private
repository and is not mirrored here. What lands here is the built app, the licence, and the
documentation a downloader needs.

## 1. Golden rules

1. Ask before doing anything irreversible or outward-facing: pushing, opening PRs, deleting
   files you didn't create, force-pushing, or publishing a release.
2. Never commit secrets. No tokens, keys, passwords, or `.env` contents in code, commits, or
   PR text. If you find one, stop and report it.
3. Every commit and PR maps to an issue (section 3). No exceptions.
4. Work in small, reviewable units. One logical change per branch and PR.
5. Verify before claiming done. Run the checks and report real output. If something was
   skipped or failed, say so plainly.
6. Don't expand scope. Note unrelated issues separately; don't fix them inline.

## 2. Attribution (hard rule)

- Never add AI or Claude attribution to anything. No `Co-Authored-By` trailers, no
  "Generated with Claude Code" lines, no robot-emoji credits, in commits, PR titles or
  bodies, docs, or any generated content. No exceptions.
- Never use em-dashes or en-dashes in any writing, and never fake one with a double hyphen.
  Use commas, semicolons, colons, parentheses, or periods. Plain hyphens are fine for
  numeric ranges and required syntax.
- Never use emojis in responses, documents, commits, or any output.

## 3. Issues and traceability (full stop)

- **Every commit and every PR must reference an issue. No exceptions.** If work is worth
  committing, it is worth an issue. File one first if it does not exist.
- PRs link the issue with `Closes #<n>` (or `Refs #<n>` when it does not fully resolve it).
  Commits reference it in a `Refs: #<n>` footer, or close it with `Closes #<n>`.
- Issues are filed through the forms in `.github/ISSUE_TEMPLATE/`. Blank issues are
  disabled.
- `.github/workflows/pr-policy.yml` enforces the linked issue and the test rule below. It is
  a required check on `main`.

## 4. Branching

- `main` is protected and always releasable. Never commit directly to `main`.
- Branch from up-to-date `main`. Name branches `<type>/<short-slug>`, for example
  `fix/gatekeeper-note`, `docs/usage-note`, `chore/tidy-templates`.
- Keep branches short-lived. Prefer rebase over merge commits to keep history linear. Delete
  the branch after merge.

## 5. Commits (Conventional Commits)

Format: `type(scope): subject`

- Types: `feat`, `fix`, `docs`, `refactor`, `test`, `chore`, `build`, `ci`, `perf`, `style`,
  `revert`.
- Subject: imperative mood, lowercase, no trailing period, 72 chars or fewer. Use "add retry
  logic", not "added" or "adds".
- Body: explain why, not what. Wrap at 72 columns.
- Reference the issue in a footer: `Refs: #<n>` or `Closes: #<n>`.
- One logical change per commit.
- No AI attribution trailers (section 2).
- **Sign every commit with your SSH key, never GPG.** Configure once per clone:

  ```
  git config gpg.format ssh
  git config user.signingkey ~/.ssh/<your-key>.pub
  git config commit.gpgsign true
  ```

  The same key must be registered on the GitHub account **as a signing key**, which is a
  separate entry from the authentication key, or commits show as Unverified even though they
  are signed. Add it under Settings, SSH and GPG keys, with "Key type: Signing key", or with
  `gh ssh-key add --type signing`.

## 6. Testing (required, waiver-gated)

- **A PR cannot move forward without tests matching its commits.** New behaviour ships with
  tests. Bug fixes ship with a regression test that fails before the fix and passes after.
- **Waivers are explicit.** If a test is not practical, the author must specifically waive
  it, documented in all three places: the commit body, the PR description, and the issue.
  State what is untested and why.
- Because this repository currently holds no source, most PRs here will carry a waiver. That
  is the expected outcome, not a reason to skip the gate: the waiver still has to be written
  down and justified, and the gate is ready if source is ever published here.

## 7. Pull requests

- Open PRs against `main`. Title follows the same Conventional Commit format.
- The description uses `.github/pull_request_template.md` and must cover what changed and
  why, how it was tested with real output, risk and rollback, and the linked issue.
- All checks must pass before merge. Delete the branch after merge.
- **Merging rewrites commits, and the rewritten commit is unsigned.** Rebase merge produces new
  commits on `main` without the signatures the branch commits carried: `d200ab2` and `68f0f4b`
  were both signed on their branches and both report `unsigned` on `main`. Sign branch commits
  anyway, so the authorship of what was proposed is provable, but do not assume `main` shows
  verified history, and do not "fix" it by pushing directly.

## 8. Labels

Issues and PRs are labelled from four families: one `kind/*`, one `status/*`, a `priority/*`
once triaged, plus `area/*` and flags such as `security` where they apply. Set them at creation
time, and fix them when you touch an issue that has them wrong. Set `status/done` as part of
merging, not as a later tidy-up.

## 9. The published build

- The app is built and published by `panda-rgb-release.yml` in the source repository on every
  change that lands there. It replaces one rolling release tagged `latest`; there are no
  version numbers yet.
- Do not hand-upload a build over it. If a release needs replacing, fix the source and let the
  automation publish, so what is downloadable always corresponds to a known commit.
- `USAGE.md` documents how to run the unsigned download. Keep it accurate: if the signing
  situation changes, that file changes in the same PR.
