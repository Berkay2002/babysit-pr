# GitHub

Use `gh` for GitHub.com and GitHub Enterprise. Shared workflow/authority: `SKILL.md`.

## Requirements

- `gh --version` ≥ 2.99.0. Older versions lack `--attach`; report and stop rather than uploading elsewhere.
- Authentication: `gh auth status --hostname HOST`; never print tokens.
- Repository commands: `-R HOST/OWNER/REPO`. Replace uppercase placeholders with verified values.

## Limits

- Formats: MP4, MOV, WebM (video); PNG, JPEG, GIF, WebP, SVG (images). Playwright's `.webm` needs no conversion.
- Video size: 10 MB on Free plans, 100 MB on paid plans. Check the repository owner's plan when unsure; assume 10 MB.

## Commands

| Purpose | Command |
| --- | --- |
| PR snapshot | `gh pr view PR --json headRefOid,headRefName,url,title,body` |
| Patch | `gh pr diff PR` |
| Post demo | `gh pr comment PR --body-file BODY.md --attach demo.webm` |
| Confirm | `gh pr view PR --json comments --jq '.comments[-1].url'` |

- `--attach` repeats for multiple files and inserts the asset inline in the body.
- Inline player appears for `github.com/user-attachments/assets/...` URLs; external URLs do not embed.

Docs: [attaching files](https://docs.github.com/en/github-cli/github-cli/attaching-files-with-github-cli), [pr comment](https://cli.github.com/manual/gh_pr_comment).
