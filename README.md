# termprofiling

Implementation of the algorithm in:
> Tomokiyo, T., & Hurst, M. (2003, July). A language model approach to keyphrase extraction. In Proceedings of the ACL 2003 workshop on Multiword expressions: analysis, acquisition and treatment-Volume 18 (pp. 33-40). Association for Computational Linguistics.

## Usage

### Stand-alone

```
python kldiv.py foreground background.txt termcloud.html
```
```
python kldiv.py foreground wiki_freqlist.txt.gz termcloud.html
```
```
python kldiv.py foreground termcloud.html
```
```
python kldiv.py foreground wiki_freqlist.txt.gz
```

`foreground` can either be a text file or a directory.

### As a package from an external script

```
import termprofiling.kldiv as kldiv
kldiv.process_corpora_and_print_terms(foreground_file,background_file,htmlpath,gamma,maxn,number_of_terms)
```

* The first argument is the foreground corpus in plain text or a directory of texts. In the case of pdf input, use https://github.com/euske/pdfminer for the conversion. Adapt if needed for json, xml, csv or any other formats, and multi-file instead of single-file. 
* The second argument (optional) is the background corpus in plain text, or the file `wiki_freqlist.txt.gz` (default), an n-gram frequency list extracted from a small portion of the English Wikipedia (3.5 M words). 
* The third argument (optional) is an HTML output file with the termcloud. In standalone use, the default value is `wordcloud.html`
* The optional fourth argument is the value of gamma (between 0.0 and 1.0; default 0.5. For English texts, we suggest to use gamma=0.8. 1.0 means the use of the phraseness component alone)
* The optional fifth argument is the maximum ngram length (number of words). The default is 3, returning, unigrams, bigrams, and trigrams.
* The optional sixth argument is the number of terms returned (default 15)

```
import termprofiling.kldiv as kldiv
kldiv.process_corpora_and_print_terms(my_corpus_directory,"wiki_freqlist.txt.gz","my_termcloud.html",0.8,4,15)
```

or, using all defaults:

```
import termprofiling.kldiv as kldiv
kldiv.process_corpora_and_print_terms(my_corpus_directory)
```

If you only get single words, and you would like to see multi-word terms, try increasing gamma:

```
import termprofiling.kldiv as kldiv
kldiv.process_corpora_and_print_terms(my_corpus_file,gamma=0.8)
```

## Description of functionality

Scores all unigrams, bigrams and trigrams in the foreground text for (a) their informativeness relative to the background corpus and (b) their 'phraseness': the frequency of the n-gram compared to the frequencies of the separate unigrams. Ngrams that start or end with a stopword are not included.

The script uses an external stopword list. The provided stoplist.txt is my own English stoplist.

If there is no background corpus provided, only the phraseness component is computed.

If you use this code, please refer to this paper:
> Suzan Verberne, Maya Sappelli, Djoerd Hiemstra, Wessel Kraaij (2016). Evaluation and analysis of term scoring methods for term extraction. Information Retrieval, Springer. doi:10.1007/s10791-016-9286-2

Source of the wordcloud css: http://onwebdev.blogspot.com/2011/05/css-word-cloud.html

### License

See the [LICENSE](LICENSE.md) file for license rights and limitations (GNU-GPL v3.0).
