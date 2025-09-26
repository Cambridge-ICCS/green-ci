# Green Software Engineering Practices for GitHub Actions Continuous Integration (CI) workflows

In this repository, we encourage the use of green software engineering practices
in continuous integration (CI) workflows implemented with GitHub Actions. It
provides [documentation](#best-practices) and templates on how to optimise CI
pipelines to reduce energy consumption, minimise resource usage, and promote
sustainable software development practices. By adopting these strategies,
developers can reduce the carbon footprint of their work and contribute to
environmentally friendly software engineering while maintaining efficient and
reliable CI processes.

## Usage

*Work in progress*

## Best practices

### Time limits

*Work in progress* (https://github.com/Cambridge-ICCS/green-ci/issues/2)

### Concurrency

*Work in progress* (https://github.com/Cambridge-ICCS/green-ci/issues/3)

### Triggers and separation of concerns

Suppose you want to make a small change such as fixing typos or updating
Markdown files. In a naive CI setup, this will trigger the full suite, involving
tasks such as running tests, building documentation, and running static analysis
tools. This can be extremely wasteful, especially for large repositories with
long-running test suites. One way to avoid this is to make use of *triggers*.
You might be familiar with triggers related to pushing to particular branches,
pushes to open PRs, and manual triggering from the
[Actions](https://github.com/Cambridge-ICCS/green-ci/actions) tab, as
demonstrated below:
```yml
name: MyTestSuite

on:
  # Triggers the workflow on pushes to the "main" branch, i.e., PR merges
  push:
    branches: [ "main" ]

  # Triggers the workflow on pushes to open pull requests
  pull_request:

  # Allows you to run this workflow manually from the Actions tab
  workflow_dispatch:
```

### Test PRs

*Work in progress* (https://github.com/Cambridge-ICCS/green-ci/issues/5)
