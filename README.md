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

Suppose you or one of your collaborators has triggered a GitHub Actions
workflow and it has jobs that are still running. If someone triggers the same
workflow again (e.g., by pushing a commit) then it usually doesn't make sense
for the original workflow to continue running, given that the subsequent one
corresponds to the more recent version of the branch. In such cases, it can make
sense to configure the `concurrency` so that in-progress jobs will be cancelled.
This can be achieved with the following syntax:
```yml
concurrency:
  group: ${{ github.workflow }}-${{ github.event.pull_request.number || github.ref }}
  cancel-in-progress: true
```

### Triggers and separation of concerns

*Work in progress* (https://github.com/Cambridge-ICCS/green-ci/issues/4)

### Test PRs

*Work in progress* (https://github.com/Cambridge-ICCS/green-ci/issues/5)
