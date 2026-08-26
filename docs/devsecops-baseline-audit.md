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