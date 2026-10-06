# Manual updates for this EdgeEver fork

Upstream: `https://github.com/tianma-if/edgeever.git`.

This personal fork synchronizes upstream locally. Every retained Actions workflow uses only
`workflow_dispatch`; upstream updates require an operator or explicitly instructed AI.
Existing Cloudflare Git builds are allowed. Whether a push also deploys depends on the existing CF configuration.
Manual Actions may remain unavailable while account permissions are restricted.

Check `git status --short` and protect existing changes. Verify `origin` and `upstream` with `git remote -v`.
Fetch and inspect the selected upstream release or branch before merging:

```powershell
git switch main
git fetch upstream main
git log --oneline HEAD..upstream/main
git diff HEAD...upstream/main
git merge --no-commit --no-ff upstream/main
```

Inspect conflicts and every file in `.github/workflows`, including new upstream files. Preserve this fork's
manual policy; never reintroduce updater workflows, scheduled triggers, token-driven pushes, deploy hooks,
or chained workflow invocations. Abort uncertain merges with `git merge --abort`. Do not reset custom code.
Keep service-level business timers. Review the diff and run checks before separately authorizing a commit,
push, or deployment. Preserve instance configuration and back up data before deployment. Reuse existing
controlled credentials; do not create a PAT or expose secret values.

The user explicitly accepted CF automatic builds on 2026-10-07. Keep existing Git connections and build
settings. Before an authorized push, verify the target branch and build/deploy commands and explain its
production impact. Back up data before migration; avoid duplicate deployment after CF has already deployed.
This local change has not verified or modified live CF settings or GitHub OAuth/App authorization.
See [Pages controls](https://developers.cloudflare.com/pages/configuration/branch-build-controls/)
and [Workers Builds](https://developers.cloudflare.com/workers/ci-cd/builds/).

## Local checks and separate deployment

```powershell
bun install --frozen-lockfile
bun run typecheck
bun run typecheck:mobile
bun run build:web
```

Customized product merges also need existing tests. Ordinary deployment mirrors do not repeat upstream
platform tests. Inspect a stable upstream release tag instead of blindly following unreleased main.
Add the upstream remote once if it is absent. Never overwrite uncommitted migrations or private settings.

Prefer existing CF Workers Builds. After verifying controlled instance configuration, use the fallback
`bun run deploy:manual` only when local deployment is
explicitly requested. It builds, migrates, deploys, and verifies; it contains POSIX shell syntax and should
use an established WSL/Linux/Git Bash environment. The existing Windows project and its private
configuration were left untouched by this change.

Retained upstream packaging, signing, store, Demo, and official-site workflows are manual and remain
guarded to `tianma-if/edgeever`, so they skip in this fork. They do not deploy the personal Worker.
This fork's manual policy takes precedence over upstream instructions to enable its updater.
