#  SPDX-License-Identifier: BSD-3-Clause
#  Copyright (C) 2015 Intel Corporation
#  All rights reserved.
#

SPDK_ROOT_DIR := $(abspath $(CURDIR)/..)
DOXYFILE ?= Doxyfile

DOC_EXCLUDE := output theme img
FIND_PRUNE  := $(foreach d,$(DOC_EXCLUDE),-not -path './$d/*')
DOC_SOURCES := $(shell find . $(FIND_PRUNE) -name '*.md') \
	$(shell find ../include/spdk -name '*.h')
DOC_ASSETS := $(DOXYFILE) $(shell find theme img -type f)

all: doc
	@:

.PHONY: all doc jsonrpc clean

doc: output

deprecation.md: ../deprecation.md
	$(Q)sed -e 's/^# Deprecation/# Deprecation {#deprecation}/' \
	    < $< > $@

changelog.md: ../CHANGELOG.md
	$(Q)sed -e 's/^# Changelog/# Changelog {#changelog}/' \
	    -e 's/^##/#/' \
	    -e 's/^# \(\(v..\...\):.*\)/# \1 {#changelog-\2}/' \
	    -e '/# v..\...:/s/\./-/2' \
	    < $< > $@

jsonrpc: $(SPDK_ROOT_DIR)/doc/jsonrpc.md

$(SPDK_ROOT_DIR)/doc/jsonrpc.md: $(SPDK_ROOT_DIR)/scripts/genrpc.py $(SPDK_ROOT_DIR)/schema/schema.json $(SPDK_ROOT_DIR)/doc/jsonrpc.md.jinja2
	$(Q)$(SPDK_ROOT_DIR)/scripts/genrpc.py --schema $(SPDK_ROOT_DIR)/schema/schema.json --doc > $@

output: $(SPDK_ROOT_DIR)/doc/jsonrpc.md changelog.md deprecation.md $(DOC_SOURCES) $(DOC_ASSETS)
	$(Q)rm -rf $@
	$(Q)doxygen $(DOXYFILE)

clean:
	$(Q)rm -rf output changelog.md deprecation.md jsonrpc.md
