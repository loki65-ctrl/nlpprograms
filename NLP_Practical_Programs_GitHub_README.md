# NLP Practical Programs (Python)

This README contains the 25 NLP practical programs in separate Python
code blocks so they can be copied directly from GitHub.

## Requirements

Some programs use NLTK, librosa, or scikit-learn. Install the libraries
with:

``` bash
pip install nltk librosa scikit-learn
```

For Program 6, place a file named `sample.wav` in the same folder as the
Python script.

------------------------------------------------------------------------

## Program 1: Tokenize a paragraph into sentences and words

``` python
import nltk
nltk.download("punkt_tab")

from nltk.tokenize import sent_tokenize, word_tokenize

text = input("Enter the paragraph: ")

print("Sentence tokenization:")
print(sent_tokenize(text))

print("\nWord tokenization:")
print(word_tokenize(text))
```

## Program 2: Build a unigram language model

``` python
from collections import Counter

corpus = input("Enter the text from corpus: ")
words = corpus.lower().split()
word_count = Counter(words)
total_words = len(words)

print("Unigrams:")
for word in word_count:
    print(word, "->", word_count[word])

print("Total words:", total_words)
```

## Program 3: Rule-based identification of parts of a sentence

``` python
sentence = input("Enter a sentence: ")
words = sentence.lower().split()

print("\nParts of sentence:")
for word in words:
    if word in ["a", "an", "the"]:
        print(word, "-> Article")
    elif word.endswith("ing"):
        print(word, "-> Verb")
    elif word.endswith("ly"):
        print(word, "-> Adverb")
    elif word.endswith(("ous", "ful", "ive")):
        print(word, "-> Adjective")
    else:
        print(word, "-> Noun")
```

## Program 4: Basic semantic analysis using word similarity

``` python
import nltk
from nltk.corpus import wordnet

nltk.download("wordnet")

word1 = input("Enter first word: ").lower()
word2 = input("Enter second word: ").lower()

synsets1 = wordnet.synsets(word1)
synsets2 = wordnet.synsets(word2)

if not synsets1 or not synsets2:
    print("One or both words were not found in WordNet.")
else:
    similarity = synsets1[0].wup_similarity(synsets2[0])

    if similarity is None:
        level = "Similarity could not be calculated"
    elif similarity >= 0.8:
        level = "Highly Similar"
    elif similarity >= 0.5:
        level = "Moderately Similar"
    else:
        level = "Less Similar"

    print("Word similarity:", similarity)
    print("Result:", level)
```

## Program 5: Identify speech acts (question, command, statement)

``` python
sentence = input("Enter a sentence: ").strip()

if sentence.endswith("?"):
    print("Speech Act: Question")
elif sentence.lower().startswith(("please", "go", "do", "open")):
    print("Speech Act: Command")
else:
    print("Speech Act: Statement")
```

## Program 6: Analyze a speech signal using Python libraries

``` python
import librosa

audio, sr = librosa.load("sample.wav")

print("Sample Rate:", sr)
print("Audio Length (samples):", len(audio))
print("Audio Duration (seconds):", len(audio) / sr)
```

## Program 7: Convert text to phoneme representation

``` python
import nltk
from nltk.corpus import cmudict

nltk.download("cmudict")
dictionary = cmudict.dict()

word = input("Enter word: ").lower()

if word in dictionary:
    print("Phonemes:", dictionary[word][0])
else:
    print("Word not found")
```

## Program 8: Demonstrate ASCII vs Unicode encoding

``` python
text = input("Enter the text: ")

print("ASCII:")
for ch in text:
    if ord(ch) < 128:
        print(ch, ord(ch))
    else:
        print(ch, "Not representable in ASCII")

print("\nUnicode code points:")
for ch in text:
    print(ch, ord(ch))
```

## Program 9: Multilingual text input and normalization

``` python
import unicodedata

text = input("Enter text: ")
text = unicodedata.normalize("NFC", text)
text = text.lower().strip()

print("Normalized Text:", text)
```

## Program 10: Text preprocessing (tokenization, stopword removal, stemming, lemmatization)

