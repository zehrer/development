# Branching & Commits

## Branching Strategy

| Branch pattern | Purpose |
|---------------|---------|
| `main` | Stable, protected. Never commit directly. |
| `feature/<short-description>` | New features |
| `fix/<short-description>` | Bug fixes |
| `docs/<short-description>` | Documentation-only changes |
| `chore/<short-description>` | Maintenance, dependency updates |

All changes reach `main` through a **pull request**. Keep PRs focused — one concern per PR.

## Commit Messages

Follow [Conventional Commits](https://www.conventionalcommits.org/):

```
<type>: <short summary>

<optional body explaining why, not what>
```

### Types

| Type | When to use |
|------|-------------|
| `feat` | A new feature |
| `fix` | A bug fix |
| `docs` | Documentation only |
| `refactor` | Code change that neither fixes a bug nor adds a feature |
| `test` | Adding or updating tests |
| `chore` | Build process, dependency updates, tooling |
| `ci` | CI/CD configuration changes |

### Examples

```
feat: add mCLIChat interactive CLI for Nostr backend testing
fix: correct AES-CBC key label for Linux builds
docs: add branch protection guidelines
chore: pin secp256k1.swift to 0.19.x
ci: add GitHub Pages deploy workflow
```

### Rules

- Summary line: 72 characters max, imperative mood ("add" not "added")
- No period at the end of the summary line
- Body: explain *why*, not *what* — the diff already shows what changed
