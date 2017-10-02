SOURCES=$(wildcard *.Rmd)
DIR=/usr/local
PANDOC_OPTIONS= --top-level-division=chapter \
	--table-of-contents \
	--listings \
	--highlight-style pygments \
	-M fignos-plus-name=Figure \
	-M fignos-cleveref=On \
	-M link-citations=true

PANDOC_FILTERS=-F pandoc-fignos -F pandoc-citeproc
#--number-sections
PANDOC_REFERENCES=--csl=$(DIR)/styles/ieee-with-url.csl --bibliography=bibtex/example.bib

RM = /bin/rm -f
PANDOC = pandoc
RSCRIPT=Rscript
LATEX=xelatex

PDF_OBJECTS=$(SOURCES:.Rmd=.pdf)
MD_OBJECTS=$(SOURCES:.Rmd=.md)

all: pdf
pdf: $(MD_OBJECTS) $(PDF_OBJECTS)

%.md: %.Rmd
	$(RSCRIPT) -e "library(knitr); knit('$<')"

%.pdf: %.md
	echo $<
	$(PANDOC) -f markdown  $(PANDOC_FILTERS) --latex-engine=xelatex --template=$(DIR)/article/article.template $(PANDOC_OPTIONS) $(PANDOC_REFERENCES)  -o $@  $<

.intermediate: $(MD_OBJECTS)

clean:
	-rm $(PDF_OBJECTS) $(MD_OBJECTS)

run:
	docker run --rm --interactive --tty --publish 8088:8088 \
		--publish 50070:50070 \
		--volume "$(shell pwd)/mywork/":/usr/local/hadoop/mywork \
		sequenceiq/hadoop-docker:2.7.1 /etc/bootstrap.sh -bash