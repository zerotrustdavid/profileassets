# profileassets

Supporting assets and tooling for the [zerotrustdavid](https://github.com/zerotrustdavid) GitHub profile.

## Contents

- `assets/`: the light and dark banner SVGs used in the profile README, switched automatically via GitHub's `prefers-color-scheme` markdown trick, and the Invision Solutions and PayReckon icons shown beside the "Building" entries. Each icon is drawn from the source geometry in its own product repository, so when either brand changes, update it there first and then refresh the copy here.
- `.github/workflows/lint.yml`: markdown lint and link-check on every pull request, with third-party actions pinned to a commit SHA rather than a floating tag.
- `SECURITY.md`: the security record for this repo.

Nothing here is required for the profile README to render. It lives in its own repo so that [zerotrustdavid/zerotrustdavid](https://github.com/zerotrustdavid/zerotrustdavid) stays minimal.
