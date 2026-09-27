# SippShip 0.3

**Change, with evidence.** SippShip turns documented requirements into repeatable checks, compares real Git revisions, and verifies bounded repairs against the same accepted checks.

This release moves beyond the original order-app demonstration. It includes a browser workspace, a Python API and persistent database, local/GitHub repository import, Python and JavaScript test profiles, IBM Bob MCP tools, optional model APIs, signed GitHub webhooks, and evidence exports.

Start with **[docs/START_HERE.md](docs/START_HERE.md)**. For the full product walkthrough, use **[docs/PRODUCT_GUIDE.md](docs/PRODUCT_GUIDE.md)**.

```bash
python -m pip install -r requirements.txt
python -m pip install --no-deps -e .
docker build -t sippship-runner:py312 -f docker/Dockerfile .
sippship configure-bob
sippship start
```

Use Python **3.12**. Run the commands inside your virtual environment. Windows commands are in the setup guide. No frontend build or npm installation is required to launch the workspace.

## What works

- Import committed local branches/SHAs or an open GitHub PR, including fork PRs through the base repository's PR ref.
- Retrieve source and documentation with exact line citations.
- Propose multiple requirement checks with Bob, an OpenAI-compatible chat API, or Anthropic's Messages API.
- Freeze generated checks **and the base's existing tests**, then run both on base, candidate and each repair.
- Distinguish regressions, baseline failures, incomplete execution and passing selected checks.
- Protect requirements, tests, dependency/configuration files and owner policy from repair changes.
- Configure automatic check acceptance and bounded repair attempts per project; ambiguity and protected-file changes stop automation.
- Reuse accepted checks when the cited documents still match.
- Trigger reviews from signed GitHub PR webhooks; deduplicate delivery IDs.
- Publish a commit status and evidence comment explicitly, with a fresh PR revision check.
- Export the accepted checks, source snapshot, repair patch, exact commit IDs, logs and JSON/Markdown evidence.

The product does **not** merge PRs. A passing local repair does not mark the unchanged GitHub PR as passing. The owner applies the exported patch and reviews the resulting commit again.

## Two ways to use AI

**Bob workflow:** create a review, copy its Bob prompt, use SippShip Reviewer mode, accept the proposed checks, compare, authorize repair, then use SippShip Fixer mode. Bob supplies the reasoning; no additional model API key is needed.

**API automation:** configure a model endpoint and project policy, then click Run automation or enable signed PR webhooks. The API loop generates checks and attempts repairs within the stored policy. You do not need to approve each step when its project policy already authorizes it.

SippShip is an orchestration and evidence layer around coding models. It does not claim that GPT/Claude agents cannot edit or test code, or that this approach is unique.

## Scope and validation

This is a **single-owner local product release**, not a hardened public SaaS. Supported execution profiles are Python/pytest and JavaScript/node:test. Dependencies must already exist in a trusted runner image. Networked integration tests, arbitrary languages and unrestricted hostile repositories need additional isolation and profiles.

See [docs/PRODUCT_SCOPE.md](docs/PRODUCT_SCOPE.md) for the limitations addressed and those that remain. See [docs/VALIDATION.md](docs/VALIDATION.md) for actual test results and unverified integrations.

## Development

```bash
python -m pip install -r requirements-dev.txt
python -m pytest
python -m ruff check src tests
node --test tests/frontend.test.cjs
python -m build --no-isolation
```

Node.js 24 is used for the Node execution tests and frontend logic checks. The browser frontend itself runs ordinary HTML, CSS and JavaScript.

[blueprint.md](blueprint.md) describes the current architecture. [docs/CODE_MAP.md](docs/CODE_MAP.md) explains the source files. The original fixture demo remains available with `sippship demo` and `sippship demo-ui`.

## Hackathon provenance

This source package was prepared outside IBM Bob. Use Bob for actual implementation, review, testing and iteration, and record what it does. Follow your event's rules for external assistance. Do not claim the package or sample fixture checks were generated in Bob.

The supplied kickoff slides require a video, problem/solution statement, Bob usage statement, public repository link, and every member's real Bob task-session summary screenshots. Templates are in `submission/`; add actual screenshots under `evidence/bob/`. Nothing in this package fabricates Bob usage or measured time savings.
