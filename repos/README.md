# repos/

Clone your product repositories here, one folder per repository, directly under `repos/`:

```bash
git clone https://github.com/your-org/backend.git repos/backend
git clone https://github.com/your-org/frontend.git repos/frontend
```

- Clone with **full history** (no `--depth`). The method reads `git log` and `git blame` to map a feature's commits;
  a shallow clone cuts that history. If you already have a shallow clone, run `git fetch --unshallow` in it.
- Setup finds repositories by looking for folders with `.git` at depth 2 or less from the workspace root, so
  `repos/<name>/` is found and `repos/<group>/<name>/` is not.
- Everything under `repos/` except this file is gitignored: product code never lands in the workspace repository.
- The agent treats these repositories as read-only. Pull them yourself when you want a fresh state.
