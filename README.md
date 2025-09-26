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

### Triggers

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

In the following, we restrict attention to `pull_request` triggers because it
usually makes sense to run the full workflow when merging into `main` and the
`workflow_dispatch` option already allows for finer-grained control over
specific jobs. Returning to the case of updating a Markdown file, the test suite
above will still run if a commit is pushed to an open PR doing so. To avoid this
unnecessary test run, we can specify a list of `paths` for files that would
trigger the test suite when they are run. For example, in Python we could use:
```yml
name: MyPythonTestSuite

on:
  # Triggers the workflow on pushes to open pull requests with code changes
  pull_request:
    paths:
      - '.github/workflows/test_suite_python.yml'
      - '**.py'
      - 'requirements.txt'
```
where `test_suite_python.yml` is the name of the workflow file itself. Here the
workflow will only be run if the file itself, any Python source files, or the
repository's `requirements.txt` dependencies file change.

For compiled languages such as C, it would likely also be a good idea to include
files related to the build system. For example:
```yml
name: MyCTestSuite

on:
  # Triggers the workflow on pushes to open pull requests with code changes
  pull_request:
    paths:
      - '.github/workflows/test_suite_c.yml'
      - '**.c'
      - '**.h'
      - '**CMakeLists.txt'
```
for source code with extension `.c`, header files with extension `.h`, and CMake
build files.

### Separation of concerns

The information on [triggers](#triggers) above is useful but what if you have
workflows for other tasks than just tests, such as static analysis and
documentation builds? In such cases, it's good practice to implement separate
workflows with appropriate triggers.

In the example case of a Python code that uses the
[ruff](https://docs.astral.sh/ruff/) static analysis tool for linting and
formatting, the triggers could take the form:
```yml
name: MyPythonStaticAnalysisWorkflow

on:
  # Triggers the workflow on pushes to open pull requests with code changes
  pull_request:
    paths:
      - '.github/workflows/static_analysis_python.yml'
      - '**.py'
```
where `static_analysis_python.yml` is the filename of the workflow. Here, the
workflow will only be triggered when commits are pushed that include changes to
the workflow configuration or Python source code.

In the example case where Fortran documentation is built using
[FORD](https://github.com/Fortran-FOSS-Programmers/ford), the triggers could
take the form:
```yml
name: BuildDocs

on:
  # Triggers the workflow on pushes to open pull requests to main with documentation changes
  pull_request:
    paths:
      - '.github/workflows/build_docs_ford.yml'
      - '**.md'
      - 'pages/*'
```

The above can be extended to separate out CPU vs. GPU test suites, test suites
on different operating systems (e.g., Ubuntu, Mac, Windows), and
[JOSS](https://joss.theoj.org/) paper rendering, for example. Having separated
concerns in this way, the overall number of jobs can be reduced, provided the
contributor doesn't modify several different parts of the repository in the same
change.

> [!NOTE]
> In some cases it can be a good thing for contributors to edit multiple
> different types of files in the same change. For example, it is good practice
> to update documentation in line with changes to source code.

### Test PRs

*Work in progress* (https://github.com/Cambridge-ICCS/green-ci/issues/5)
