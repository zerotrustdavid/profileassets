# Security record

This is a living record of the hardening state of this repository. It is
edited in place as settings change — it does not accumulate dated entries.
[zerotrustdavid/SECURITY.md](https://github.com/zerotrustdavid/zerotrustdavid/blob/main/SECURITY.md)
holds the record for the other profile repository.

## zerotrustdavid/profileassets

- **Visibility:** public (verified via the GitHub API).
- **Purpose:** banner assets, lint tooling, and this security record.
- **Collaborators:** owner only, no outside collaborators (verified via the GitHub API).
- **CI:** `.github/workflows/lint.yml` — markdown lint and link-check, third-party actions pinned to commit SHA, `permissions: contents: read`.
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
- **`has_wiki` / `has_projects`:** on, as last confirmed live via the GitHub API — target is off.
- **Secrets stored:** none. Stats cards are unauthenticated public reads; no token is needed or present.

## Outstanding

Settings-page checklist (this environment has no tool that reaches any of
these endpoints, so they must be applied and confirmed directly):

1. **Settings → General → Features** — untick Wikis and Projects.
2. **Settings → General → Pull Requests** — tick "Automatically delete head
   branches"; untick "Allow merge commits" and "Allow rebase merging", leave
   "Allow squash merging" ticked.
3. **Settings → Advanced Security** — enable Secret scanning, its Push
   protection sub-toggle, and Dependabot security updates. Public repos
   sometimes ship with secret scanning already on — check current state first.
4. Same page — enable Private vulnerability reporting.
5. **Settings → Actions → General → Workflow permissions** — select "Read
   repository contents permission"; untick "Allow GitHub Actions to create
   and approve pull requests".
6. On this repo's ruleset, tick "Require signed commits".
7. Re-verify with a live query (API or the Settings UI) and update this file
   to match — do not mark an item done without seeing the confirming state.
