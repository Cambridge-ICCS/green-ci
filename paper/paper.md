---
title: "Green software engineering practices for GitHub Actions continuous integration workflows"
tags:
  - continuous integration
  - continuous development
  - green software
  - jinja
authors:
  - name: Joseph G. Wallwork
    affiliation: "1"
    orcid: 0000-0002-3646-091X
    corresponding: true
  - name: Landung Setiawan
    affiliation: "2"
    orcid: 0000-0002-1624-2667
  - name: Marion Weinzierl
    affiliation: "1"
    orcid: 0000-0002-2302-5476
affiliations:
 - name: Institute of Computing for Climate Science, University of Cambridge, UK
   index: 1
 - name: Scientific Software Engineering Center, University of Washington, USA
   index: 2
date: 15 April 2026
bibliography: paper.bib

---

# Summary

Continuous integration (CI) and continuous deployment (CD) workflows form part
of the bread-and-butter of software development. They allow developers to
run test suites, static analysis tools, and build and deploy documentation and
websites whenever changes are made, thereby ensuring that everything remains
working as expected. However, while regular testing is good for software
sustainability, it isn't necessarily good for environmental sustainability.
Green CI/CD is concerned with minimising the resource usage by such workflows,
while maintaining the same coverage. That is, finding a minimal set of
conditions such that all code changes give rise to the appropriate responses.

The `green-ci` package documents best practices for Green CI/CD such as how to
configure timeouts, concurrency, and triggers. Examples are given for the case
of workflows set up using the popular GitHub Actions framework. The package also
provides a templating approach for setting up new GitHub Actions workflows that
follow Green CI/CD best practices.

# Statement of need

It is now well understood that tackling the climate crisis requires reducing
CO$_2$ emissions across all sectors, including software development. As
motivated above, CI/CD forms a key part of software development workflows and
should also be targeted.

## State of the field

Several green computing tools that are relevant to CI/CD, although these are
mostly focused on measurement. CodeCarbon [@CodeCarbon] is a Python package,
which estimates the carbon footprint of a computation based on the energy used
to run it and carbon intensity data for the region in which it is being
performed. EcoCI [@EcoCI] applies the same methodology to CI/CD workflows so
that users can estimate the carbon footprint of their CI/CD tasks.

There is some existing guidance on green practices for CI/CD, including general
guidance [@Claßen2023;@Alvarez2024] as well as guidance for the GitHub Actions
framework specifically [@Saavedra2025;@Laskar2025]. However, to the best of the
authors' knowledge, no tool currently exists which provides easily adoptable
workflow templates following such practices.

# Software design

The `green-ci` package was born out of a hackathon at the Virtual Institute for
Scientific Software Convening in Cambridge in September 2025. In the hackathon,
we established the statement of need above and set out to create a package which
meets this need. The package contains two main components: best practices
documentation and templates for green GitHub Actions workflows.

## Best practices documentation

The best practices documentation builds on the work of
[@Claßen2023;@Alvarez2024;@Saavedra2025;@Laskar2025], as well as best practices
established within our teams and by the Green Research Software Engineering
Special Interest Group (Green RSE SIG). This long-form documentation can be
found on the [`green-ci` webpage](https://cambridge-iccs.github.io/green-ci/).

## Templating

The templating approach is based on `copier` [@Copier]. The `green-ci`
repository includes several GitHub actions workflow templates making use of
Jinja templating engine. These provide various options for the user to specify, such as the
file extensions for source code, documentation, and build system files, timeouts
for different tasks, and additional options such as whether to enable carbon
footprint estimation with EcoCI.

Templates for test suites and static analysis workflows are included, so users can
verify that their source code continues to work as expected. While the name
only mentions CI, the `green-ci` package does apply to CD, too. In particular,
templates are provided for building documentation and webpage, and for rendering
JOSS papers.

### Usage

There is no need to clone the `green-ci` repository. Simply install `copier`
into your Python environment (e.g., with `pip` or `uv`) and run
```sh
copier copy https://github.com/Cambridge-ICCS/green-ci.git /path/to/my-project
```
from the command line, substituting `/path/to/my-project` for the path to the
project to which templates should be applied.

### Testing

The `green-ci` package is (of course) automatically tested with GitHub Actions.
Workflows are included for Jinja linting, building and deploying the
documentation and webpage, and for verifying that (this) JOSS submission renders
correctly. These workflows were themselves generated using `green-ci` templates
and a further workflow is included that checks that the linting, documentation,
and JOSS paper workflows can be generated from the templates. For added
recursion, the workflow checks that it can itself be generated from the test
suite template.

## Research impact statement

While `green-ci` is a new tool, it is beginning to be used in the community.
It was used to inform the continuous integration testing framework for the
Fortran-based machine learning package, *FTorch* [@FTorch].
The tool is also gathering GitHub stars.

# Future development

Currently, `green-ci` is restricted to GitHub Actions. In the future, we would
like to expand to support GitLab CI/CD and Bitbucket Pipelines.

# Acknowledgments

The Institute of Computing for Climate Science and Scientific Software
Engineering Center are supported by Schmidt Sciences, LLC. Thank you to members
of the Green RSE SIG for suggestions and feedback on best practices.

# AI usage disclosure

No AI tools were used in the writing or reviewing of this paper. GitHub Copilot
was used in a limited way to assist with debugging the testing setup.

# References
