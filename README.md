<div align="center">
  <img src="assets/pr-toolkit.png" alt="PR Toolkit logo: a branching path leading to a play symbol" width="160" />
  <h1>PR Toolkit</h1>
  <p><strong>Get the PR ready. Show the change working.</strong></p>
  <p>Works with</p>
  <p>
    <a href="https://github.com/"><img src="assets/badges/github.svg" alt="GitHub" height="30" /></a>
    <a href="https://gitlab.com/"><img src="assets/badges/gitlab.svg" alt="GitLab" height="30" /></a>
    <a href="https://developers.openai.com/codex/"><img src="assets/badges/openai.svg" alt="Codex" height="30" /></a>
    <a href="https://code.claude.com/docs/en/overview"><img src="assets/badges/claude.svg" alt="Claude Code" height="30" /></a>
  </p>
  <p>Two skills for Codex and Claude Code, supporting GitHub and GitLab.</p>
  <p><a href="#install">Install</a> · <a href="#try-it">Try it</a> · <a href="#how-it-works">How it works</a> · <a href="#requirements">Requirements</a></p>
</div>

PR Toolkit helps your coding agent carry a pull request through failing checks and review feedback, then record a short demo of the result. Use either skill independently, or ask for a demo when the PR is ready.

| Skill | What you get |
| --- | --- |
| **[Babysit PR](skills/babysit-pr/SKILL.md)** | Scoped fixes, validation, and pushes, followed by a readiness report tied to the current PR head. |
| **[PR Demo](skills/pr-demo/SKILL.md)** | A short recording of the change working, attached to the PR with the recorded commit and steps shown. |

## Install

### Codex

Run in your terminal:

```sh
codex plugin marketplace add Berkay2002/babysit-pr
codex plugin add babysit-pr@berkay
```

Start a new session and invoke `$babysit-pr` or `$pr-demo`. The plugin appears as **PR Toolkit** in the plugin directory.

### Claude Code

Run inside Claude Code:

```text
/plugin marketplace add Berkay2002/babysit-pr
/plugin install babysit-pr@berkay
```

Restart Claude Code, then invoke `/babysit-pr:babysit-pr` or `/babysit-pr:pr-demo`.

> [!NOTE]
> The display name is **PR Toolkit**. The repository and plugin ID remain `babysit-pr`, so existing installation commands and skill invocations still work.

## Try it

Replace the example URL with your pull request or merge request. You can also refer to the current repository when the target is unambiguous.

**Get a PR ready for review or merge**

```text
Babysit https://github.com/OWNER/REPO/pull/123.
Fix CI failures and actionable review feedback, and tell me when it is ready.
```

**Show the change working**

```text
Record a short demo of https://github.com/OWNER/REPO/pull/123
and attach the video to the PR.
```

**Use both skills together**

```text
Babysit https://github.com/OWNER/REPO/pull/123
and post a demo video when it is ready. Leave merging to me.
```

To select a skill explicitly, prefix your request with `$babysit-pr` or `$pr-demo` in Codex, or the corresponding `/babysit-pr:...` command in Claude Code.

## How it works

### From failing checks to verified readiness

Babysit PR refreshes the PR head, checks, reviews, and merge requirements. It diagnoses failures from logs and feedback, makes fixes within the PR's scope, runs relevant validation, and pushes the changes. It then checks the new head again.

The result is either verified readiness or a specific blocker: pending checks, missing approvals, conflicts, unavailable access, or a decision that needs you. A green check alone does not establish readiness.

### From a change to a recorded demo

PR Demo reads the diff and builds a 3–6 step scenario, targeting a video of 60 seconds or less. It records web apps with Playwright or terminal apps with VHS, checks the recording, and posts it with the recorded head SHA and any omissions. If the PR head changes, the recording must be refreshed before posting.

When combined, the demo runs against the verified head after babysitting finishes and before any authorized merge.

> [!IMPORTANT]
> Babysitting permits scoped fixes, tests, and pushes. Merging, force-pushing, and review actions require explicit authorization. A demo request authorizes one comment containing the recording; the demo skill does not modify the branch.

## Requirements

- **Host:** Codex or Claude Code with plugin support.
- **Repository access:** Git and an authenticated [GitHub CLI](https://cli.github.com/) (`gh`) or [GitLab CLI](https://docs.gitlab.com/cli/) (`glab`), with permissions for the requested work.
- **Project setup:** The dependencies and services needed to run the project's checks or demonstrate the change.
- **Web demos:** Playwright and its browser runtime.
- **Terminal demos:** [VHS](https://github.com/charmbracelet/vhs), `ttyd`, and `ffmpeg` on your PATH. Recording verification uses `ffprobe` and `ffmpeg`.
- **GitHub video uploads:** `gh` 2.99.0 or newer with `gh pr comment --attach` support, as required by the demo skill.

Desktop and native mobile app recording are not supported. Mobile web layouts can be recorded through Playwright. Missing tools or access are reported as blockers; the skill does not silently install global tools or upload to another host.

Monitoring continues during the active run. Continuing after the session requires an automation facility supported by the host; installing the plugin alone does not create a background service.

## Update

**Codex — run in your terminal:**

```sh
codex plugin marketplace upgrade berkay
codex plugin add babysit-pr@berkay
```

**Claude Code — run in your terminal:**

```sh
claude plugin marketplace update berkay
claude plugin update babysit-pr@berkay
```

Restart the host or begin a new session to load the updated skills.

## Explore the skills

The plugin packages instructions and provider-specific references; it has no separate application server to deploy.

- [Babysit PR workflow](skills/babysit-pr/SKILL.md) · [GitHub reference](skills/babysit-pr/references/github.md) · [GitLab reference](skills/babysit-pr/references/gitlab.md)
- [PR Demo workflow](skills/pr-demo/SKILL.md) · [Recorder setup](skills/pr-demo/references/recorders.md) · [GitHub uploads](skills/pr-demo/references/github.md) · [GitLab uploads](skills/pr-demo/references/gitlab.md)

To try a local checkout in Claude Code:

```sh
git clone https://github.com/Berkay2002/babysit-pr.git
claude --plugin-dir ./babysit-pr
```
