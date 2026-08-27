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
- **Branch protection on `main`:** not confirmed. No branch ruleset has been
  seen for this repo specifically — only `zerotrustdavid/zerotrustdavid`'s has
  been checked, and it applied to zero targets when last checked.
- **Secret scanning / push protection / Dependabot security updates:** not
  confirmed. The environment that verified this record has no route to the
  repository security-settings API — check directly under Settings → Code security.
- **Private vulnerability reporting:** not confirmed.
- **Actions default workflow permissions:** not confirmed.
- **Signed commits on `main`:** not required, as last checked.
- **`has_wiki` / `has_projects`:** on, as last confirmed live via the GitHub API — target is off.
- **Secrets stored:** none. Stats cards are unauthenticated public reads; no token is needed or present.

## Outstanding — verify and, where missing, apply

The environment that built and verifies this record can reach GitHub only
through a restricted API surface (repo creation, file commits, collaborator
and repository-metadata reads) with no route to branch-ruleset or
security-and-analysis administration endpoints. Confirm each of these
directly (Settings UI or an authenticated `gh`) and update this file to match
what you actually see — not what a command was expected to do:

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
done
```

Branch protection is now set up as a repository ruleset rather than the
classic branch-protection API (Settings → Rulesets) — for each repo, confirm
the ruleset actually targets the `main` branch (a ruleset with no target
applies to nothing) and carries: require a pull request before merging,
restrict deletions, block force pushes, and signed commits.

For `profileassets`, once its ruleset is targeted at `main`, also add the
`markdown-lint` and `link-check` check names as required status checks so a
PR cannot merge on a failing lint.

Re-verify after any change and update the section above with the live
result — do not mark a setting as applied without the confirming state in hand.
