# GitLab

Use `glab` for GitLab.com and self-managed GitLab. Shared workflow/authority: `SKILL.md`.

## Requirements

- Authentication: `glab auth status --hostname HOST`; never print tokens.
- Repository commands: `-R https://HOST/GROUP/REPO`. API: `--hostname HOST` plus verified project ID or URL-encoded full path; API calls do not inherit `-R`.
- Replace uppercase placeholders with verified values. Preserve nested groups.

## Limits

- Max attachment size is set per instance by admins; default 100 MB. `glab api --hostname HOST application/settings` shows `max_attachment_size` when permitted; otherwise assume 100 MB.
- MP4 and WebM render inline in notes.

## Commands

| Purpose | Command |
| --- | --- |
| MR snapshot | `glab mr view MR --output json` |
| Patch | `glab mr diff MR` |
| Upload | `glab api --hostname HOST --method POST projects/PROJECT_ID/uploads -F file=@demo.mp4` |
| Post demo | `glab mr note MR -m "$(cat BODY.md)"` |
| Confirm | `glab mr view MR --comments` |

- The upload response contains `markdown` (for example `![demo](/uploads/HASH/demo.mp4)`). Append it to the note body verbatim; the link is only valid inside that project.
- Upload with the target project's ID, not a fork's.

Docs: [markdown uploads](https://docs.gitlab.com/api/project_markdown_uploads), [mr note](https://docs.gitlab.com/cli/mr/note/).
