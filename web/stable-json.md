Stable JSON
===========

The order of properties of JSON object is not guaranteed. But if the JSON data file is managed by a version control system. Guaranteed property order will be much better. This format also called "Stable JSON." I took some time survey how to produce stable JSON. And found [jq][] already supports output object with sorted properties. And combine with the util [sponge][] (part of moreutils). It's very easy to parse an existing JSON file.

```sh
cat ../zipdata/$$file | jq -S -c -M -j . | sponge ../zipdata/$$file
```

[jq]:https://stedolan.github.io/jq/
[sponge]:https://linux.die.net/man/1/sponge