#  SPDX-License-Identifier: BSD-3-Clause
#  Copyright (C) 2015 Intel Corporation
#  All rights reserved.
#

SPDK_ROOT_DIR := $(abspath $(CURDIR)/..)
DOXYFILE ?= Doxyfile
# Has to match OUTPUT_DIRECTORY in $(DOXYFILE).
OUTPUT_DIRECTORY ?= output

all: doc
	@:

.PHONY: all doc jsonrpc clean changelog.md

doc: $(OUTPUT_DIRECTORY)

deprecation.md: ../deprecation.md
	$(Q)sed -e 's/^# Deprecation/# Deprecation {#deprecation}/' \
	    < $< > $@

changelog.md:
	$(Q)$(SPDK_ROOT_DIR)/scripts/changelog.py --dry-run > $@.tmp
	$(Q)sed -e 's/^# Changelog/# Changelog {#changelog}/' \
	    -e 's/^##/#/' \
	    -e 's/^# \(\(v..\...\):.*\)/# \1 {#changelog-\2}/' \
	    -e '/# v..\...:/s/\./-/2' \
	    < $@.tmp > $@
	$(Q)rm -f $@.tmp

jsonrpc: $(SPDK_ROOT_DIR)/doc/jsonrpc.md

$(SPDK_ROOT_DIR)/doc/jsonrpc.md: $(SPDK_ROOT_DIR)/scripts/genrpc.py $(SPDK_ROOT_DIR)/schema/schema.yaml $(SPDK_ROOT_DIR)/doc/jsonrpc.md.jinja2
	$(Q)$(SPDK_ROOT_DIR)/scripts/genrpc.py --schema $(SPDK_ROOT_DIR)/schema/schema.yaml --doc > $@

$(OUTPUT_DIRECTORY): jsonrpc Doxyfile changelog.md deprecation.md $(shell find . -name '*.md' -not -path './output/*') $(wildcard ../include/spdk/*.h)
	$(Q)rm -rf $@
	$(Q)doxygen "$(DOXYFILE)"

clean:
	$(Q)rm -rf $(OUTPUT_DIRECTORY) changelog.md changelog.md.tmp deprecation.md jsonrpc.md
