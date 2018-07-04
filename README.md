# spelling-correction-nltk
detecting miss spelled word and give suggestions/correction is our goal
I would like to thanks www.nltk.org for it's tremendously nice documentation.

to install nltk just visit your command prompt or jupyter notebook and type 'python' then 'nltk.download()' 

Here we are using a model that is known as `Bag of words` We know that language is very complicated, but we can create a simplified model of language that captures part of the complexity. In this model, we avoid the order of words, but carry their frequencies. Here's a function to sample an n word sentence from a bag of words:

```python 

def samples(bag,  n=10):
         "Sample a random n-word sentence from the model described by the bag of words.""Sample  
    return ' '.join(random.choice(bag) for _ in range(n))
```

linguist 'George Zipf' Doctorate from Harvard, noted that in any big text file, the nth most frequent word appears with a frequency of about 1/n. He is the enventor of Zipf's Law. If we plot the frequency of words, most common first, on a log-log plot, they should come out as a straight line if Zipf's Law holds. Here we see that it is a fairly close fit:

![title](https://github.com/nirajdevpandey/spelling-correction-nltk/blob/master/data/Unbenannt.PNG)

Given a word w, find the most likely correction c = correct(w).

Approach: Try all candidate words c that are known words that are near w. Choose the most likely one.

How to balance near and likely?

For this time we always prefer nearer, but when there is a tie on nearness, then the program use the word with the highest WORDS count. to Measure nearness we are going to use edit distance.I have determined that going out to edit distance 2 will give us reasonable results. Then we can define correct(w)

```python
def correct(word):
    "Find the best spelling correction for this word."
    # Prefer edit distance 0, then 1, then 2; otherwise default to word itself.
    candidates = (known(edits0(word)) or 
                  known(edits1(word)) or 
                  known(edits2(word)) or 
                  [word])
    return max(candidates, key=COUNTS.get)
```

### The functions known and edits0 are easy; and edits2 is easy if we assume we have edits1

```python
def known(words):
    "Return the subset of words that are actually in the dictionary."
    return {w for w in words if w in COUNTS}

def edits0(word): 
    "Return all strings that are zero edits away from word (i.e., just word itself)."
    return {word}

def edits2(word):
    "Return all strings that are two edits away from this word."
    return {e2 for e1 in edits1(word) for e2 in edits1(e1)}
```

finally we will make few more functions to help us in order to detect miss spelled word and suggest a correct one. 

```python
def correct_text(text):
    "Correct all the words within a text, returning the corrected text."
    return re.sub('[a-zA-Z]+', correct_match, text)

def correct_match(match):
    "Spell-correct word in match, and preserve proper upper/lower/title case."
    word = match.group()
    return case_of(word)(correct(word.lower()))

def case_of(text):
    "Return the case-function appropriate for text: upper, lower, title, or just str."
    return (str.upper if text.isupper() else
            str.lower if text.islower() else
            str.title if text.istitle() else
            str)
```

Here are the results 

![title](https://github.com/nirajdevpandey/spelling-correction-nltk/blob/master/data/Unbenannt1.PNG)

it's done...!!

### Cheers :) 
