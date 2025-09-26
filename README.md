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

By default, a job will continue running for 360 minutes (6 hours) before being
cancelled. This can be extremely wasteful in cases where the code has stalled,
for example. As such, it is good practice to provide a shorter time limit after
which the job will be cancelled. This should be an over-estimate, so that the
job will still pass when the code is working as expected.

Some example GitHub Actions workflow syntax for implementing a 10 minute time
limit on a job called `test-ubuntu-serial` with the `ubuntu-latest` runner is as
follows:
```yml
jobs:
  test-ubuntu-serial:
    runs-on: ubuntu-latest
    timeout-minutes: 10
    # <Further job definition>
```

In most cases, your job will hopefully complete and pass within the allotted
time limit. However, there may be cases where this doesn't happen. This could
happen due to:
1. There is an intermittent or random issue (e.g., connection issue to an web
   resource, hardware malfunction, cosmic ray). In such cases, it may be
   sufficient to retry the workflow manually by clicking the red cross
   indicating the workflow failure, selecting the offending job, and clicking
   the 'Re-run jobs' button near the top of the page.
2. The job has stalled due to an issue on your branch. Addressing this will
   require [debugging](#debugging) your change.
3. You made a change that just causes your tests or docs build to require longer
   to run. In this case, you'll need to increase the time limit.

### Concurrency

*Work in progress* (https://github.com/Cambridge-ICCS/green-ci/issues/3)

### Triggers and separation of concerns

*Work in progress* (https://github.com/Cambridge-ICCS/green-ci/issues/4)

### Test PRs

*Work in progress* (https://github.com/Cambridge-ICCS/green-ci/issues/5)

## Debugging

*Work in progress (https://github.com/Cambridge-ICCS/green-ci/issues/16)*
