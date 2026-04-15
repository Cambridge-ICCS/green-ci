.. title:: Best practices

.. only:: html

Best practices
==============

Time limits
^^^^^^^^^^^

By default, a job will continue running for 360 minutes (6 hours) before being
cancelled. This can be extremely wasteful in cases where the code has stalled,
for example. As such, it is good practice to provide a shorter time limit after
which the job will be cancelled. This should be an over-estimate, so that the
job will still pass when the code is working as expected.

Some example GitHub Actions workflow syntax for implementing a 10 minute time
limit on a job called ``test-ubuntu-serial`` with the ``ubuntu-latest`` runner
is as follows:

.. code-block:: yaml

  jobs:
    test-ubuntu-serial:
      runs-on: ubuntu-latest
      timeout-minutes: 10
      # <Further job definition>

In most cases, your job will hopefully complete and pass within the allotted
time limit. However, there may be cases where this doesn't happen. This could
happen due to:

1. There is an intermittent or random issue (e.g., connection issue to an web
   resource, hardware malfunction, cosmic ray). In such cases, it may be
   sufficient to retry the workflow manually by clicking the red cross
   indicating the workflow failure, selecting the offending job, and clicking
   the 'Re-run jobs' button near the top of the page. Then select 'Re-run failed
   jobs'. (See the `Rerun only failed tests
   <best_practices.html#rerun-only-failed-tests>`__ for more details.)
2. The job has stalled due to an issue on your branch. Addressing this will
   require `debugging <best_practices.html#debugging>`__ your change.
3. You made a change that just causes your tests or docs build to require longer
   to run. In this case, you'll need to increase the time limit.

Concurrency
^^^^^^^^^^^

Suppose you or one of your collaborators has triggered a GitHub Actions
workflow and it has jobs that are still running. If someone triggers the same
workflow again (e.g., by pushing a commit) then it usually doesn't make sense
for the original workflow to continue running, given that the subsequent one
corresponds to the more recent version of the branch. In such cases, it can make
sense to configure the ``concurrency`` so that in-progress jobs will be cancelled.
This can be achieved with the following syntax:

.. code-block:: yaml

  concurrency:
    group: ${{ github.workflow }}-${{ github.event.pull_request.number || github.ref }}
    cancel-in-progress: true

Fail-fast
^^^^^^^^^

Sometimes workflow jobs contain several steps that are executed in series, for
example both formatting checks and test suite runs. For such jobs, it is
important to order the steps such that failure is detected as soon as possible,
i.e., a *fail-fast* policy. Assuming that formatting checks execute signficantly
faster than running a test suite, failing fast is achieved by putting the
formatting check before the test suite run.

A good general ordering is as follows:

1. Formatting/style/linting checks
2. Other static analysis checks
3. Unit test suites
4. System/integration test suites

Triggers
^^^^^^^^

Suppose you want to make a small change such as fixing typos or updating
Markdown files. In a naive CI setup, this will trigger the full suite, involving
tasks such as running tests, building documentation, and running static analysis
tools. This can be extremely wasteful, especially for large repositories with
long-running test suites. One way to avoid this is to make use of *triggers*.
You might be familiar with triggers related to pushing to particular branches,
pushes to open PRs, and manual triggering from the `Actions
<https://github.com/Cambridge-ICCS/green-ci/actions>`__ tab, as demonstrated
below:

.. code-block:: yaml

  name: MyTestSuite

  on:
    # Triggers the workflow on pushes to the "main" branch, i.e., PR merges
    push:
      branches: [ "main" ]

    # Triggers the workflow on pushes to open pull requests
    pull_request:

    # Allows you to run this workflow manually from the Actions tab
    workflow_dispatch:

In the following, we restrict attention to ``pull_request`` triggers because it
usually makes sense to run the full workflow when merging into ``main`` and the
``workflow_dispatch`` option already allows for finer-grained control over
specific jobs. Returning to the case of updating a Markdown file, the test suite
above will still run if a commit is pushed to an open PR doing so. To avoid this
unnecessary test run, we can specify a list of ``paths`` for files that would
trigger the test suite when they are run. For example, in Python we could use:

.. code-block:: yaml

  name: MyPythonTestSuite

  on:
    # Triggers the workflow on pushes to open pull requests with code changes
    pull_request:
      paths:
        - '.github/workflows/test_suite_python.yml'
        - '**.py'
        - 'requirements.txt'

where ``test_suite_python.yml`` is the name of the workflow file itself. Here
the workflow will only be run if the file itself, any Python source files, or
the repository's ``requirements.txt`` dependencies file change.

For compiled languages such as C, it often also is a good idea to include
files related to the build system. For example:

.. code-block:: yaml

  name: MyCTestSuite

  on:
    # Triggers the workflow on pushes to open pull requests with code changes
    pull_request:
      paths:
        - '.github/workflows/test_suite_c.yml'
        - '**.c'
        - '**.h'
        - '**CMakeLists.txt'

