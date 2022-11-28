dist: dist/haskell-cli-template.tar.gz

ARCHIVE_TARGETS := \
  dist/haskell-cli-template/haskell-cli-template \
  dist/haskell-cli-template/completion/bash \
  dist/haskell-cli-template/completion/fish \
  dist/haskell-cli-template/completion/zsh \
  dist/haskell-cli-template/doc/haskell-cli-template.1 \
  dist/haskell-cli-template/Makefile

dist/haskell-cli-template.tar.gz: $(ARCHIVE_TARGETS)
	tar -C ./dist -czvf $@ ./haskell-cli-template

SRCS := $(shell \
  find ./src ./app -name '*.hs'; \
  echo stack.yaml; \
  echo haskell-cli-template.cabal \
)

dist/haskell-cli-template/haskell-cli-template: $(SRCS)
	mkdir -p ./dist/haskell-cli-template
	stack build --pedantic --test --copy-bins --local-bin-path dist/haskell-cli-template

dist/haskell-cli-template/completion/%: dist/haskell-cli-template/haskell-cli-template
	mkdir -p ./dist/haskell-cli-template/completion
	./$< --$(@F)-completion-script haskell-cli-template > dist/haskell-cli-template/completion/$(@F)

PANDOC ?= stack exec pandoc --

dist/haskell-cli-template/doc/%: doc/%.md
	mkdir -p ./dist/haskell-cli-template/doc
	$(PANDOC) --standalone $< --to man >$@

dist/haskell-cli-template/Makefile: Makefile
	mkdir -p dist/haskell-cli-template
	cp $< $@

.PHONY: clean
clean:
	$(RM) -r ./dist
	stack clean --full

DESTDIR ?=
PREFIX ?= /usr/local
MANPREFIX ?= $(PREFIX)/share/man

INSTALL ?= $(shell command -v ginstall 2>/dev/null || echo install)

.PHONY: install
install:
	$(INSTALL) -Dm755 haskell-cli-template $(DESTDIR)$(PREFIX)/bin/haskell-cli-template
	$(INSTALL) -Dm644 completion/bash $(DESTDIR)$(PREFIX)/share/bash-completion/completions/haskell-cli-template
	$(INSTALL) -Dm644 completion/fish $(DESTDIR)$(PREFIX)/share/fish/vendor_completions.d/haskell-cli-template.fish
	$(INSTALL) -Dm644 completion/zsh $(DESTDIR)$(PREFIX)/share/zsh/site-functions/_haskell-cli-template
	$(INSTALL) -Dm644 doc/haskell-cli-template.1 $(DESTDIR)$(MANPREFIX)/man1/haskell-cli-template.1

.PHONY: uninstall
uninstall:
	$(RM) $(DESTDIR)$(PREFIX)/bin/haskell-cli-template
	$(RM) $(DESTDIR)$(PREFIX)/share/bash-completion/completions/haskell-cli-template
	$(RM) $(DESTDIR)$(PREFIX)/share/fish/vendor_completions.d/haskell-cli-template.fish
	$(RM) $(DESTDIR)$(PREFIX)/share/zsh/site-functions/_haskell-cli-template
	$(RM) $(DESTDIR)$(MANPREFIX)/man1/haskell-cli-template.1

.PHONY: install.check
install.check: dist/haskell-cli-template.tar.gz
	cp dist/haskell-cli-template.tar.gz /tmp && \
	  cd /tmp && \
	  tar xvf haskell-cli-template.tar.gz && \
	  cd haskell-cli-template && \
	  make install PREFIX=$$HOME/.local
	PATH=$$HOME/.local/bin:$$PATH haskell-cli-template
