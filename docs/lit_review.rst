.. title:: Existing literature

.. only:: html

Existing literature
===================

.. note::

   Green CI/CD is an active area of research and development meaning the list
   of resources provided in this section is far from complete. It mainly serves
   to provide pointers to helpful references.

There are a few studies on this topic, for example
`Carbon Awareness in CI/CD (Classen et al, 2023)
<https://arxiv.org/abs/2310.18718>`__ and `Carbon-Aware Continuous Integration:
Reducing Emissions Without Sacrificing Performance (Laskar, 2025)
<https://lorojournals.com/index.php/emsj/article/view/1581>`__.
These mostly rely on scheduling workloads for low-carbon times or shifting to
low-carbon locations, as well as reducing overhead in the workloads (e.g.,
through minimisation of unnecessary runs and builds).

In terms of the GitHub Actions CI/CD framework specifically, the conference
poster `Environmentally-aware use of GitHub Actions (Alvarez, 2024)
<https://doi.org/10.5281/zenodo.12754188>`__ proposes good practices and
evaluates their impact on workflow time and the implied energy usage for an
example setup. `Environmental impact of CI/CD pipelines (Saavedra et al, 2025)
<https://arxiv.org/abs/2510.26413>`__ takes a step back and provides an
assessment of the environmental impact of the GitHub Actions infrastructure on
the whole.

Tools like `Eco CI
<https://github.com/green-coding-solutions/eco-ci-energy-estimation>`__ can help
measuring/estimating the energy consumption of CI/CD runs which can help making
informed decisions about adapting the runs. There are `a range of other tools
<https://github.blog/open-source/social-impact/the-10-best-tools-to-green-your-software/>`__
out there that can help you make your software development practices greener.
