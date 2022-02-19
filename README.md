# Nextword-data

## **🎉 NEW PREDICTION ENGINE [MOCWORD](https://github.com/high-moctane/mocword) IS AVAILABLE 🎉**

[Mocword](https://github.com/high-moctane/mocword) is more advanced engine than Nextword.

- Less data file size
  - 1.63GB (Nextword) -> 655MB (Mocword)
- Using latest Google Ngram dataset
  - 2012 data (Nextword) -> 2020 data (Mocword)
- More appropriate prediction
- Less noisy vocabularies

---

A dataset for nextword.

## Install

0. (Recommended) Star this repository (｀･ω･´)★

1. Visit [releases](https://github.com/high-moctane/nextword-data/releases) page.

2. Download `zip` or `tar.gz`.

   You can choose larger or smaller one.

   |       | Zip size | Total size |
   | ----- | -------: | ---------: |
   | Small | 152.2 MB |   493.1 MB |
   | Large | 483.3 MB |    1.63 GB |

3. Decompress downloaded data.

4. Set `$NEXTWORD_DATA_PATH` environment variable.

   Example:

   ```bash
   export NEXTWORD_DATA_PATH=/path/to/nextword-data
   ```

## Uninstall

1. Remove `$NEXTWORD_DATA_PATH` environment variable.

2. Remove nextword-data directory.

## Format

```
(n-1)gram tab candidates newline
```

Candidates are sorted by appearance order.

### Example

You can find the line

```
empty milk	bottles carton bottle cartons cans
```

at line 59349 in file `3gram-e.txt`.

This line describes the word "bottles" is the most likely word after "empty milk"
and "carton" is the next.

## Recipe

1. Fetch data.

   ```
   $ mkdir fetch
   $ nwgen-fetch fetch
   ```

2. Run xonsh script.

   ```xonsh
   dstdir = "dstdir"
   mkdir -p @(dstdir)/format
   mkdir -p @(dstdir)/concat

   ls fetch | grep 1gram | xargs -P 8 -I fname nwgen-format -max-candidates 10000 -min-count 10000 @(dstdir)/format/fname fetch/fname

   ls fetch | grep 2gram | xargs -P 8 -I fname nwgen-format -max-candidates 10000 -min-count 2000 @(dstdir)/format/fname fetch/fname

   ls fetch | grep 3gram | xargs -P 8 -I fname nwgen-format -max-candidates 10000 -min-count 400 @(dstdir)/format/fname fetch/fname

   ls fetch | grep 4gram | xargs -P 8 -I fname nwgen-format -max-candidates 10000 -min-count 300 @(dstdir)/format/fname fetch/fname

   ls fetch | grep 5gram | xargs -P 8 -I fname nwgen-format -max-candidates 10000 -min-count 200 @(dstdir)/format/fname fetch/fname

   nwgen-concat @(dstdir)/concat/1gram.txt.gz @(dstdir)/format/1gram*

   for n in [2,3,4,5]:
       for c in [chr(i) for i in range(97, 97+26)]:
           nwgen-concat @(dstdir)/concat/@(n)gram-@(c).txt.gz @(dstdir)/format/@(n)gram-@(c)*

   cp -R @(dstdir)/concat @(dstdir)/data

   gunzip @(dstdir)/data/*
   ```

## Notice

Nextword-data is based on
[Google Books Ngram Viewer English Version 20120701](http://storage.googleapis.com/books/ngrams/books/datasetsv2.html)
which is distributed under a [Creative Commons Attribution 3.0 Unported](http://creativecommons.org/licenses/by/3.0/).
See [NOTICE.txt](NOTICE.txt).

## License

Nextword-data is distributed under a [Creative Commons Attribution 4.0 International](https://creativecommons.org/licenses/by/4.0/).
See [LICENSE.txt](LICENSE.txt).