``` python
import nltk
from nltk.tokenize import word_tokenize
from nltk.corpus import stopwords
from nltk.stem import PorterStemmer, WordNetLemmatizer

nltk.download("punkt_tab")
nltk.download("stopwords")
nltk.download("wordnet")
nltk.download("omw-1.4")

text = input("Enter the text: ")
tokens = word_tokenize(text)

stop = set(stopwords.words("english"))
filtered = [word for word in tokens if word.lower() not in stop]

stemmer = PorterStemmer()
stemmed = [stemmer.stem(word) for word in filtered]

lemmatizer = WordNetLemmatizer()
lemmatized = [lemmatizer.lemmatize(word) for word in filtered]

print("Tokens:", tokens)
print("Stopword Removed:", filtered)
print("Stemmed:", stemmed)
print("Lemmatized:", lemmatized)
```

## Program 11: Load and explore a corpus using NLTK

``` python
import nltk
from nltk.corpus import gutenberg

nltk.download("gutenberg")

print("Available Corpus Files:")
print(gutenberg.fileids())

text = gutenberg.words("austen-emma.txt")

print("\nFirst 20 Words:")
print(text[:20])

print("\nTotal Words:")
print(len(text))
```

## Program 12: Query WordNet for synonyms, antonyms, and hypernyms

``` python
import nltk
from nltk.corpus import wordnet

nltk.download("wordnet")

word = input("Enter a word: ").lower()
synsets = wordnet.synsets(word)

if not synsets:
    print("Word not found in WordNet")
else:
    synonyms = set()
    antonyms = set()
    hypernyms = set()

    for syn in synsets:
        for lemma in syn.lemmas():
            synonyms.add(lemma.name())
            for ant in lemma.antonyms():
                antonyms.add(ant.name())

        for hypernym in syn.hypernyms():
            hypernyms.add(hypernym.name().split(".")[0])

    print("\nSynonyms:")
    print("\n".join(sorted(synonyms)[:10]) or "None found")

    print("\nAntonyms:")
    print("\n".join(sorted(antonyms)[:10]) or "No antonyms found")

    print("\nHypernyms:")
    print("\n".join(sorted(hypernyms)[:10]) or "No hypernyms found")
```

## Program 13: Parse and extract information from an XML linguistic resource

Create a file named `data.xml` with this content:

``` xml
<corpus>
    <sentence id="1">
        <word>Natural</word>
        <word>Language</word>
        <word>Processing</word>
    </sentence>
    <sentence id="2">
        <word>XML</word>
        <word>parsing</word>
        <word>is</word>
        <word>easy</word>
    </sentence>
</corpus>
```

Save the following Python program as `program13.py` in the same folder:

``` python
import xml.etree.ElementTree as ET

tree = ET.parse("data.xml")
root = tree.getroot()

for sentence in root.findall("sentence"):
    sid = sentence.get("id")
    words = [word.text for word in sentence.findall("word")]

    print("\nSentence ID:", sid)
    print("Words:", words)
    print("Sentence:", " ".join(words))
```

## Program 14: Extract emails, dates, and URLs using regular expressions

``` python
import re

text = input("Enter text: ")

email_pattern = r"[a-zA-Z0-9_.+-]+@[a-zA-Z0-9-]+\.[a-zA-Z0-9-.]+"
emails = re.findall(email_pattern, text)
print("Emails Found:")
print(emails)

url_pattern = r"https?://[^\s]+"
urls = re.findall(url_pattern, text)
print("\nURLs Found:")
print(urls)

date_pattern = r"\d{2}[/\-]\d{2}[/\-]\d{4}|\d{4}-\d{2}-\d{2}"
dates = re.findall(date_pattern, text)
print("\nDates Found:")
print(dates)
```

## Program 15: Simple Finite State Automaton (FSA) for word recognition

``` python
word = input("Enter a word: ").lower()

state = 0

for ch in word:
    if "a" <= ch <= "z":
        state = 1
    else:
        state = -1
        break

if state == 1 and word:
    print("Accepted (Valid word)")
else:
    print("Rejected (Invalid word)")
```

## Program 16: Simple morphological analyzer (prefix and suffix stripping)

