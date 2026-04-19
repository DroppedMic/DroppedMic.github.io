# Build pipeline for Coaching the Lead Learner
# Requires: pandoc, xelatex (MacTeX/TeXLive), and the fonts listed in
# metadata.yaml installed on the system.

SRC := $(sort $(wildcard manuscript/*.md))
META := metadata.yaml
BIB := references.bib
CSL := csl/apa.csl

BUILD := build
PDF := $(BUILD)/paperback.pdf
EPUB := $(BUILD)/kindle.epub

PANDOC_COMMON := \
	--from=markdown+smart+yaml_metadata_block+citations \
	--citeproc \
	--bibliography=$(BIB) \
	--csl=$(CSL) \
	--metadata-file=$(META) \
	--top-level-division=chapter

.PHONY: all paperback kindle clean

all: paperback kindle

paperback: $(PDF)

kindle: $(EPUB)

$(PDF): $(SRC) $(META) $(BIB) | $(BUILD)
	pandoc $(PANDOC_COMMON) \
		--pdf-engine=xelatex \
		--output=$@ \
		$(SRC)

$(EPUB): $(SRC) $(META) $(BIB) | $(BUILD)
	pandoc $(PANDOC_COMMON) \
		--to=epub3 \
		--epub-chapter-level=1 \
		--output=$@ \
		$(SRC)

$(BUILD):
	mkdir -p $(BUILD)

clean:
	rm -rf $(BUILD)
