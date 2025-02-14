# Frequency histogram

The 'Lorem ipsum' is a typical text used in computer science and web deskign to fill paragraph with text to show how they will appear when filled out with real text. An example of such text can be seen [here](https://gist.githubusercontent.com/lucaneg/935a380c8b2a6f5c33c1dd264fe48e88/raw/210316a1b05079e552601f540db2170353c1c570/lorem.txt).

Extract from such text the frequency histogram of words, and plot a bar chart of the 10 most frequent words. To properly compute the plot, you should remove all punctuation and be case-insensitive when counting words. To read the text, we must read the file over the internet. The easiest way to achieve this is by using the `requests` module:
```python
import requests
url = '...'
response = requests.get(url)
print (response.text)
```
Use the above url as parameter of `requests.get()` to retrieve the text.

## Solution

```py
import matplotlib.pyplot as plt
import string
import requests

response = requests.get('https://gist.githubusercontent.com/lucaneg/935a380c8b2a6f5c33c1dd264fe48e88/raw/210316a1b05079e552601f540db2170353c1c570/lorem.txt')
lorem = response.text

# first we split the sting and remove punctuation and casing
lorem_split = lorem.split()
for i in range(len(lorem_split)):
  # https://docs.python.org/3/library/stdtypes.html#str.translate
  # https://docs.python.org/3/library/stdtypes.html#str.maketrans
  lorem_split[i] = lorem_split[i].lower().translate(str.maketrans('', '', string.punctuation))

# these are all the words we need to count, with no duplicates
unique_words = set(lorem_split)

# dictionary comprehension: same concept as the list comprehension,
# but to generate dictionaries
frequencies = { word:lorem_split.count(word) for word in unique_words }

# we sort the dictionary to get the first ten words
sorted_frequencies = sorted(frequencies.items(), key=lambda item: item[1], reverse=True)

# finally, extract the first 10 words with their frequencies
labels = [x[0] for x in sorted_frequencies[:10]]
values = [x[1] for x in sorted_frequencies[:10]]

fig, axes = plt.subplots()
axes.bar(labels, values, color='red')
axes.set_xlabel('Word')
axes.set_ylabel('Frequency')
axes.set_title('Words histogram')
```
