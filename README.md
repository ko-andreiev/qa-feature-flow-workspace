# qa-feature-flow workspace

A ready-to-use QA workspace for [qa-feature-flow](https://github.com/ko-andreiev/qa-feature-flow): a phase-gated QA
workflow for one feature (Discovery → Business rules → Test cases → Pre-flight → Live verification → Triage), run by
an AI agent in Claude Code, with a human decision at the gate between phases.

Clone it, put your product repositories inside, start Claude Code, answer the setup interview, and run the method.

```
.
├── .claude/settings.json   enables the qa-feature-flow plugin for this folder
├── .mcp.json               offers the Playwright browser for live checks of web pages (you approve it at first start)
├── AGENTS.md               tells any agent what to do here: state at start, commands, rules (no editing needed)
├── CLAUDE.md               imports AGENTS.md for Claude Code
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

Details in [`repos/README.md`](repos/README.md). There is nothing to fill in by hand: the product name, the language
and the rest come from the setup interview in step 4 and live in `feature-flow.toml`.

**3. Open the folder in Claude Code and trust it.** `.claude/settings.json` names the plugin's marketplace
(`ko-andreiev/qa-feature-flow` on GitHub) and enables the plugin, so when you trust the folder Claude Code adds the
marketplace and loads the plugin by itself, without asking. Check it: type `/qa-feature-flow` and the command
`/qa-feature-flow:qa-feature-flow` shows up. If it does not, install it yourself in the session:

```
/plugin marketplace add ko-andreiev/qa-feature-flow
/plugin install qa-feature-flow@qa-feature-flow
```

**Approve the `playwright` browser when Claude Code asks.** `.mcp.json` offers it as a project MCP server, so the
first time you open the folder Claude Code asks whether to use it: approve it. Phase 5 (live verification) clicks
through your web pages with it, including the browser's own `alert` / `confirm` / `prompt` dialogs. Without a browser
tool setup writes `channels.web.tool = "none"` and every case that happens on a web page becomes a manual card for
you. It needs Node.js (`npx`); it uses your installed Google Chrome, headless (delete `--headless` in `.mcp.json` to
watch it). Check it with `/mcp`: `playwright` shows as connected. If you skipped the question, run `/mcp` and enable it
there, then restart Claude Code.

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

If the plugin was loaded automatically in step 3, the update answers "Plugin is not installed": register it once with
`claude plugin install qa-feature-flow@qa-feature-flow --scope project` (this leaves `.claude/settings.json`
unchanged), and the update works from then on. Then restart Claude Code in this folder. `/qa-feature-flow:qa-feature-flow setup check` compares your
`feature-flow.toml` with the current config contract and lists keys a newer version added.

## Other agents

The method is also an [Agent Skill](https://agentskills.io/specification) that other agents with a shell can run.
[`AGENTS.md`](AGENTS.md) is the one instruction file for all of them (Codex and Cursor read it directly, Claude Code
through `CLAUDE.md`): at the start of a session the agent tells you whether the workspace is set up and which runs are
unfinished, and it maps "test feature X", "continue" or "status" to the right command. Codex and Cursor need the skill
installed first, per the adapter READMEs in the package. See
[“Use in other agents”](https://github.com/ko-andreiev/qa-feature-flow#use-in-other-agents) in the package README.

## Licence

MIT — see [LICENSE](LICENSE).
