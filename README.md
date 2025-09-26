# Green Software Engineering Practices for GitHub Actions Continuous Integration (CI) workflows

In this repository, we encourage the use of green software engineering practices
in continuous integration (CI) workflows implemented with GitHub Actions. It
provides [documentation](#best-practices) and templates on how to optimise CI
pipelines to reduce energy consumption, minimise resource usage, and promote
sustainable software development practices. By adopting these strategies,
developers can reduce the carbon footprint of their work and contribute to
environmentally friendly software engineering while maintaining efficient and
reliable CI processes.

## Existing Literature

There are a couple of studies on this topic, for example [Carbon Awareness in CI/CD by Classen et al](https://arxiv.org/abs/2310.18718)
and [Carbon-Aware Continuous Integration: Reducing Emissions Without Sacrificing Performance by Laskar](https://lorojournals.com/index.php/emsj/article/view/1581).
These mostly rely on scheduling workloads for low-carbon times or shifting to low-carbon locations, as well as reducing overhead in the workloads (e.g., through
minimisation of unnecessary runs and builds).

Tool like [Eco CI](https://github.com/green-coding-solutions/eco-ci-energy-estimation) can help measuring/estimating the energy consumption of CI/CD runs which can help 
making informed decisions about adapting the runs. There are [a range of other tools](https://github.blog/open-source/social-impact/the-10-best-tools-to-green-your-software/) out there
that can help you make your software development practices greener.

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

## Debugging

If your code fails during a CI run, it sometimes can be hard to find the issue without trying and pushing a series of fixes, which in turn will trigger the CI to run each time - and thus waste energy.
If the issue is not in the test suit which can be easily rerun (for example, using [pytest](https://docs.pytest.org/), a tool like [act](https://github.com/nektos/act) can be used to run your CI pipeline locally in a container.

Alternatively, you can interact with your Github actions using [action-tmate](https://github.com/mxschmitt/action-tmate) (with tmate being a fork of [tmux](https://github.com/tmux/tmux/wiki)). This enables you to use ssh to connect with the machine that the actions are run on.


*Work in progress (https://github.com/Cambridge-ICCS/green-ci/issues/16)*
