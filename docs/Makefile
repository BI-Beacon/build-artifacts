# Minimal makefile for Sphinx documentation
#

# Default build target is making single page html
all:	static-resources code-examples singlehtml

static-resources:
	test -z "`dot -Tpng architecture.gv -o_static/architecture.png 2>&1`"

code-examples:
	cd .. ; ./dist/generate-code-examples.sh

# You can set these variables from the command line.
SPHINXOPTS    =
SPHINXBUILD   = sphinx-build
SPHINXPROJ    = BI-Beacon
SOURCEDIR     = .
BUILDDIR      = _build

# Put it first so that "make" without argument is like "make help".
help:
	pipenv run $(SPHINXBUILD) -M help "$(SOURCEDIR)" "$(BUILDDIR)" $(SPHINXOPTS) $(O)

setup:	Pipfile
	pipenv --three install
	sudo apt install graphviz

clean:
	rm -rf _build *~ \#*\# 2>/dev/null

.PHONY: help Makefile clean setup

# Catch-all target: route all unknown targets to Sphinx using the new
# "make mode" option.  $(O) is meant as a shortcut for $(SPHINXOPTS).
.DEFAULT: Makefile
	pipenv run $(SPHINXBUILD) -M $@ "$(SOURCEDIR)" "$(BUILDDIR)" $(SPHINXOPTS) $(O)
