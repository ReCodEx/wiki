NAME = user_doc
CHAPTERS_DIR = ../../GlobalWiki.wiki
IMAGES_DIR = ../images
CHAPTER_FILES = $(wildcard $(CHAPTERS_DIR)/*.tex $(CHAPTERS_DIR)/*.md)
CHAPTERS = $(sort $(notdir $(basename $(CHAPTER_FILES))))

BUILD_DIR = build

CHAPTERS_TEX = $(CHAPTERS:%=$(BUILD_DIR)/%.tex)
COMBINED_TEX = $(BUILD_DIR)/$(NAME).combined.tex
PDFL_FLAGS = -jobname $(NAME) -file-line-error -output-directory $(BUILD_DIR)

all: $(NAME).pdf

# LaTeX must be run twice to get references right
$(NAME).pdf: $(COMBINED_TEX)
	pdflatex $(PDFL_FLAGS) $<
	pdflatex $(PDFL_FLAGS) $<

$(COMBINED_TEX): $(BUILD_DIR) $(CHAPTERS_TEX) $(wildcard *.tex)
	cat start.tex > $@
	sed -f rewrite_filter.sed $(CHAPTERS_TEX) >> $@
	cat end.tex >> $@

$(BUILD_DIR):
	mkdir -p "$@"

$(BUILD_DIR)/%.tex: $(CHAPTERS_DIR)/%.tex
	cp $< $@

$(BUILD_DIR)/%.tex: $(CHAPTERS_DIR)/%.md
	pandoc --chapters --template=default.latex -o $@ $<

clean:
	- rm -rf $(BUILD_DIR)
