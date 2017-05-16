# Enabling Synonyms, Stemming and Multi word Search on AEM

This is a sample package that can be uploaded to AEM to enable Synonyms, Stemming and Multi word search on Lucene.

## Introduction
AEM uses lucene as the default search and by default searching on synonyms, stemming, multiple words is not configured. 
1. Synonyms (e.g: shut, close) - a word or phrase that means exactly or nearly the same as another word or phrase in the same language.
2. Stemming (e.g: teach, teaches, teaching, teached) - is the process of reducing inflected (or sometimes derived) words to their word stem, base or root form. 
3. Multi words (e.g: tree house, covered bridge) - enabling to search on both words, or part of the words.

## Step-by-step guide
1. Upload package
   Use the attached package to AEM using Package Manager. This will add nodes to "/oak:index/mybrand"
2. Check mybrand
   Open http://localhost:4502/crx/de/index.jsp#/oak%3Aindex and check "mybrand" is added to crx
   This package contains the following:
   1. Required for **Synonyms** (e.g: peanuts, treenuts): syn.txt [ /oak:index/mybrand/analyzers/default/filters/Synonym/syn.txt ]
   2. Required for **Stemming** (e.g: if "teach" is keyword, search should also find "teaching, teaches, teached, etc")
      * **PorterStem** [ /oak:index/mybrand/analyzers/default/filters/PorterStem ]
   3. Required for searching parts of multiple words (e.g: tree house): 
      * **WhitespaceTokenizerFactory** [ /oak:index/mybrand/analyzers/default/tokenizer/WhitespaceTokenizerFactory ]
      * **WordDelimiter** [ /oak:index/mybrand/analyzers/default/filters/WordDelimiter ]
3. Disable **damAssetLucene**
   Open "damAssetLucene" under "/oak:index" http://localhost:4502/crx/de/index.jsp#/oak%3Aindex/damAssetLucene
   Choose "type" from properties and change the value from "lucene" to "disabled" (as listed below)
4. Save all

## Test
1. Open AEM > Assets > Pick any asset (say asset1.jpg) and add each of the below to "cq:Tags" as separate tag
   ```
   teach
   tree house
   peanuts
   ```
   The reason we gave the above is becaue these are listed in the sample "syn.txt". The content of "syn.txt" is:
   ```
   tree house,beach house,
   teach
   covered bridge
   peanuts,treenuts
   fasc,cargo authorized,authorized ship center,authorized shipping center,authorized ship center, hello
   fcis,customer information services,customer services
   ```
2. Choose AEM > Assets > Select search bar. Enter the below:
   Search term: **peanuts** - Should see asset1.jpg
   Search term: **treenuts** - Should see asset1.jpg
   Search term: **teach** - Should see asset1.jpg
   Search term: **teaches** - Should see asset1.jpg
   Search term: **teaching** - Should see asset1.jpg
   Search term: **tree** - Should see asset1.jpg
   Search term: **house** - Should see asset1.jpg
   Search term: **tree house** - Should see asset1.jpg
   Search term: **tree houses** - Should see asset1.jpg
   Search term (with double-quotes): **"tree house"** - Should see asset1.jpg
   Search term (with double-quotes): **"tree houses"** - Should see asset1.jpg
