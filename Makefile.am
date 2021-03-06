## Makefile for Portuguese--Catalan
###############################################################################

LANG1=por
LANG2=cat

VAR1=#leave empty
VAR2=_BR
VAR3=_PTpre1990

LANG1P=$(LANG1)$(VAR1)
LANG1B=$(LANG1)$(VAR2)
LANG1PP=$(LANG1)$(VAR3)

PREFIX1=$(LANG1)-$(LANG2)
PREFIX1P=$(LANG1P)-$(LANG2)
PREFIX1B=$(LANG1B)-$(LANG2)
PREFIX1PP=$(LANG1PP)-$(LANG2)

PREFIX2=$(LANG2)-$(LANG1)
PREFIX2P=$(LANG2)-$(LANG1P)
PREFIX2B=$(LANG2)-$(LANG1B)
PREFIX2PP=$(LANG2)-$(LANG1PP)

BASENAME=apertium-$(PREFIX1)

TARGETS_COMMON =		\
	$(PREFIX1).automorf.bin	\
	$(PREFIX1).prob		\
	$(PREFIX1).rlx.bin	\
	$(PREFIX1).autobil.bin	\
	$(PREFIX1).autolex.bin	\
	$(PREFIX1).autogen.bin	\
	$(PREFIX1).autopgen.bin	\
	$(PREFIX1).t1x.bin	\
	$(PREFIX2P).automorf.bin	\
	$(PREFIX2B).automorf.bin	\
	$(PREFIX2PP).automorf.bin	\
	$(PREFIX2).prob		\
	$(PREFIX2).rlx.bin	\
	$(PREFIX2P).autobil.bin	\
	$(PREFIX2B).autobil.bin	\
	$(PREFIX2PP).autobil.bin	\
	$(PREFIX2).autolex.bin	\
	$(PREFIX2P).autogen.bin	\
	$(PREFIX2B).autogen.bin	\
	$(PREFIX2PP).autogen.bin	\
	$(PREFIX2P).autopgen.bin	\
	$(PREFIX2B).autopgen.bin	\
	$(PREFIX2PP).autopgen.bin	\
	$(PREFIX2P).t1x	\
	$(PREFIX2P).t1x.bin	\
	$(PREFIX2B).t1x	\
	$(PREFIX2B).t1x.bin

# This include defines goals for install-modes, .deps/.d, autobil.prefixes and .mode files:

@ap_include@

###############################################################################
## Monolingual transducers
###############################################################################

# -------------------
# Portuguese analysis:
# -------------------

.deps/$(LANG1).automorf.bin: $(AP_SRC1)/$(LANG1).automorf.bin .deps/.d
	cp $< $@

$(PREFIX1).automorf.bin: .deps/$(LANG1).automorf.bin $(PREFIX1).autobil.bin
	lt-trim $^ $@

# -------------------
# Portuguese generation:
# ---------------------


$(PREFIX2P).autogen.bin: $(AP_SRC1)/$(LANG1P).autogen.bin
	cp $< $@

$(PREFIX2B).autogen.bin: $(AP_SRC1)/$(LANG1B).autogen.bin
	cp $< $@

$(PREFIX2PP).autogen.bin: $(AP_SRC1)/$(LANG1PP).autogen.bin
	cp $< $@

# -------------------
# Catalan analysis:
# -------------------

.deps/$(LANG2).automorf.bin: $(AP_SRC2)/$(LANG2).automorf.bin .deps/.d
	cp $< $@

$(PREFIX2P).automorf.bin: .deps/$(LANG2).automorf.bin $(PREFIX2P).autobil.bin
	lt-trim $^ $@

$(PREFIX2B).automorf.bin: .deps/$(LANG2).automorf.bin $(PREFIX2B).autobil.bin
	lt-trim $^ $@

$(PREFIX2PP).automorf.bin: .deps/$(LANG2).automorf.bin $(PREFIX2PP).autobil.bin
	lt-trim $^ $@

# -------------------
# Catalan generation:
# ---------------------


$(PREFIX1).autogen.bin: $(AP_SRC2)/$(LANG2).autogen.bin
	cp $< $@

###############################################################################
## Tagger models
###############################################################################

$(PREFIX1).prob: $(AP_SRC1)/$(LANG1).prob
	cp $< $@

$(PREFIX2).prob: $(AP_SRC2)/$(LANG2).prob
	cp $< $@

###############################################################################
## Disambiguation rules
###############################################################################

$(PREFIX1).rlx.bin: $(AP_SRC1)/apertium-$(LANG1).$(LANG1).rlx
	cg-comp $< $@

$(PREFIX2).rlx.bin: $(AP_SRC2)/apertium-$(LANG2).$(LANG2).rlx
	cg-comp $< $@

###############################################################################
## Bilingual dictionaries
###############################################################################

$(PREFIX1).autobil.bin: $(BASENAME).$(PREFIX1).dix
	apertium-validate-dictionary $<
	lt-comp lr $< $@

.deps/$(PREFIX1).dix: $(BASENAME).$(PREFIX1).dix .deps/.d
	apertium-translate-to-default-equivalent $< $@

.deps/$(PREFIX1).por.dix: .deps/$(PREFIX1).dix
	apertium-genvldix pt $< >$@

.deps/$(PREFIX1).por_BR.dix: .deps/$(PREFIX1).dix
	apertium-genvldix br $< >$@

.deps/$(PREFIX1).por_PTpre1990.dix: .deps/$(PREFIX1).dix
	apertium-genvldix pt_pre1990 $< >$@

$(PREFIX2P).autobil.bin: .deps/$(PREFIX1).por.dix
	apertium-validate-dictionary $<
	lt-comp --var-left=pt rl $< $@

