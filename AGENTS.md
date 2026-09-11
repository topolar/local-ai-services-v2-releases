# Local AI Services v2 Releases — project instructions

## Registry identity

- Project ID: `local-ai-services-releases`
- Canonical workdir: `/home/mistercz/projects/local-ai-services-releases`
- Repository: `topolar/local-ai-services-v2-releases`
- Registry status: `unconfirmed` (default/unknown registry state, not runtime health).
- Canonical registry: `/home/hermes/.hermes/shared/projects.json`; tracking issue: https://github.com/topolar/hermes-settings/issues/28

## Safe working scope

- This registry status is unconfirmed; do not infer that the project is active or that any service is healthy.
- This is the companion release repository for `local-ai-services`; confirm which repository owns the requested change.
- Before work, read this file, relevant nested `AGENTS.md`/project instructions for affected paths, the README, development docs, and manifests. Use only scripts and checks actually documented or present in the repository; do not invent test commands.
- Resolve the actual Git worktree root. If a task worktree has no local `AGENTS.md`, read the canonical root instructions rather than creating or overwriting instructions in every worktree.
- A registry row identifies local project context only; inspect Git state and real service boundaries separately. Report ambiguity instead of guessing.

<!-- projectctl-worktree-pointer-v1 -->
## Project-local worktrees

Load the shared project-context skill and resolve this repository in the shared
projects.json registry. Create/reuse task worktrees through projectctl under the
registered canonical root's .worktrees/, never relative to a linked worktree or
beside the project. Keep owner/task/session evidence in handoffs. Finish only
through its preservation and usage checks; cleanup remains dry-run. Read this
file and applicable nested instructions explicitly. Guidance is not a sandbox
and grants no new permission; existing roles and task approvals still apply.
