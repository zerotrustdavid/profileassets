# Security record

This is a living record of the hardening state of this repository. It is
edited in place as settings change — it does not accumulate dated entries.
[zerotrustdavid/SECURITY.md](https://github.com/zerotrustdavid/zerotrustdavid/blob/main/SECURITY.md)
holds the record for the other profile repository.

## zerotrustdavid/profileassets

- **Visibility:** public (verified via the GitHub API).
- **Purpose:** banner assets, lint tooling, and this security record.
- **Collaborators:** owner only, no outside collaborators (verified via the GitHub API).
- **CI:** `.github/workflows/lint.yml` — markdown lint and link-check, third-party actions pinned to commit SHA, `permissions: contents: read`. Note: `markdown-lint` was red from the very first commit until 2026-08-27 (default 80-character line cap tripped by the intentionally long bio/badge lines) — undetected until the ruleset's required checks blocked a merge and forced it to surface. Fixed via `.markdownlint.json` (`MD013: false`); both checks are green as of the current `main`.
- **Code scanning:** a CodeQL default-setup workflow is active on this repo (owner-enabled) and has run green on every push and PR so far.
- **Branch protection on `main`:** a ruleset now targets `main` and requires a
  pull request before merging, restrict deletions, block force pushes, and
  the `markdown-lint` and `link-check` status checks (owner-reported; no tool
  available to this record's verifying environment can read rulesets, so this
  is taken on the owner's word, not independently confirmed).
- **Secret scanning / push protection / Dependabot security updates:** not
  confirmed. The environment that verified this record has no route to the
  repository security-settings API — check directly under Settings → Advanced Security.
- **Private vulnerability reporting:** not confirmed.
- **Actions default workflow permissions:** not confirmed.
- **Signed commits on `main`:** not confirmed whether ticked on the ruleset.
- **`has_wiki` / `has_projects`:** off (confirmed live via the GitHub API).
- **Secrets stored:** none. Stats cards are unauthenticated public reads; no token is needed or present. The profile README no longer embeds stats cards at all (removed 2026-08-27 — the public github-readme-stats.vercel.app instance kept rendering broken even on the built-in theme).

## Outstanding

Still not independently confirmed (this environment has no tool that reaches
these endpoints, so they can only be taken on the owner's word or checked
directly): secret scanning, push protection, Dependabot security updates,
private vulnerability reporting, Actions default workflow permissions,
delete-branch-on-merge and merge-method restrictions, and whether "Require
signed commits" is ticked on this repo's ruleset. Confirm each under
Settings → Advanced Security / General / Actions / Rulesets, and update the
relevant line above once seen — not on the strength of a toggle having been
clicked.
