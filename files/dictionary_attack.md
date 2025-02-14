# Decryption with a dictionary attack

Decrypt a cyphertext by using the frequency of letters in a sample english text.

## Solution

```py
import requests
response = requests.get('https://raw.githubusercontent.com/lucaneg/inf1mod2/refs/heads/master/dictionary.txt')
dictionary_f = 'dictionary.txt'
with open(dictionary_f, 'w') as f:
  f.write(response.text)
response = requests.get('https://raw.githubusercontent.com/lucaneg/inf1mod2/refs/heads/master/cyphertext.txt')
cyphertext_f = 'cyphertext.txt'
with open(cyphertext_f, 'w') as f:
  f.write(response.text)

def make_hist(fname):
  hist = {}
  with open(fname) as f:
    text = f.read().lower()
  for l in text:
    if l in 'abcdefghijklmnopqrstuvwxyz':
      if l not in hist:
        hist[l] = 1
      else:
        hist[l] += 1
  return sorted(hist.items(), key=lambda item: item[1], reverse=True)

def poor_man_double_histogram(sorted_cipher, sorted_corpus, spaces=20):
  max_value_1 = max([x[1] for x in sorted_cipher])
  max_value_2 = max([x[1] for x in sorted_corpus])
  for i in range(len(sorted_cipher)):
    k1, v1 = sorted_cipher[i]
    k2, v2 = sorted_corpus[i]
    bar_len = int(spaces*v1/max_value_1)
    pad_value = spaces - bar_len
    bar_len2 = int(spaces*v2/max_value_2)
    pad_value_2 = spaces - bar_len2
    print(k1, 'x'*bar_len, ' '*pad_value, f'{v1:06}', k2, 'x'*bar_len2, ' '*pad_value_2, f'{v2:06}')

sorted_cipher = make_hist(cyphertext_f)
sorted_corpus = make_hist(dictionary_f)
poor_man_double_histogram(sorted_cipher, sorted_corpus)
dec_key = make_dict([x[0] for x in sorted_cipher], [x[0] for x in sorted_corpus])

# you can start improving the decyphered text by swapping letters!
# you can do it quickly by adding a string containing the letters to swap
# in the following list. for instance, try swapping i and l by adding 'il'
swaps = ['il', 'xu'] # add more!
for swap in swaps:
  dec_key[swap[0]], dec_key[swap[1]] = dec_key[swap[1]], dec_key[swap[0]]

for k,v in dec_key.items():
  print(f'{k} -> {v}')
with open(cyphertext_f) as f:
  candidate_p = encrypt(f.read(), dec_key)
print(candidate_p)
```
