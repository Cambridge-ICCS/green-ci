# Green Software Engineering Practices for GitHub Actions Continuous Integration (CI) workflows

In this repository, we encourage the use of green software engineering practices
in continuous integration (CI) workflows implemented with GitHub Actions. It
provides [documentation](#best-practices) and templates on how to optimise CI
pipelines to reduce energy consumption, minimise resource usage, and promote
sustainable software development practices. By adopting these strategies,
developers can reduce the carbon footprint of their work and contribute to
environmentally friendly software engineering while maintaining efficient and
reliable CI processes.

## Documentation

The documentation for `green-ci` is hosted at
[https://cambridge-iccs.github.io/green-ci/](https://cambridge-iccs.github.io/green-ci/).
The pages there include a review of the existing literature, best practices for
green CI, usage instructions for the `green-ci` templating tool, and information
on communities related to green software engineering more generally.

## Usage

This repository provides templates for GitHub Actions workflows that can be
used as a starting point for implementing green software engineering practices
in CI pipelines. To use the templates, use
[`copier`](https://copier.readthedocs.io/en/stable/) with instructions below.
You can then modify the workflow files to suit your specific needs,
such as changing the triggers, adding or removing jobs, and adjusting the time
limits.

You will need to `pip install copier`, then you can create a new module via:

```bash
copier copy https://github.com/Cambridge-ICCS/green-ci.git /path/to/my-project
```

> [!NOTE]
> You will be prompted to enter some information, such as the triggers for the
> workflow, the timeout limits, and whether this workflow should be carbon aware
> (extra monitoring for energy usage).

You can also use it via `uvx copier` if you have [`uv`](https://docs.astral.sh/uv/) installed.

## Contributing

The `green-ci` package is open source and is licensed under a permissive
[MIT license](LICENSE.md). We very much welcome contributions. Please read the
[CONTRIBUTING.md](./CONTRIBUTING.md) and
[CODE_OF_CONDUCT.md](./CODE_OF_CONDUCT.md) documents before starting your first
contribution.
