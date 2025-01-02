%.b64: %.json
	jq -Mc . <$< | gzip | base64 > $@

prompt_only.txt: prompt_inline_deps.json
	jq -r .messages[].text <$< >$@

README.md: prompt_inline_deps.json
	jq -r .description <$< | sed 's/<p>\|<\/p>/\n/g' >$@

%.json: %.b64
	base64 -d <$< | gunzip | jq . > $@


publish: $(patsubst %.json,%.b64,$(wildcard *.json)) prompt_only.txt README.md

extract: $(patsubst %.b64,%.json,$(wildcard *.b64)) prompt_only.txt README.md

.PHONY: publish extract
