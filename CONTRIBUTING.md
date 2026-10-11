# Contributing

[简体中文](CONTRIBUTING.zh-CN.md)

## Setting up

You need Docker, Node 22, pnpm and Python 3.9 or later.

```bash
docker compose up -d postgres storage
python3 scripts/fetch_cases.py
cd app && pnpm install && pnpm dev    # http://localhost:5173
```

"Working on the site" in [docs/self-hosting.md](docs/self-hosting.md#working-on-the-site) has the
rest: running a runner beside it, the model key the customer's people and grading need, and running
it as one process.

## Before you open a pull request

Run what CI runs (`.github/workflows/checks.yml`):

```bash
cd app && pnpm typecheck && pnpm test && pnpm build
python3 -m compileall -q harness scripts
```

- The tests that touch the records work in a database of their own in the local PostgreSQL, and are
  skipped when there is none. CI has one, and there a test that cannot reach it fails.
- The harness and the scripts are standard library only and promise Python 3.9: no dependencies,
  and nothing newer than 3.9 allows.
- If you changed how delivered code is started, the runner image, or how a case is graded, run
  `python3 scripts/smoke_cases.py` against a running site (see its header). A grade of 0 proves
  nothing by itself: check that the delivered system loaded.

## Branches and pull requests

- Branch from `dev` and open the pull request into `dev`. `main` takes changes only from `dev`.
- Both branches are protected: the `app` and `harness` checks have to pass, and nothing is pushed to
  them directly.
- One change to a pull request. Say what it does and how you checked it; for anything on a page, a
  screenshot helps.

## Cases

The cases are not in this repository and should stay out of it: no case files, no reference
solutions, and nothing in code, tests, screenshots or recordings that gives a case away. The three
open examples (`campus_booking`, `oncall_incident`, `robot_cell_rollout`) are the ones to use in
pictures and in documentation.

## Licence

What you contribute is under the Apache License 2.0, as the rest is (see `LICENSE`).
