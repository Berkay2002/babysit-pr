# GitLab

Use `glab` for GitLab.com and self-managed GitLab. Shared workflow/authority: `SKILL.md`.

## Target

- Authentication: `glab auth status --hostname HOST`; never print tokens.
- Repository commands: `-R https://HOST/GROUP/REPO`. API: `--hostname HOST` plus verified project ID or encoded full project path; API calls do not inherit `-R`.
- Preserve nested groups. MR number belongs to target project; pipeline/jobs may belong to a fork's source project.
- Replace uppercase placeholders with verified values. Check installed `--help` for flags/fields.

## Commands

Append repository selection above to every repository command; use the pipeline/job's owning project.

| Purpose | Command |
| --- | --- |
| MR snapshot | `glab mr view MR --output json` |
| Patch | `glab mr diff MR` |
| Unresolved discussions | `glab mr view MR --unresolved --output json` |
| Exact pipeline/jobs | `glab ci get --pipeline-id PIPELINE_ID --output json --with-job-details` |
| Branch snapshot | `glab ci status --branch BRANCH --output json` |
| Watch branch pipeline | `glab ci status --branch BRANCH --live` |
| Job log | `glab ci trace JOB_ID` |
| Retry exact job | `glab ci retry JOB_ID --pipeline-id PIPELINE_ID` |
| Authorized immediate merge | `glab mr merge MR --sha SHA --auto-merge=false --yes` |

## Interpretation

- Resolve MR-associated pipeline first; prefer exact IDs. Branch live mode does not pin an MR/pipeline. Verify association before relying on it.
- JSON output cannot combine with `--live`/`--compact`. Never request `--with-variables` for monitoring.
- Apply project rules for manual/allowed-to-fail jobs; never trigger manual deployments merely to clear status.
- For merged-result/merge-train synthetic commits, verify association with current source head and target rather than direct SHA equality. Unknown association blocks readiness.
- Use `glab api --hostname HOST` for complete discussions, approvals, MR pipelines, and mergeability. Respect host API/edition/permission differences.
- Paginate discussions, notes, pipelines, and jobs. First comments page is incomplete evidence. Inspect resolvable/resolved state and actual notes. Eligible approvers do not establish granted approvals.
- REST reads with fields need `--method GET`; fields otherwise default to POST. GraphQL reads use queries, not mutations. Quote variables safely or use an input file.

## Mutation details

- Retry exact job IDs in their owning project; inspect external effects and apply shared retry limit.
- Installed CLI defaults auto-merge to true: choose mode explicitly. `--yes` skips CLI confirmation, not user authorization.
- Use repository-approved merge strategy; source-branch deletion requires request.
- Select auto-merge/train mode deliberately when authorized intent requires it. Report queued state; verify final merged state/commit.

Docs: [status](https://docs.gitlab.com/cli/ci/status/), [get](https://docs.gitlab.com/cli/ci/get/), [API](https://docs.gitlab.com/cli/api/), [merge](https://docs.gitlab.com/cli/mr/merge/).
