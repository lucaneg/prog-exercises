# Encrypting files

1. Write a function that encrypts and/or decrypts the lorem in the next code cell using key K
2. Save the lorem in a file `lorem.txt`, then open it, encrypt it, save the encrypted text in `cipher.txt`. Open again `cipher.txt`, decrypt it, save the candidate decription in `plaintext.txt`.
3. Open the original `lorem.txt`, and check that `plaintext.txt` has the same content.

## Solution

```py
import requests
response = requests.get('https://raw.githubusercontent.com/lucaneg/inf1mod2/refs/heads/master/lorem.txt')
lorem = response.text

lorem_f = 'lorem.txt'
cipher_f = 'cipher.txt'
candidate_f = 'plaintext.txt'
A = 'abcdefghijklmnopqrstuvwxyz'
K = 'qwertyuiopasdfghjklzxcvbnm'

def make_dict(frm, to):
  key_dict = {}
  for idx, letter in enumerate(frm):
    key_dict[letter] = to[idx]
  return key_dict

def encrypt(text, key):
  enc_text = []
  for l in text:
    try:
      enc_text.append(key[l])
    except KeyError:
      enc_text.append(l)
  return ''.join(enc_text)

enc_dict = make_dict(A, K)
dec_dict = make_dict(K, A)

with open(lorem_f, 'w') as f:
  f.write(lorem.lower())

with open(lorem_f) as f:
  txt = f.read()
ciphertext = encrypt(txt, enc_dict)
with open(cipher_f, 'w') as f:
  f.write(ciphertext)

with open(cipher_f) as f:
  cipher = f.read()
decipher = encrypt(cipher, dec_dict)
with open(candidate_f, 'w') as f:
  f.write(decipher)

with open(lorem_f) as l:
  with open(candidate_f) as c:
    print(l.read() == c.read())
```
