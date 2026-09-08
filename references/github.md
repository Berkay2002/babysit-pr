# GitHub

Use `gh` for GitHub.com and GitHub Enterprise. Shared workflow/authority: `SKILL.md`.

## Target

- Authentication: `gh auth status --hostname HOST`; never print tokens.
- Repository commands: `-R HOST/OWNER/REPO`. API: `--hostname HOST` plus explicit repository paths; API calls do not inherit `-R`.
- Distinguish target and fork source repositories before fetching/pushing.
- Replace uppercase placeholders with verified values. Check installed `--help` for flags/fields.

## Commands

Append repository selection above to every repository command.

| Purpose | Command |
| --- | --- |
| PR snapshot | `gh pr view PR --json headRefOid,headRefName,baseRefName,state,isDraft,mergeable,mergeStateStatus,reviewDecision,reviews,statusCheckRollup,url` |
| Patch | `gh pr diff PR` |
| Check snapshot | `gh pr checks PR --json name,state,bucket,link,workflow` |
| Required checks | `gh pr checks PR --required --json name,state,bucket,link` |
| Watch checks | `gh pr checks PR --watch --fail-fast --interval 30` |
| Exact run | `gh run view RUN_ID --json headSha,status,conclusion,jobs,url` |
| Watch run | `gh run watch RUN_ID --exit-status --compact --interval 30` |
| Failure logs | `gh run view RUN_ID --log-failed` |
| Retry failed jobs/dependencies | `gh run rerun RUN_ID --failed` |
| Authorized merge | `gh pr merge PR --match-head-commit SHA` |

## Interpretation

- `gh pr checks` exit 8 means pending. Separate check outcomes from CLI/auth/network errors.
- Missing required-check data does not establish absence of requirements; inspect repository rules.
- `reviewDecision`/`reviews` summarize reviews. Query full review threads, resolution, and comments via `gh api graphql --hostname HOST`. Outdated does not mean resolved.
- Paginate threads and nested comments; GraphQL requires cursor variables and collection `pageInfo`.
- REST collections: `gh api --hostname HOST --paginate`. REST reads with fields need `--method GET`; fields otherwise default to POST. GraphQL reads use queries, not mutations. Quote query variables safely or use an input file.
- Match CI to current head and target. For synthetic merge-queue commits, verify association rather than direct SHA equality; refresh after source/target changes.

## Mutation details

- Apply shared retry limit and check dependency-job effects. Specific-job reruns use `databaseId` from `gh run view RUN_ID --json jobs`, not an assumed browser URL job number.
- Select repository-approved merge strategy when needed. Never use `--admin`; branch deletion requires request.
- Queue/auto-merge acceptance is not completion. Follow required queue behavior, report queued state, and verify final merged state/commit.

Docs: [checks](https://cli.github.com/manual/gh_pr_checks), [watch](https://cli.github.com/manual/gh_run_watch), [API](https://cli.github.com/manual/gh_api), [merge](https://cli.github.com/manual/gh_pr_merge).
