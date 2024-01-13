OTT_OPTS  = -tex_show_meta false
OTT_FILES  = ../spec/source.ott

MAIN = thesis

RULESFILE = ott-rules.tex

AUTOGEN = chapters/acknowledgment.mng chapters/abstract.mng chapters/introduction.mng \
		chapters/background.mng chapters/switch.mng chapters/poly.mng chapters/merge.mng \
		chapters/related.mng chapters/future.mng chapters/conclusion.mng \
		chapters/duotyping.mng chapters/appendix.mng

all: $(MAIN).pdf

pure: $(MAIN).tex $(wildcard chapters/*.tex) thesis.bib $(AUTOGEN)
	@latexmk -pdf thesis.tex -pdflatex='pdflatex "\def\ispure{1}\input" %S' \

$(MAIN).pdf: $(MAIN).tex $(wildcard chapters/*.tex) thesis.bib $(AUTOGEN)
	@latexmk -pdf $(@:.pdf=.tex)

$(RULESFILE): $(OTT_FILES)
	ott -tex_wrap false $(OTT_OPTS) -o $@ $^
	@if grep '<<no parses (' $@ >/dev/null 2>&1 && \
		[ -z "$(DONTSTOP)" ]; then \
			echo; \
			echo "***** OTT PARSE ERROR(S) *****"; \
			grep -n '<<no parses (' $@; \
			$(RM) $@; \
			exit 1; \
	fi >&2

%.mng: %.tex $(RULESFILE)
	ott -tex_wrap false $(OTT_OPTS) -tex_filter $*.tex $*.mng $(OTT_FILES)

clean:
	@latexmk -c
	@rm -f $(MAIN).pdf $(RULESFILE)  *.aux *.fls *.out *.synctex.gz *.log *.fdb_latexmk \
	*.blg bundle.tar.gz chapters/*.mng chapters/*.bak *.bbl $(MAIN).brf *.lot *.lof *.brf \
	*.toc

.PHONY: all clean
