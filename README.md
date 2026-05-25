# Development Guidelines

This repository documents standards and practices for all repositories under the [zehrer](https://github.com/zehrer) GitHub account.

> This repo is a documentation-only exception and may be committed to directly on `main`.

---

## Repository Setup Checklist

When creating a new repository, complete the following steps before the first commit:

- [ ] Add a `README.md` describing the project
- [ ] Add a `.gitignore` appropriate for the language/platform
- [ ] Set the default branch to `main`
- [ ] Enable branch protection (see below)

---

## Branch Protection

All repositories must have branch protection enabled on their default branch (`main` or `master`).

### Required rules

| Rule | Setting |
|------|---------|
| Require a pull request before merging | ✅ enabled |
| Required approving reviews | 0 (increase when team grows) |
| Dismiss stale reviews on new push | optional |
| Allow force pushes | ❌ disabled |
| Allow deletions | ❌ disabled |

### Applying protection via GitHub CLI

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

When CI is set up, add `required_status_checks` to prevent merging failing builds.

---

## Branching Strategy

- `main` — stable, protected. Never commit directly (except in this documentation repo).
- Feature branches — named `feature/<short-description>`
- Bug fixes — named `fix/<short-description>`

All changes go through a **pull request**. Keep PRs focused; one concern per PR.

---

## Commit Messages

Follow the [Conventional Commits](https://www.conventionalcommits.org/) format:

```
<type>: <short summary>

<optional body>
```

Common types: `feat`, `fix`, `docs`, `refactor`, `test`, `chore`

Examples:
```
feat: add mCLIChat interactive CLI for Nostr backend testing
fix: correct AES-CBC key label for Linux builds
docs: add branch protection guidelines
```

---

## CI / Status Checks

CI is not yet configured on most repositories. When added:

1. Update branch protection to require status checks before merging
2. Document the CI setup in the relevant repo's `README.md`
