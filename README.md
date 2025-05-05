# ESC180-Semantic-Similarity-assignment-3
To build an intelligent system that can approximate the semantic similarity of any pair of words. Credits go to ESC180, instructor Michael Guerzhoy, and functions not described here are pre-written by Michael Guerzhoy

The final file is the synonyms.py file, the txt files are the ones used for training.

# Assignment description
One type of question encountered in the Test of English as a Foreign Language (TOEFL) is the “Synonym Question”, where students are asked to pick a synonym of a word out of a list of alternatives. The semantic similarity between two words is the measure of the closeness of their meanings. For example, the semantic similarity between “car” and “vehicle” is high, while that between “car” and “flower” is low. 

In order to answer the TOEFL question, you will compute the semantic similarity between the word you are given and all the possible answers, and pick the answer with the highest semantic similarity to the given word. More precisely, given a word w and a list of potential synonyms s1, s2, s3, s4, we compute the similarities of (w, s1), (w, s2), (w, s3), (w, s4) and choose the word whose similarity to w is the highest.

We will measure the semantic similarity of pairs of words by first computing a semantic descriptor vector of each of the words, and then taking the similarity measure to be the cosine similarity between the two vectors. Given a text with n words denoted by (w1, w2, ..., wn) and a word w, let descw be the semantic descriptor vector of w computed using the text. descw is an n-dimensional vector. The i-th coordinate of descw is the number of sentences in which both w and wi occur. For efficiency’s sake, we will store the semantic descriptor vector as a dictionary, not storing the zeros that correspond to words which don’t co-occur with w. 
For example, suppose we are given the following text (the opening of Notes from the Underground by Fyodor Dostoyevsky, (translated by Constance Garnett):
" I am a sick man. I am a spiteful man. I am an unattractive man. I believe my liver is diseased. However, I know nothing at all about my disease, and do not know for certain what ails me. "

The word “man” only appears in the first three sentences. Its semantic descriptor vector would be: {"i": 3, "am": 3, "a": 2, "sick": 1, "spiteful": 1, "an": 1, "unattractive": 1}. The word “liver” only occurs in the second sentence, so its semantic descriptor vector is: {"i": 1, "believe": 1, "my": 1, "is": 1, "diseased": 1}. We store all words in all-lowercase, since we don’t consider, for example, “Man” and “man” to be different words. We do, however, consider, e.g., “believe” and “believes”, or “am” and “is” to be different words. We discard all punctuation.

Implement the following functions in synonyms.py:
- cosine_similarity(vec1, vec2): This function returns the cosine similarity between the sparse vectors vec1 and vec2, stored as dictionaries. For example, cosine_similarity({"a": 1, "b": 2, "c": 3}, {"b": 4, "c": 5, "d": 6}) should return approximately 0.70 (as a float).
- build_semantic_descriptors(sentences): This function takes in a list sentences which contains lists of strings (words) representing sentences, and returns a dictionary d such that for every word w that appears in at least one of the sentences, d[w] is itself a dictionary which represents the semantic descriptor of w (note: the variable names here are arbitrary).
For example, if sentences represents the opening of Notes from the Underground above:
[["i", "am", "a", "sick", "man"],
["i", "am", "a", "spiteful", "man"],
["i", "am", "an", "unattractive", "man"],
["i", "believe", "my", "liver", "is", "diseased"],
["however", "i", "know", "nothing", "at", "all", "about", "my",
"disease", "and", "do", "not", "know", "for", "certain", "what", "ails", "me"]],
part of the dictionary returned would be:
{ "man": {"i": 3, "am": 3, "a": 2, "sick": 1, "spiteful": 1, "an": 1,
"unattractive": 1},
"liver": {"i": 1, "believe": 1, "my": 1, "is": 1, "diseased": 1},
... }
with as many keys as there are distinct words in the passage.

- build_semantic_descriptors_from_files(filenames): This function takes a list of filenames of strings, which contains the names of files (the first one can
be opened using open(filenames[0], "r", encoding="latin1")), and returns the a dictionary of the semantic descriptors of all the words in the files filenames, with the files treated as a single text. You should assume that the following punctuation always separates sentences: ".", "!", "?", and that is the only punctuation that separates sentences. You should also assume that that is the only punctuation that separates sentences. Assume that only the following punctuation is present in the texts: [",", "-", "--", ":", ";"]

- most_similar_word(word, choices, semantic_descriptors, similarity_fn): This function takes in a string word, a list of strings choices, and a dictionary semantic_descriptors which is built according to the requirements for build_semantic_descriptors, and returns the element of choices which has the largest semantic similarity to word, with the semantic similarity computed using the data in semantic_descriptors and the similarity function similarity_fn. The similarity function is a function which takes in two sparse vectors stored as dictionaries and returns a float. An example of such a function is cosine_similarity. If the semantic similarity between two words cannot be computed, it is considered to be −1. In case of a tie between several elements in choices, the one with the smallest index in choices should be returned (e.g., if there is a tie between choices[5] and choices[7], choices[5] is returned).

- run_similarity_test(filename, semantic_descriptors, similarity_fn): This function takes in a string filename which is the name of a file in the same format as test.txt, and returns the percentage (i.e., float between 0.0 and 100.0) of questions on which most_similar_word() guesses the answer correctly using the semantic descriptors stored in semantic_descriptors, using the similarity function similariy_fn. The format of test.txt is as follows. On each line, we are given a word (all-lowercase), the correct answer, and the choices. For example, the line: feline cat dog cat horse
 represents the question:
"feline:
(a) dog
(b) cat
(c) horse"
and indicates that the correct answer is “cat”.
