# Makefile for building green-ci documentation

# You can set these variables from the command line
SPHINXOPTS		=
SPHINXBUILD		= sphinx-build
SPHINXPROJ		= green-ci
SOURCEDIR		= .
BUILDDIR		= build
PYTHON			= ${VIRTUAL_ENV}/bin/python3

help:
	@$(SPHINXBUILD) -M help "$(SOURCEDIR)" "$(BUILDDIR)" $(SPHINXOPTS) $(O)
	@echo "  \033[0;34minstall\033[0m     to install all requirements"

.PHONY: help clean

install:
	python3 -m pip install -r ../requirements-dev.txt

SPHINX_TARGETS = dirhtml singlehtml pickle json htmlhelp qthelp devhelp epub \
	text texinfo info gettext changes xml pseudoxml linkcheck

# Catch-all target: route all unknown targets to Sphinx using the new
# "make mode" option.  $(O) is meant as a shortcut for $(SPHINXOPTS).
$(SPHINX_TARGETS): Makefile
	@$(SPHINXBUILD) -M $@ "$(SOURCEDIR)" "$(BUILDDIR)" $(SPHINXOPTS) $(O)

html:
	$(SPHINXBUILD) -M $@ "$(SOURCEDIR)" "$(BUILDDIR)" $(SPHINXOPTS) $(O)

clean:
	-git clean -fdx $(BUILDDIR)/html/
	-rm -rf $(BUILDDIR)/doctrees
