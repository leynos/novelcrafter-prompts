%.b64: %.json
	jq -Mc . <$< | gzip | base64 > $@

prompt_only.txt: prompt_inline_deps.json
	jq -r .messages[].text <$< >$@

README.md: prompt_inline_deps.json
	echo -n "# " >$@
	jq -r .name <$< >>$@
	jq -r .description <$< | sed 's/<p>\|<\/p>/\n/g' | sed 's/<strong>\|<\/strong>/**/g' | awk 'BEGIN {inli=0} /<ul>/ {print ""} /<li>/ {inli=1; next} /<\/ul>/ {inli=0; next} inli==1 {print "- " $$0; next} 1' >>$@

%.json: %.b64
	base64 -d <$< | gunzip | jq . > $@

publish: $(patsubst %.json,%.b64,$(wildcard *.json)) prompt_only.txt README.md

extract: $(patsubst %.b64,%.json,$(wildcard *.b64)) prompt_only.txt README.md

.PHONY: publish extract
