#
# Licensed to the Apache Software Foundation (ASF) under one
# or more contributor license agreements. See the NOTICE file
# distributed with this work for additional information
# regarding copyright ownership. The ASF licenses this file
# to you under the Apache License, Version 2.0 (the
# "License"); you may not use this file except in compliance
# with the License. You may obtain a copy of the License at
#
#   http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing,
# software distributed under the License is distributed on an
# "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
# KIND, either express or implied. See the License for the
# specific language governing permissions and limitations
# under the License.
#

ACLOCAL_AMFLAGS = -I ./aclocal

SUBDIRS = compiler/cpp lib

if WITH_TESTS
SUBDIRS += test
endif

if WITH_TUTORIAL
SUBDIRS += tutorial
endif

dist-hook:
	find $(distdir) -type f \( -iname ".DS_Store" -or -iname "._*" -or -iname ".gitignore" \) | xargs rm -rf
	find $(distdir) -type d \( -iname ".deps" -or -iname ".libs" \) | xargs rm -rf
	find $(distdir) -type d \( -iname ".svn" -or -iname ".git" \) | xargs rm -rf

print-version:
	@echo $(VERSION)

.PHONY: precross cross
precross-%: all
	$(MAKE) -C $* precross
precross: all precross-test precross-lib

# TODO: generate --server and --client switches from "--with(out)-..." build flags


empty :=
space := $(empty) $(empty)
comma := ,

CROSS_LANGS = @MAYBE_CPP@ @MAYBE_C_GLIB@ @MAYBE_JAVA@ @MAYBE_CSHARP@ @MAYBE_PYTHON@ @MAYBE_RUBY@ @MAYBE_HASKELL@ @MAYBE_PERL@ @MAYBE_PHP@ @MAYBE_GO@ @MAYBE_NODEJS@
CROSS_LANGS_COMMA_SEPARATED = $(subst $(space),$(comma),$(CROSS_LANGS))

cross: precross
	$(PYTHON) test/test.py -s --server $(CROSS_LANGS_COMMA_SEPARATED) --client $(CROSS_LANGS_COMMA_SEPARATED)

TIMES = 1 2 3
fail: precross
	$(PYTHON) test/test.py || true
	$(PYTHON) test/test.py --update-expected-failures=overwrite
	$(foreach var,$(TIMES),test/test.py -s || true;test/test.py --update-expected-failures=merge;)

codespell_skip_files = \
	*.jar \
	*.class \
	*.so \
	*.a \
	*.la \
	*.o \
	*.p12 \
	*OCamlMakefile \
	.keystore \
	.truststore \
	CHANGES \
	config.sub \
	configure \
	depcomp \
	libtool.m4 \
	output.* \
	rebar \
	thrift

skipped_files = $(subst $(space),$(comma),$(codespell_skip_files))

style-local:
	codespell --write-changes --skip=$(skipped_files) --disable-colors

EXTRA_DIST = \
	.clang-format \
	.editorconfig \
	.travis.yml \
	appveyor.yml \
	bower.json \
	build \
	CMakeLists.txt \
	composer.json \
	contrib \
	CONTRIBUTING.md \
	debian \
	doc \
	doap.rdf \
	json-schema.json \
	package.json \
	rat_exclude \
	sonar-project.properties \
	LICENSE \
	CHANGES \
	NOTICE \
	README.md
