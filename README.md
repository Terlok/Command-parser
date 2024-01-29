# Command-parser
A collection of text documents with the possibility of full-text search. \
Inverted indexes are used to speed up the execution of search commands. To build an index, the program must analyze the documents that are added to the collections, select individual words from them and store a list of positions where the given word occurs within the given document. For simplicity, within the scope of this workshop, we consider a "word" in a document to be an arbitrary sequence consisting of alphanumeric characters and the _ symbol (in terms of regular expressions - [a-zA-Z0-9_]+). The rest of the characters found in documents define word boundaries and can be ignored. 

The list of commands that must be implemented:
1. __CREATE collection_name;__ - creation of a new collection named collection_name. \
As a result, the command should return a certain message to the user, for example “Collection collection_name has been created”.

2. __INSERT collection_name “value”;__ - adding a new document to the collection_name collection. \
As a result, the command returns a certain message to the user, for example “Document has been added to collection_name”. 

Example: \
INSERT wiki_articles “The word 'algorithm' has its roots in Latinizing the ... ”;

3. __PRINT_INDEX collection_name;__ - output to the screen of the internal structure of the inverted index built for the collection collection_name. \
The output should include the highlighted keywords, the documents to which they belong, and the positions in which they occur in the given documents. 

Example: \
For an index containing documents “to be or not to be” and “to go or not to go” that were assigned identifiers d1 and d2 respectively, the output should look like the following: \
"be": \
   d1 -> [2, 6] \
"go": \
   d2 -> [2, 6] \
"not": \
   d1 -> [4] \
   d2 -> [4] \
"or": \
   d1 -> [3] \
   d2 -> [3] \
"to": \
   d1 -> [1, 5] \
   d2 -> [1, 5] 

4. __SEARCH collection_name [WHERE query];__ - search for documents by collection_name. \
If the WHERE keyword is specified and a search query follows it, then the command should include in the sample only those documents that satisfy this query. Otherwise, all documents from the specified collection must be returned. The query definition has the following form: \
query := “keyword” \
    | "prefix"* \
    | “keyword_1” <N> “keyword_2” 

Explanation:
- "keyword" - documents containing the word "keyword" must be included in the sample.
- "prefix"* - prefix search, i.e. documents that contain a word beginning with "prefix" should be included in the sample. For example, if query = "univer"*, then all documents containing the words "university", "universally", etc. should be included in the selection.
- “keyword_1” <N> “keyword_2” - the sample should include documents that contain the words “keyword_1” and “keyword_1” at a distance of N words from each other. The position in the document and the relative order of the words are not important. For example, the document “The word 'algorithm' has its roots in Latinizing the nisba ...” satisfies the following queries: “word” <1> “algorithm”, “algorithm” <1> “word”, “word” <4> “roots”, “roots” <4> “word”, etc. \
It can be assumed that “keyword”, “prefix”, “keyword_1” and “keyword_2” above are correct words, according to the definition given at the beginning of this variant, that is, they satisfy the regular expression [a-zA-Z0-9_]+. \
The search for documents satisfying a certain request should be carried out without taking into account the register of documents and keywords in these requests. That is, for example, the document “The word 'algorithm' has its roots in Latinizing the nisba ...” satisfies all the following search queries: “algorithm”, “ALGORithm”, “ALGo”* , “WORD” <2> “has” etc. \
As a result, the command should return a list of found documents. The order of the lines can be arbitrary.