$(PREFIX2B).autobil.bin: .deps/$(PREFIX1).por_BR.dix
	apertium-validate-dictionary $<
	lt-comp --var-left=br rl $< $@

$(PREFIX2PP).autobil.bin: .deps/$(PREFIX1).por_PTpre1990.dix
	apertium-validate-dictionary $<
	lt-comp --var-left=pt_pre1990 rl $< $@



###############################################################################
## Lexical selection rules
###############################################################################

.deps/%.metalrx: %.metalrx .deps/.d
	apertium-metalrx $< $@

.deps/%.lrx: .deps/%.metalrx
	apertium-metalrx-to-lrx $< $@

$(PREFIX1).autolex.bin: .deps/$(BASENAME).$(PREFIX1).lrx
	lrx-comp $< $@

$(PREFIX2).autolex.bin: .deps/$(BASENAME).$(PREFIX2).lrx
	lrx-comp $< $@

###############################################################################
## Structural transfer rules
###############################################################################

$(PREFIX1).t1x.bin: $(BASENAME).$(PREFIX1).t1x
	apertium-validate-transfer $<
	apertium-preprocess-transfer $< $@

#$(PREFIX1).t2x.bin: $(BASENAME).$(PREFIX1).t2x
#	apertium-validate-interchunk $<
#	apertium-preprocess-transfer $< $@

#$(PREFIX1).t3x.bin: $(BASENAME).$(PREFIX1).t3x
#	apertium-validate-postchunk $<
#	apertium-preprocess-transfer $< $@


#$(PREFIX2).t1x.bin: $(BASENAME).$(PREFIX2).t1x
#	apertium-validate-transfer $<
#	apertium-preprocess-transfer $< $@

$(PREFIX2P).t1x: $(BASENAME).$(PREFIX2).t1x
	apertium-filter-rules pt_PT $(BASENAME).$(PREFIX2).t1x >$@

$(PREFIX2B).t1x: $(BASENAME).$(PREFIX2).t1x
	apertium-filter-rules pt_BR $(BASENAME).$(PREFIX2).t1x >$@

$(PREFIX2P).t1x.bin: $(PREFIX2P).t1x
	apertium-validate-transfer $(PREFIX2P).t1x
	apertium-preprocess-transfer $(PREFIX2P).t1x $@

$(PREFIX2B).t1x.bin: $(PREFIX2B).t1x
	apertium-validate-transfer $(PREFIX2B).t1x
	apertium-preprocess-transfer $(PREFIX2B).t1x $@

#$(PREFIX2).t2x.bin: $(BASENAME).$(PREFIX2).t2x
#	apertium-validate-interchunk $<
#	apertium-preprocess-transfer $< $@

#$(PREFIX2).t3x.bin: $(BASENAME).$(PREFIX2).t3x
#	apertium-validate-postchunk $<
#	apertium-preprocess-transfer $< $@

###############################################################################
## Postgeneration dictionaries
###############################################################################

$(PREFIX1).autopgen.bin: $(AP_SRC2)/$(LANG2).autopgen.bin
	cp $< $@

$(PREFIX2P).autopgen.bin: $(AP_SRC1)/$(LANG1P).autopgen.bin
	cp $< $@

$(PREFIX2B).autopgen.bin: $(AP_SRC1)/$(LANG1B).autopgen.bin
	cp $< $@

$(PREFIX2PP).autopgen.bin: $(AP_SRC1)/$(LANG1PP).autopgen.bin
	cp $< $@

###############################################################################
## Distribution
###############################################################################

EXTRA_DIST=					\
		.deps/$(BASENAME).$(PREFIX1).lrx	\
		.deps/$(BASENAME).$(PREFIX2).lrx	\
		$(BASENAME).$(PREFIX1).dix	\
		$(BASENAME).$(PREFIX1).t1x	\
		$(BASENAME).$(PREFIX2).t1x	\
		modes.xml

###############################################################################
## Installation
###############################################################################

apertium_por_catdir=$(prefix)/share/apertium/$(BASENAME)/

# Files which should be installed to $(prefix)/share/apertium/$(BASENAME)/:
apertium_por_cat_DATA=	\
	$(TARGETS_COMMON)				\
	$(BASENAME).$(PREFIX1).t1x
# PREFIX2P/PREFIX2B.t1x are in TARGETS_COMMON

# All modes are created by the same goal, listing several will just give problems with make -j2
noinst_DATA=modes/$(PREFIX1).mode

install-data-local: install-modes

###############################################################################
## Cleanup
###############################################################################

CLEANFILES = $(TARGETS_COMMON)
clean-local:
	-rm -rf .deps modes

###############################################################################
## Test
###############################################################################

test: all
	echo "Apertium é uma plataforma de tradução automática livre e aberta" | apertium -d . por-cat | tee .test-mt
	@echo "Apertium és una plataforma de traducció automàtica lliure i oberta" > .test-ref
	@diff .test-ref .test-mt
	echo "Els tresors arquitectònics" | apertium -d . cat-por | tee .test-mt
	@echo "Os tesouros arquitetónicos" > .test-ref
	@diff .test-ref .test-mt
	echo "Els tresors arquitectònics" | apertium -d . cat-por_PTpre1990 | tee .test-mt
	@echo "Os tesouros arquitectónicos" > .test-ref
	@diff .test-ref .test-mt
	echo "Els tresors arquitectònics" | apertium -d . cat-por_BR | tee .test-mt
	@echo "Os tesouros arquitetônicos" > .test-ref
	@diff .test-ref .test-mt
	@rm .test-ref .test-mt
