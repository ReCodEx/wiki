NAME = recodex_doc
CHAPTERS_DIR ?= ../../wiki.wiki
IMAGES_DIR ?= ../images
BUILD_DIR = build

#CHAPTERS = Introduction User-documentation Overall-architecture
#CHAPTERS += Assignments Submission-flow Installation
#CHAPTERS += Worker Broker Monitor Fileserver Web-API
#CHAPTERS += Web-application Database Conclusion

# Set CHAPTERS variable to new (rewritten) file
CHAPTERS = Rewritten-docs

# Doc appendices
APPENDICES = Installation System-configuration Job-configuration

CHAPTERS_TEX = $(CHAPTERS:%=$(BUILD_DIR)/%.tex)
CHAPTERS_MD = $(CHAPTERS:%=$(CHAPTERS_DIR)/%.md)
APPENDICES_TEX = $(APPENDICES:%=$(BUILD_DIR)/%.tex)
APPENDICES_MD = $(APPENDICES:%=$(BUILD_DIR)/%.md)

COMBINED_TEX = $(BUILD_DIR)/$(NAME).combined.tex
PDFL_FLAGS = -jobname $(NAME) -file-line-error -output-directory $(BUILD_DIR) --interaction=batchmode

all: $(NAME).pdf  # $(NAME).html

# LaTeX must be run twice to get references right
$(NAME).pdf: $(COMBINED_TEX)
	pdflatex $(PDFL_FLAGS) $<
	pdflatex $(PDFL_FLAGS) $<

$(NAME).html: $(CHAPTERS_MD) $(APPENDICES_MD)
	pandoc -o $@ --toc $(CHAPTERS_MD) $(APPENDICES_MD)

$(COMBINED_TEX): $(BUILD_DIR) $(CHAPTERS_TEX) $(APPENDICES_TEX) start.tex appendices.tex end.tex
	cat start.tex > $@
	sed -f rewrite_filter.sed $(CHAPTERS_TEX) >> $@
	cat appendices.tex >> $@
	sed -f rewrite_filter.sed $(APPENDICES_TEX) >> $@
	cat end.tex >> $@

$(BUILD_DIR):
	mkdir -p "$@"

$(BUILD_DIR)/%.tex: $(CHAPTERS_DIR)/%.md
	pandoc --chapters --smart --normalize -o $@ $<

clean:
	- rm -rf $(BUILD_DIR)
