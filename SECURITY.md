# Security record

This is a living record of the hardening state of the two `zerotrustdavid` profile
repositories. It is edited in place as settings change — it does not accumulate
dated entries.

Both repos were built and pushed from an environment with GitHub write access but
no access to repository-administration endpoints (branch protection, security-and-analysis,
Actions permissions). The state below is what was live-verified from that
environment; the target-state commands still to be run locally are recorded under
"Outstanding" for each repo so this file stays accurate rather than aspirational.

## zerotrustdavid/zerotrustdavid

- **Visibility:** public (verified).
- **Purpose:** profile README only — no tooling, no workflows.
- **Collaborators:** owner only, no outside collaborators (verified via the GitHub API).
- **Branch protection on `main`:** not yet configured.
- **Secret scanning / push protection / Dependabot security updates:** not yet configured (repository defaults).
- **Private vulnerability reporting:** not yet configured.
- **Actions default workflow permissions:** not yet configured (repository defaults; this repo carries no workflows).
- **Signed commits on `main`:** not required yet.
- **`has_wiki` / `has_projects`:** on (repository defaults) — target is off.

## zerotrustdavid/profileassets

- **Visibility:** public (verified).
- **Purpose:** banner assets, lint tooling, and this security record for both repos.
- **Collaborators:** owner only, no outside collaborators (verified via the GitHub API).
- **CI:** `.github/workflows/lint.yml` — markdown lint and link-check, third-party actions pinned to commit SHA, `permissions: contents: read`.
- **Branch protection on `main`:** not yet configured.
- **Secret scanning / push protection / Dependabot security updates:** not yet configured (repository defaults).
- **Private vulnerability reporting:** not yet configured.
- **Actions default workflow permissions:** not yet configured (repository defaults).
- **Signed commits on `main`:** not required yet.
- **`has_wiki` / `has_projects`:** on (repository defaults) — target is off.
- **Secrets stored:** none. Stats cards are unauthenticated public reads; no token is needed or present.

## Outstanding — run locally with an authenticated `gh`

The environment that built these repos could reach the GitHub REST API only through
a restricted MCP tool surface (repo creation, file commits, collaborator listing) and
had no route to the administration endpoints below. Run this once, from a machine
with `gh auth status` showing `zerotrustdavid`, to bring both repos to the target
state, then update the sections above to match what a fresh query returns:

```bash
for REPO in zerotrustdavid profileassets; do
  gh api --method PATCH "repos/zerotrustdavid/${REPO}" --input - <<'JSON'
{
  "security_and_analysis": {
    "secret_scanning": { "status": "enabled" },
    "secret_scanning_push_protection": { "status": "enabled" },
    "dependabot_security_updates": { "status": "enabled" }
  },
  "delete_branch_on_merge": true,
  "allow_squash_merge": true,
  "allow_merge_commit": false,
  "allow_rebase_merge": false,
  "has_wiki": false,
  "has_projects": false
}
JSON
  gh api --method PUT "repos/zerotrustdavid/${REPO}/private-vulnerability-reporting"
  gh api --method PUT "repos/zerotrustdavid/${REPO}/actions/permissions/workflow" --input - <<'JSON'
{ "default_workflow_permissions": "read", "can_approve_pull_request_reviews": false }
JSON
  gh api --method PUT "repos/zerotrustdavid/${REPO}/branches/main/protection" --input - <<'JSON'
{
  "required_status_checks": null,
  "enforce_admins": true,
  "required_pull_request_reviews": {
    "required_approving_review_count": 1,
    "dismiss_stale_reviews": true
  },
  "restrictions": null,
  "allow_force_pushes": false,
  "allow_deletions": false,
  "required_conversation_resolution": true
}
JSON
  gh api --method POST "repos/zerotrustdavid/${REPO}/branches/main/protection/required_signatures"
done
```

For `profileassets`, once the above is applied, also add the `markdown-lint` and
`link-check` check names to `required_status_checks` so a PR cannot merge on a
failing lint.

Re-query after running this and update the two sections above with the live result —
do not mark a setting as applied without the confirming API response in hand.
