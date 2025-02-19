# Plotting-wordclouds

## Project Overview

**Database**: `(http://shakespeare.mit.edu/othello/full.html)`

This project is designed to demonstrate Python skills and techniques typically used by data analysts to explore, clean, and analyze wordcloud. The project involves setting up a database, cleaning up the data, formatting the text, removing unnecessary punctuations.

## Project Structure

### 1. Database Setup

```python
import nltk
#nltk.download('stopwords')
#nltk.download('punkt')
#nltk.download('words')
from nltk.corpus import stopwords, words
import string
import itertools

from wordcloud import WordCloud, STOPWORDS, ImageColorGenerator

#import pandas as pd
import matplotlib.pyplot as plt
filename = '/Users/iayushjav/Downloads/othello.txt'
with open(filename) as f:
    text = f.read()
```

### 2. Split the text into words

```python
myWords = text.split()
```

### 3. Count how many times each word is repeated

```python
wordsDict = {}
for word in myWords:
    if word in wordsDict:
        wordsDict[word] += 1
    else:
        wordsDict[word] = 1
wordsDict = dict(wordsDict[::-1])
```

### 4. Bar graph for 50 most repeated words
```python
n = 50 # just 50 most popular
wordsPlot = {k: wordsDict[k] for k in list(wordsDict)[:n]}

fig, ax = plt.subplots(figsize=(20,10))

ax.bar(range(len(wordsPlot)), list(wordsPlot.values()), align='center')

ax.set_xticks(range(len(wordsPlot)))
ax.set_xticklabels(list(wordsPlot.keys()))

plt.xticks(rotation=90)

plt.show()
```
<img width="604" alt="bar plt" src="https://github.com/user-attachments/assets/8eddcce6-caa2-49a3-babd-857ffccf3eb9" />

From this we can see that there is a lot of "stop words", i.e. words like "i" or "the" that are common but we probably don't want to count in our analysis.

### 5. Removing stop words
```python
stop_words =set(stopwords.words('english'))
less_words = {}
for w, num in wordsDict.items():
    if w.lower() not in stop_words:
        less_words[w] = num
```

### 5. Plot the 50 most popular words
```python
n = 50 # just 50 most popular
wordsPlot = {k: less_words[k] for k in list(less_words)[:n]}

fig, ax = plt.subplots(figsize=(20,10))

ax.bar(range(len(wordsPlot)), list(wordsPlot.values()), align='center')

ax.set_xticks(range(len(wordsPlot)))
ax.set_xticklabels(list(wordsPlot.keys()))

plt.xticks(rotation=90)

plt.show()
```
<img width="602" alt="bar plt2" src="https://github.com/user-attachments/assets/46a92f4b-5016-4623-80ba-daf332fc910a" />

So, now we can see that the names of each of the characters are most often repeated, which makes some sort of sense! We have a different problem though - there is also punctuation included here!

In some cases it makes sense, like "I'll" vs "Ill", but sometimes it does not, like "him," vs "him". When and how to take out punctuation can be tricky.

### 6. Remove punctuation and capitalization

```python
word_tokens = nltk.word_tokenize(" ".join(myWords))
less_words = [w for w in word_tokens if not w.lower() in stop_words]
no_punc_words = []
for word in less_words:
    if word[-1] in string.punctuation:
        word = word[:-1]
    # make sure it wasn't ONLY punctuation
    if len(word) > 0:
        no_punc_words.append(word)
```

7. **Creating wordcloud picturea **:
```python
wordcloud = WordCloud().generate_from_frequencies(wordsDict)
fig, ax = plt.subplots(figsize=(10,5))        

ax.imshow(wordcloud)
        
plt.show()
```
<img width="593" alt="wordcloud" src="https://github.com/user-attachments/assets/a4db99d4-1a64-4b5c-9a58-c11cefd51560" />

