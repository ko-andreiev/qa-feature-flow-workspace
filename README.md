# qa-feature-flow workspace

A ready-to-use QA workspace for [qa-feature-flow](https://github.com/ko-andreiev/qa-feature-flow): a phase-gated QA
workflow for one feature (Discovery → Business rules → Test cases → Pre-flight → Live verification → Triage), run by
an AI agent in Claude Code, with a human decision at the gate between phases.

Clone it, put your product repositories inside, start Claude Code, answer the setup interview, and run the method.

```
.
├── .claude/settings.json   enables the qa-feature-flow plugin for this folder
├── CLAUDE.md               your project's instructions for the agent (edit it)
├── repos/                  your product repositories (gitignored, read-only for the agent)
└── qa-work/                QA artifacts: notes, test cases, run journals (committed here)
```

## Five steps

**1. Get your own copy.** On GitHub, click **Use this template** → create a repository of your own (private is fine).
Or clone and detach from the template:

```bash
git clone https://github.com/ko-andreiev/qa-feature-flow-workspace.git my-qa-workspace
cd my-qa-workspace
git remote remove origin
```

This repository is where your QA artifacts get committed; add your own remote when you want one.

**2. Clone your product repositories into `repos/`**, with full history (no `--depth`):

```bash
git clone https://github.com/your-org/backend.git repos/backend
git clone https://github.com/your-org/frontend.git repos/frontend
```

Details in [`repos/README.md`](repos/README.md). Then edit [`CLAUDE.md`](CLAUDE.md): product name, one sentence about
it, the language of artifacts.

**3. Open the folder in Claude Code and trust it.** `.claude/settings.json` names the plugin's marketplace
(`ko-andreiev/qa-feature-flow` on GitHub) and enables the plugin, so Claude Code offers to install it when you trust
the folder. If it does not, install it yourself in the session:

```
/plugin marketplace add ko-andreiev/qa-feature-flow
/plugin install qa-feature-flow@qa-feature-flow
```

**4. Run setup and answer the interview** (5–10 minutes):

```
/qa-feature-flow:qa-feature-flow setup
```

The agent scans the folder read-only (repositories, stack, migrations, API docs, environment variable names), shows
what it found with sources, and asks only about what is missing, in a few blocks. Every question comes with a proposed
answer: reply `ok` to accept it, or correct it. **No secrets in chat:** name only the file and the variables that hold
credentials (for example `.env.qa` with `QA_ADMIN_EMAIL`, `QA_ADMIN_PASSWORD`); `.env` files are gitignored here.
After your OK it writes `feature-flow.toml` at the root and the templates and folders under `qa-work/`. This folder is
already a git repository, so setup does not need to `git init` anything: QA artifacts are committed here, product
repositories stay ignored.

**5. Run a feature:**

```
/qa-feature-flow:qa-feature-flow {feature} {platform}
```

`{feature}` is a ticket key or a feature name; `{platform}` is `web`, `mobile` or `both`. The agent runs one phase and
stops at its gate with a short report and a question. Answer it; then start a **fresh session** with the same command
for the next phase (state is read from files).

## Updating the method

The plugin comes from the package repository. To get a newer version, refresh the marketplace
(`/plugin marketplace update qa-feature-flow` in a session), then update the plugin. From a terminal in this folder:

```bash
claude plugin marketplace update qa-feature-flow
```

```bash
claude plugin update qa-feature-flow@qa-feature-flow --scope project
```

Then restart Claude Code in this folder. `/qa-feature-flow:qa-feature-flow setup check` compares your
`feature-flow.toml` with the current config contract and lists keys a newer version added.

## Other agents

The method is also an [Agent Skill](https://agentskills.io/specification) that other agents with a shell can run,
with the gates run by hand instead of hooks. See
[“Use in other agents”](https://github.com/ko-andreiev/qa-feature-flow#use-in-other-agents) in the package README.

## Licence

MIT — see [LICENSE](LICENSE).
