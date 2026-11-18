# sample-nodejs-gitops

GitOps repository for the [sample-nodejs](https://github.com/einatsh/sample-nodejs) application.

## Repository layout

| Path | Purpose |
|---|---|
| `bootstrap/` | The only thing applied by hand with `kubectl`. |
| `argocd/` | Argo CD's own configuration, synced automatically by the root app. |
| `apps/` | Per-environment Helm values. |

### Sync policy
- `automated` sync with `prune` (delete what was removed from Git) and `selfHeal` (revert manual `kubectl` changes), so Git is the single source of truth
- `CreateNamespace=true`
- exponential-backoff `retry` for transient failures

### Immutable image tags
`image.tag` is the full commit SHA. The CI step `yq -i '.image.tag = "<sha>"'` is the only automated change to this repo.
