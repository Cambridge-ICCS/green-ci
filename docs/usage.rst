.. title:: Usage

.. only:: html

Usage
=====

This repository provides templates for GitHub Actions workflows that can be
used as a starting point for implementing green software engineering practices
in CI pipelines. To use the templates, use `Copier
<https://copier.readthedocs.io/en/stable/>`__ with instructions below. This can
be installed with ``pip install copier`` or ``uvx copier`` if you have `uv
<https://docs.astral.sh/uv/>`__ installed.

With Copier installed, you can apply the template by running the following:

.. code-block:: shell

  copier copy https://github.com/Cambridge-ICCS/green-ci.git /path/to/my-project

Note that there is no need to clone the ``green-ci`` repository.

You will be prompted to enter some information, such as the triggers for the
workflow, the timeout limits, and whether this workflow should be carbon
aware (extra monitoring for energy usage).
