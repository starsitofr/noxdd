all:
.PHONY: update-po update-js update-pot
.PHONY: FORCE

LINGUAS := cs de el es fr it ja ko nl pl pt_BR ru sv tr zh_CN zh_TW

VERSION := $(shell grep '"version"' ../package.json | cut -d '"' -f 4)

POFILES := $(addsuffix .po,$(LINGUAS))
JSONFILES := $(addprefix ../app/locale/,$(addsuffix .json,$(LINGUAS)))

update-po: $(POFILES)
update-js: $(JSONFILES)

%.po: FORCE
	msgmerge --update --lang=$* $@ noxdd.pot
../app/locale/%.json: FORCE
	./po2js $*.po $@

update-pot:
	xgettext --output=noxdd.js.pot \
		--copyright-holder="The noxdd Authors" \
		--package-name="noxdd" \
		--package-version="$(VERSION)" \
		--msgid-bugs-address="noxdd@googlegroups.com" \
		--add-comments=TRANSLATORS: \
		--from-code=UTF-8 \
		--sort-by-file \
		../app/*.js \
		../core/*.js \
		../core/input/*.js
	./xgettext-html --output=noxdd.html.pot \
		../xdd.html
	msgcat --output-file=noxdd.pot \
		--sort-by-file noxdd.js.pot noxdd.html.pot
	rm -f noxdd.js.pot noxdd.html.pot
