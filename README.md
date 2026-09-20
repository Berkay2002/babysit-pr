# Babysit PR

A plugin for [Claude Code](https://code.claude.com/docs/en/plugins) and [OpenAI Codex](https://developers.openai.com/plugins/build/plugins) that monitors a pull request or merge request, fixes scoped CI failures and actionable review feedback, and prepares it for merge. It merges only with explicit authorization.

Supports GitHub via `gh` and GitLab via `glab`. Install the CLI for your provider and authenticate before using the skill.

## Install in Claude Code

Add this repository as a marketplace, then install the plugin:

```shell
/plugin marketplace add Berkay2002/babysit-pr
/plugin install babysit-pr@babysit-pr
```

Then invoke the skill:

```shell
/babysit-pr:babysit-pr https://github.com/OWNER/REPO/pull/123
```

Claude also picks the skill up automatically when you ask it to babysit a PR or MR.

To try it without installing, run Claude Code with the plugin loaded from a local checkout:

```shell
claude --plugin-dir /path/to/babysit-pr
```

## Install in Codex

Add this repository as a marketplace, then install the plugin:

```shell
codex plugin marketplace add Berkay2002/babysit-pr
codex plugin add babysit-pr@babysit-pr
```

Then invoke the skill with `$babysit-pr` in Codex, or pick **Babysit PR** from the Plugins Directory in the ChatGPT desktop app after adding the marketplace there.

## Usage

Ask the agent to babysit a PR or MR, optionally with a URL or number. It will:

1. Resolve the provider, repository, branches, and current head from live data.
2. Refresh CI, reviews, draft state, and mergeability.
3. Diagnose failures from logs and review threads, separating real defects from transient failures and out-of-scope requests.
4. Fix scoped defects, verify locally, and push.
5. Repeat until the PR is verified ready, then report the exact blocker or readiness.

Merge, force-push, reviewer messages, thread resolution, and draft or approval changes always require explicit authorization.

## Layout

```text
babysit-pr/
├── plugin.json                       # Portable Agent Plugins manifest (Codex)
├── .claude-plugin/
│   ├── plugin.json                   # Claude Code plugin manifest
│   └── marketplace.json              # Claude Code marketplace (this repo)
├── .agents/plugins/marketplace.json  # Codex marketplace (this repo)
└── skills/babysit-pr/
    ├── SKILL.md                      # The skill
    ├── agents/openai.yaml            # Codex skill presentation metadata
    └── references/
        ├── github.md                 # gh commands and interpretation
        └── gitlab.md                 # glab commands and interpretation
```

## License

MIT. See [LICENSE](LICENSE).
