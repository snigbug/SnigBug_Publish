# Notion Press — Kane CLI Assurance Exercise

This repository demonstrates the Kane CLI Assurance + Evidence lifecycle against a real self-publishing platform.

Application under test:
https://notionpress.com

## Learning objective

Demonstrate this lifecycle:

Requirement → Use Case → Acceptance Criteria → Scenario → test.md → Execution → Evidence → Coverage

## Repository layout

```text
Snigdha_Publish/
├── .github/
│   └── workflows/
│       └── notionpress-assurance.yml
├── requirements/
│   └── notionpress_requirements.md
├── .testmuai/
│   └── tests/
│       ├── verify-separate-isbn-pod-and-global-distribution-claims-for_test.md
│       ├── verify-supported-release-formats-and-publication-languages_test.md
│       └── verify-built-in-cover-and-interior-design-tools-are_test.md
├── .gitignore
└── README.md
```

## Important design decision

The assurance design phase is intentionally performed by a human on a workstation:

1. Ingest the requirements.
2. Extract use cases.
3. Review and trust the proposed use cases.
4. Design tests.
5. Review the generated acceptance criteria, scenarios and tests.
6. Commit the reviewed `_test.md` files.

GitHub Actions then performs the repeatable execution phase:

1. Install Kane CLI.
2. Authenticate using GitHub Secrets.
3. Run the committed `_test.md` suite.
4. Validate the generated evidence pack.
5. Upload the evidence and reports as workflow artifacts.

This avoids treating the `.context/` assurance store as a Git-mergeable artifact. The Kane CLI documentation describes `.context/` as append-only and single-writer, and recommends keeping it out of Git merges.

## Prerequisites

- TestMu AI account with Kane CLI access.
- TestMu AI username and access key.
- Node.js 18+.
- Google Chrome for local execution.
- Git and GitHub access.

Install Kane CLI:

```bash
npm install -g @testmuai/kane-cli
```

Verify:

```bash
kane-cli --version
```

Use Kane CLI 0.6.1 or later for the assurance commands.

## Phase 1 — Local assurance design

From the repository root:

```bash
kane-cli login
```

For a non-interactive login you can use:

```bash
kane-cli login --username "<username>" --access-key "<access-key>"
kane-cli config project 01J87QEHKVYWMP0W5E8K5CMXRA
```

### 1. Ingest requirements

```bash
kane-cli context ingest ./requirements/notionpress_requirements.md
```

This snapshots the requirement document into the local assurance store.

### 2. Extract use cases

```bash
kane-cli context extract
```

Kane proposes use cases from the requirement document and cites the source material. Against `notionpress_requirements.md` this currently proposes five use cases: choosing a publishing package, self-publishing a book globally, receiving royalty payouts, managing published-book operations and promotion, and browsing/purchasing published books.

### 3. Review

```bash
kane-cli context review
```

Review the proposed use cases. Promote the useful ones to trusted; edit or reject proposals where appropriate.

### 4. Design tests

For each trusted use case:

```bash
kane-cli design tests --use-case <USE_CASE_ID>
```

Review the generated acceptance criteria, scenarios and `_test.md` files. The `.testmuai/tests/` directory in this repository holds the reviewed test artifacts that are actually committed and run in CI — that's the kane-cli default output location, not a manually curated copy.

### 5. Review the design

Run:

```bash
kane-cli context review
```

Do not skip this step. The generated design is also derived content and should be reviewed before becoming part of the trusted test suite.

## Phase 2 — Local execution

First list the tests:

```bash
kane-cli testmd list
```

Run one test:

```bash
kane-cli testmd run ./.testmuai/tests/verify-supported-release-formats-and-publication-languages_test.md --agent
```

For CI-style local execution:

```bash
kane-cli testmd run ./.testmuai/tests/verify-supported-release-formats-and-publication-languages_test.md \
  --agent \
  --headless \
  --on-lock-conflict wait \
  --retry
```

Run the full suite:

```bash
kane-cli testrun run \
  --headless \
  --on-failure fail-fast
```

A batch `testrun` produces one sealed evidence pack for the suite.

## Phase 3 — Inspect evidence

After a run, look under:

```text
.testmuai/evidence/
```

Validate the pack:

```bash
kane-cli evidence validate .testmuai/evidence/<execution-id>.evidence --json
```

Serve it locally if you want to inspect it in the evidence viewer:

```bash
kane-cli evidence serve .testmuai/evidence/<execution-id>.evidence
```

The evidence pack contains the test definitions, results, screenshots, console/network logs and failure information.

`.testmuai/evidence/` itself is gitignored — kane-cli names each pack with a random execution id, so it's not something to commit as-is. CI copies the latest pack to `evidence/latest.evidence` (a fixed, non-ignored path, overwritten every run) and commits it, so the most recent run's full evidence is always in the repo without the history growing unbounded. Open it locally after pulling:

```bash
kane-cli evidence serve evidence/latest.evidence
```

## Phase 4 — GitHub Actions

Create the following GitHub repository secrets:

- `LT_USERNAME`
- `LT_ACCESS_KEY`

Then push the repository.

The workflow in `.github/workflows/notionpress-assurance.yml`:

1. Checks out the repository.
2. Installs Node.js.
3. Installs Kane CLI.
4. Logs into TestMu AI using GitHub Secrets.
5. Runs the committed Notion Press tests in headless mode.
6. Validates the generated evidence pack.
7. Commits the pack to `evidence/latest.evidence` (skipped on `pull_request` runs — see the workflow file's comment on why).
8. Writes pass/fail totals and the evidence pack id to the run's Step Summary.
9. Uploads evidence, `Result.md` files, test outputs, and the raw NDJSON logs as workflow artifacts.

## Recommended training demo

Do not make every scenario pass.

The committed suite already demonstrates this: of the three designed tests for "Self-publish a book globally," one (built-in design tools) passes cleanly, and two (ISBN/POD/distribution claims; supported formats and languages) fail against the live site — kane-cli's own bug-detection flags both as agent missteps in the verification step, not defects in Notion Press itself. Use them to walk through:

```text
Requirement
   ↓
Acceptance Criterion
   ↓
Scenario
   ↓
Test
   ↓
Execution
   ↓
Failure
   ↓
Evidence
```

Then explain the difference between:

- a product assertion that failed;
- an environment/test problem that prevented verification;
- evidence showing exactly what happened.

## Suggested training discussion

Ask the trainees:

1. Which requirement does this test prove?
2. Which acceptance criteria are covered?
3. What evidence proves the criterion?
4. If the test fails, is the product wrong or is the environment broken?
5. What remains unverified?
6. What happens to the suite if the requirement changes?

That is the core Assurance + Evidence lesson.
