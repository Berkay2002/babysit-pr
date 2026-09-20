# PR Toolkit

Review, fix, and demonstrate pull request changes.

The display name is **PR Toolkit** in Codex and Claude Code. The plugin ID remains `babysit-pr`, so existing installation commands and skill invocations continue to work.

A plugin for [Claude Code](https://code.claude.com/docs/en/plugins) and [OpenAI Codex](https://developers.openai.com/plugins/build/plugins) that monitors a pull request or merge request, fixes scoped CI failures and actionable review feedback, and prepares it for merge. It merges only with explicit authorization.

A second skill, `pr-demo`, records a short screen video of the change working and attaches it to the PR. Ask for a demo on its own, or say "post a demo video when ready" while babysitting.

Supports GitHub via `gh` and GitLab via `glab`. Install the CLI for your provider and authenticate before using the skills. Attaching videos on GitHub needs `gh` 2.99.0 or newer.

## Install in Claude Code

Add this repository as a marketplace, then install the plugin:

```shell
/plugin marketplace add Berkay2002/babysit-pr
/plugin install babysit-pr@berkay
```

Then invoke the skill:

```shell
/babysit-pr:babysit-pr https://github.com/OWNER/REPO/pull/123
/babysit-pr:pr-demo https://github.com/OWNER/REPO/pull/123
```

Claude also picks the skills up automatically when you ask it to babysit a PR or MR, or to record a demo of one.

To try it without installing, run Claude Code with the plugin loaded from a local checkout:

```shell
claude --plugin-dir /path/to/babysit-pr
```

## Install in Codex

Add this repository as a marketplace, then install the plugin:

```shell
codex plugin marketplace add Berkay2002/babysit-pr
codex plugin add babysit-pr@berkay
```

Then invoke the skills with `$babysit-pr` or `$pr-demo` in Codex, or pick **PR Toolkit** from the Plugins Directory in the ChatGPT desktop app after adding the marketplace there.

## Usage

Ask the agent to babysit a PR or MR, optionally with a URL or number. It will:

1. Resolve the provider, repository, branches, and current head from live data.
2. Refresh CI, reviews, draft state, and mergeability.
3. Diagnose failures from logs and review threads, separating real defects from transient failures and out-of-scope requests.
4. Fix scoped defects, verify locally, and push.
5. Repeat until the PR is verified ready, then report the exact blocker or readiness.

Merge, force-push, reviewer messages, thread resolution, and draft or approval changes always require explicit authorization.

### Demo videos

Ask for a demo of a PR, or add "post a demo video when ready" to a babysit request. The agent will:

1. Check out the PR head and read the title, body, and diff.
2. Write a 3–6 step scenario showing the behavior the PR claims.
3. Record it with Playwright (web apps) or [VHS](https://github.com/charmbracelet/vhs) (terminal apps), under 60 seconds.
4. Attach the video to a PR comment via `gh pr comment --attach` or GitLab's uploads API, noting the recorded head SHA.

Desktop and mobile apps are not supported; the skill reports that and stops. Requesting a demo authorizes that one comment and nothing else.

## Layout

```text
babysit-pr/
├── plugin.json                       # Portable Agent Plugins manifest (Codex)
├── .claude-plugin/
│   ├── plugin.json                   # Claude Code plugin manifest
│   └── marketplace.json              # Claude Code marketplace (this repo)
├── .agents/plugins/marketplace.json  # Codex marketplace (this repo)
└── skills/
    ├── babysit-pr/
    │   ├── SKILL.md                  # Monitor and fix until merge-ready
    │   ├── agents/openai.yaml        # Codex skill presentation metadata
    │   └── references/
    │       ├── github.md             # gh commands and interpretation
    │       └── gitlab.md             # glab commands and interpretation
    └── pr-demo/
        ├── SKILL.md                  # Record and attach a demo video
        ├── agents/openai.yaml        # Codex skill presentation metadata
        └── references/
            ├── github.md             # gh --attach usage and limits
            ├── gitlab.md             # GitLab uploads API and MR notes
            └── recorders.md          # Playwright and VHS recipes
```

## License

MIT. See [LICENSE](LICENSE).
