# PyGoat DevSecOps Baseline Audit

## Audit objective

Evaluate the existing GitHub Actions workflows before introducing additional
DevSecOps controls.

## Existing workflows

### Flake8

- Purpose: Python code-quality analysis
- Trigger: Pushes and pull requests involving the master branch
- Runner: Ubuntu
- Python version: 3.8
- Current status: Pending execution

### Hadolint

- Purpose: Dockerfile linting
- Trigger: Pushes and pull requests involving the master branch
- Runner: Ubuntu
- Current status: Pending execution

## Initial observations

- The workflows use older GitHub Action versions.
- The Flake8 description mentions tests, but no test command is present.
- The Hadolint workflow appears to scan the same Dockerfile twice.
- Neither workflow provides comprehensive application-security testing.

## Baseline execution results

To be completed after running the inherited workflows.

## Recommended improvements

To be completed after reviewing the workflow results.

### Flake8 findings

The inherited Flake8 workflow completed successfully but reported 1,383
code-quality violations. The majority involved whitespace, formatting,
line-length and blank-line conventions. The scan also identified findings
related to bare exception handling, wildcard imports, unused imports,
potentially undefined names and excessive function complexity.

The workflow remained successful because its broad analysis command uses
`--exit-zero`. Consequently, the workflow acts primarily as an informational
reporting control. It does not enforce remediation of the reported style and
complexity findings.

Given that PyGoat is an existing intentionally vulnerable educational
application, immediately enforcing every Flake8 rule would introduce
significant noise and could disrupt project development. A more practical
approach is to establish the current result as a baseline, prevent additional
quality debt and progressively remediate higher-value findings.

### Hadolint execution analysis

The Hadolint workflow completed successfully in approximately 11 seconds and
did not report any Dockerfile linting violations.

The audit confirmed that both scanning steps received `./Dockerfile` as their
input. Although the second step is named `Run hadolint on pygoat/Dockerfile`,
its configured path is identical to the first step. The workflow therefore
scans the root Dockerfile twice rather than scanning two distinct files.

Both executions also generated a warning stating that the GitHub Actions
`set-output` command is deprecated. The Hadolint action is pinned to an exact
commit, which improves reproducibility and reduces exposure to unexpected
upstream changes. However, the pinned revision uses an obsolete GitHub Actions
output mechanism and should be updated to a reviewed, maintained revision.

### Hadolint recommendations

- Remove the duplicate scanning step if only one Dockerfile exists.
- Correct the second path if another Dockerfile is intended to be scanned.
- Upgrade the Hadolint action to a reviewed, maintained release or commit.
- Continue pinning third-party actions to reviewed commit SHAs.
- Periodically review pinned actions for deprecations and security updates.

### Summary table
| Workflow | Result | Duration | Findings |
|---|---|---:|---|
| Flake8 | Passed | 51 seconds | Reported 1,383 violations and a deprecated action-runtime warning |
| Hadolint | Passed | 11 seconds | Scanned the same Dockerfile twice and generated a deprecated `set-output` warning |