``` python
word = input("Enter a word: ").lower()
original = word

prefixes = [
    "un", "re", "in", "im", "il", "ir", "dis", "mis", "non",
    "pre", "post"
]

suffixes = [
    "ing", "ed", "er", "est", "ly", "ment", "ness", "ful",
    "less", "s", "es"
]

prefix_found = []
suffix_found = []

changed = True
while changed:
    changed = False
    for pre in prefixes:
        if word.startswith(pre) and len(word) > len(pre) + 2:
            prefix_found.append(pre)
            word = word[len(pre):]
            changed = True
            break

changed = True
while changed:
    changed = False
    for suf in suffixes:
        if word.endswith(suf) and len(word) > len(suf) + 2:
            suffix_found.append(suf)
            word = word[:-len(suf)]
            changed = True
            break

print("\nOriginal Word:", original)
print("Prefixes removed:", prefix_found if prefix_found else "None")
print("Suffixes removed:", suffix_found if suffix_found else "None")
print("Root Word:", word)
```

## Program 17: Perform POS tagging using NLTK

``` python
import nltk
from nltk.tokenize import word_tokenize

nltk.download("punkt_tab")
nltk.download("averaged_perceptron_tagger_eng")

text = input("Enter a sentence: ")
words = word_tokenize(text)
pos_tags = nltk.pos_tag(words)

print("\nPOS Tags:")
for word, tag in pos_tags:
    print(word, "->", tag)
```

## Program 18: Stochastic POS tagger using NLTK taggers

``` python
import nltk
from nltk.corpus import treebank
from nltk.tokenize import word_tokenize
from nltk.tag import DefaultTagger, UnigramTagger, BigramTagger

nltk.download("treebank")
nltk.download("punkt_tab")

train_data = treebank.tagged_sents()

default_tagger = DefaultTagger("NN")
unigram = UnigramTagger(train_data, backoff=default_tagger)
bigram = BigramTagger(train_data, backoff=unigram)

sentence = input("Enter a sentence: ")
words = word_tokenize(sentence)
result = bigram.tag(words)

print("\nPOS Tagged Output:")
for word, tag in result:
    print(word, "->", tag)
```

## Program 19: Demonstrate transformation-based-style POS tagging

``` python
import nltk
from nltk.tokenize import word_tokenize
from nltk.tag import pos_tag

nltk.download("punkt_tab")
nltk.download("averaged_perceptron_tagger_eng")

text = input("Enter sentence: ")
words = word_tokenize(text)
tags = pos_tag(words)

transformed = []
for word, tag in tags:
    if word.lower().endswith("ing"):
        tag = "VBG"
    elif word.lower().endswith("ly"):
        tag = "RB"
    elif word and word[0].isupper():
        tag = "NNP"
    elif word.isdigit():
        tag = "CD"
    transformed.append((word, tag))

print("\nTagged Output:")
for word, tag in transformed:
    print(word, "->", tag)
```

## Program 20: Handle unknown words in POS tagging

``` python
import nltk
from nltk.tokenize import word_tokenize
from nltk.tag import pos_tag

nltk.download("punkt_tab")
nltk.download("averaged_perceptron_tagger_eng")

text = input("Enter sentence: ")
words = word_tokenize(text)

for word in words:
    tag = pos_tag([word])[0][1]

    if tag == "NN":
        if word.lower().endswith("ing"):
            tag = "VBG"
        elif word.lower().endswith("ly"):
            tag = "RB"
        elif word and word[0].isupper():
            tag = "NNP"
        elif word.isdigit():
            tag = "CD"

    print(word, "->", tag)
```

## Program 21: Define a Context-Free Grammar (CFG) and parse sentences

``` python
import nltk
from nltk import CFG
from nltk.parse import ChartParser

grammar = CFG.fromstring("""
S -> NP VP
NP -> Det N | Det Adj N | 'I' | 'You'
VP -> V NP | V
Det -> 'a' | 'an' | 'the'
Adj -> 'big' | 'small' | 'red'
N -> 'cat' | 'dog' | 'apple' | 'boy' | 'girl'
V -> 'eat' | 'see' | 'like' | 'likes'
""")

parser = ChartParser(grammar)
sentence = input("Enter a sentence: ")
words = sentence.split()

try:
    trees = list(parser.parse(words))
    if trees:
        print("\nSentence follows the CFG!\n")
        for tree in trees:
            print(tree)
            tree.pretty_print()
    else:
        print("\nSentence does NOT follow the CFG.")
except ValueError:
    print("\nSentence contains words not in grammar.")
```

