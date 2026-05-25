# Branch Protection

All repositories must have branch protection enabled on their default branch (`main` or `master`) before any code is merged.

## Required Rules

| Rule | Setting |
|------|---------|
| Require a pull request before merging | ✅ enabled |
| Required approving reviews | 0 (increase when team grows) |
| Dismiss stale reviews on new push | optional |
| Allow force pushes | ❌ disabled |
| Allow deletions | ❌ disabled |
| Require status checks before merging | ❌ until CI is configured |

## Applying Protection via GitHub CLI

```bash
gh api repos/zehrer/<REPO>/branches/main/protection \
  --method PUT \
  -H "Accept: application/vnd.github+json" \
  --input - <<'EOF'
{
  "required_status_checks": null,
  "enforce_admins": false,
  "required_pull_request_reviews": {
    "required_approving_review_count": 0,
    "dismiss_stale_reviews": false
  },
  "restrictions": null,
  "allow_force_pushes": false,
  "allow_deletions": false
}
EOF
```

## Adding Status Checks Once CI Is Set Up

When a CI workflow exists, update `required_status_checks` to enforce it:

```json
"required_status_checks": {
  "strict": true,
  "contexts": ["build"]
}
```

## Exceptions

| Repository | Reason |
|------------|--------|
| [development](https://github.com/zehrer/development) | Documentation-only, commits directly to `main` |