for source code with extension ``.c``, header files with extension ``.h``, and
`CMake <https://cmake.org/>`__ build files.

.. warning::

   Often administrators configure repository settings such that certain (or all)
   workflows are required to pass (on the latest commit) before a PR is merged.
   In this case, untriggered workflows can become problematic. One way to get
   around this issue is to include ``pull_request_review`` triggers to any
   workflows that need to be run before merging so that they are triggered
   whenever a review is received. A downside of this workaround is that it can
   itself lead to unnecessary workflow runs.

Separation of concerns
^^^^^^^^^^^^^^^^^^^^^^

The information on `failing fast <best_practices.html#fail-fast>`__ above is
useful for workflows containing several stages, but further improvements can be
made if those stages are independent of one another. We can do this by applying
a separation-of-concerns approach to split the workflow into several workflows
and accounting for `triggers <best_practices.html#triggers>`__ in each of them.

In the example case of a Python code that uses the `ruff
<https://docs.astral.sh/ruff/>`__ static analysis tool for linting and
formatting, the triggers could take the form:

.. code-block:: yaml

  name: MyPythonStaticAnalysisWorkflow

  on:
    # Triggers the workflow on pushes to open pull requests with code changes
    pull_request:
      paths:
        - '.github/workflows/static_analysis_python.yml'
        - '**.py'

where ``static_analysis_python.yml`` is the filename of the workflow. Here, the
workflow will only be triggered when commits are pushed that include changes to
the workflow configuration or Python source code.

In the example case where Fortran documentation is built using `FORD
<https://github.com/Fortran-FOSS-Programmers/ford>`__, the triggers could take
the form:

.. code-block:: yaml

  name: BuildDocs

  on:
    # Triggers the workflow on pushes to open pull requests to main with documentation changes
    pull_request:
      paths:
        - '.github/workflows/build_docs_ford.yml'
        - '**.md'
        - 'pages/*'

The above can be extended to separate out CPU vs. GPU test suites, test suites
on different operating systems (e.g., Ubuntu, Mac, Windows), and `JOSS
<https://joss.theoj.org/>`__ paper rendering, for example. Having separated
concerns in this way, the overall number of jobs can be reduced, provided the
contributor doesn't modify several different parts of the repository in the same
change.

.. note::

   In some cases it can be a good thing for contributors to edit multiple
   different types of files in the same change. For example, it is good practice
   to update documentation in line with changes to source code.

Skip CI
^^^^^^^

GitHub Actions supports manually skipping of CI workflows that would be
triggered by ``push`` or ``pull_request`` by including any of the following
strings in a commit message:

* ``[skip ci]``
* ``[ci skip]``
* ``[no ci]``
* ``[skip actions]``
* ``[actions skip]``

The same warning applies as mentioned in the `Triggers
<best_practices.html#triggers>`__ section. As such, you should not use this
notation in the final commit included in a PR before requesting reviews.

See the `GitHub documentation page
<https://docs.github.com/en/actions/how-tos/manage-workflow-runs/skip-workflow-runs>`__
for more details.

Rerun only failed tests
^^^^^^^^^^^^^^^^^^^^^^^

When re-running tests in GitHub Actions from the `Actions
<https://github.com/Cambridge-ICCS/green-ci/actions>`__ tab, there are two
options: 'Re-run all tests' and 'Re-run failed tests'. The latter option is the
more energy efficient and so is preferred.

Some testing frameworks support similar features when running tests locally. For
example, `Pytest <https://docs.pytest.org/>`__ has ``pytest --last-failed`` (or
``pytest --lf``) and `CTest
<https://cmake.org/cmake/help/latest/manual/ctest.1.html>`__ has
``ctest --rerun-failed``. These can cut down both the energy consumption and
turn-around time when debugging code or test changes.

Debugging
^^^^^^^^^

If your code fails during a CI run, it sometimes can be hard to find the issue
without trying and pushing a series of fixes, which in turn will trigger the CI
to run each time - and thus waste energy. If the issue is not in the test suite
which can be easily rerun (for example, using `Pytest
<https://docs.pytest.org/>`__, a tool like `act
<https://github.com/nektos/act>`__ can be used to run your CI pipeline locally
in a container.

Alternatively, you can interact with your GitHub actions using `action-tmate
<https://github.com/mxschmitt/action-tmate>`__ (with tmate being a fork of
`tmux <https://github.com/tmux/tmux/wiki>`__). This enables you to use SSH
to connect with the machine that the actions are run on.

Test PRs
""""""""

If you have set up `triggers <best_practices.html#triggers>`__ properly, you
should look at the way you are debugging/changing PRs. Rather than making small
changes to a big PR and checking whether your CI runs through, it can be more
energy efficient to separate out a smaller PR with just those changes. This will
then (hopefully) trigger a smaller set of tests being rerun instead of all those
that were affected in the big PR.