## Program 22: Use chart parsing to analyze an ambiguous sentence

``` python
import nltk
from nltk import CFG
from nltk.parse import ChartParser

grammar = CFG.fromstring("""
S -> NP VP
NP -> Pronoun
NP -> Det N
NP -> Det N PP
VP -> V NP
VP -> VP PP
PP -> P NP
Pronoun -> 'I'
Det -> 'the'
N -> 'man' | 'telescope'
V -> 'saw'
P -> 'with'
""")

sentence = "I saw the man with the telescope"
tokens = sentence.split()
parser = ChartParser(grammar)

print("Possible Parse Trees:\n")
for tree in parser.parse(tokens):
    print(tree)
    print()
    tree.pretty_print()
```

## Program 23: Load and analyze Treebank using NLTK

``` python
import nltk
from nltk.corpus import treebank

nltk.download("treebank")

sentences = treebank.tagged_sents()
print("Total sentences:", len(sentences))

print("\nFirst sentence:")
print(sentences[0])

words = treebank.words()
print("\nTotal words:", len(words))

print("\nFirst 10 words:")
print(words[:10])

tags = [tag for word, tag in treebank.tagged_words()]
freq = nltk.FreqDist(tags)

print("\nMost common POS tags:")
print(freq.most_common(5))
```

## Program 24: Represent sentence meaning using logical forms

``` python
sentence = input("Enter a sentence: ")
words = sentence.lower().replace(".", "").split()

if len(words) < 2:
    print("Not enough words to form a logical representation")
else:
    subject = words[0]
    verb = words[1]

    if len(words) > 2:
        obj = "_".join(words[2:])
        logical_form = f"{verb}({subject}, {obj})"
    else:
        logical_form = f"{verb}({subject})"

    print("\nLogical Form:")
    print(logical_form)
```

## Program 25: Simple English-to-Malayalam translation using TF-IDF similarity

``` python
from sklearn.feature_extraction.text import TfidfVectorizer
from sklearn.metrics.pairwise import cosine_similarity

english = [
    "i am student",
    "how are you",
    "i love machine learning",
    "what is your name",
    "good morning",
    "i am fine",
    "where are you going",
    "i need help",
    "thank you",
    "good night"
]

malayalam = [
    "ഞാൻ വിദ്യാർത്ഥി ആണ്",
    "സുഖമാണോ",
    "എനിക്ക് മെഷീൻ ലേണിംഗ് ഇഷ്ടമാണ്",
    "നിങ്ങളുടെ പേര് എന്താണ്",
    "സുപ്രഭാതം",
    "എനിക്ക് സുഖമാണ്",
    "നിങ്ങൾ എവിടേക്ക് പോകുന്നു",
    "എനിക്ക് സഹായം വേണം",
    "നന്ദി",
    "ശുഭ രാത്രി"
]

vectorizer = TfidfVectorizer()
X = vectorizer.fit_transform(english)

def translate(sentence):
    sentence_vec = vectorizer.transform([sentence])
    similarity = cosine_similarity(sentence_vec, X)
    index = similarity.argmax()
    score = similarity[0][index]

    if score < 0.2:
        return "Translation not found in dataset"

    return malayalam[index]

while True:
    text = input("\nEnter English sentence (or 'exit'): ").lower()

    if text == "exit":
        break

    print("Malayalam Translation:", translate(text))
```

------------------------------------------------------------------------

## Notes

-   Each program is separate. Copy one code block at a time into its own
    `.py` file, or run it in a notebook.
-   NLTK may download datasets the first time a program runs, so an
    internet connection may be needed initially.
-   Program 13 requires `data.xml` beside the Python script.
-   Program 6 requires `sample.wav` beside the Python script.
-   Programs 18 and 19 are simplified classroom demonstrations: Program
    18 uses NLTK's unigram/bigram taggers, and Program 19 applies
    hand-written tag adjustments rather than training a full TBL model.
-   Program 25 uses a small phrase list and TF-IDF similarity; it is a
    demonstration, not a general-purpose translation system.
