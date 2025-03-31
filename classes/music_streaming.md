# Streaming di musica

Un sito online permette agli utenti di ascoltare musica o podcast, e di organizzarle in playlist. Un contenuto disponibile sul sito è modellato dalla classe `AudioContent`, che è descritta da un titolo, dal nome dell'autore del contenuto, e dalla sua durata in secondi. I contenuti possono essere di due tipi: canzoni o podcast. Entrambi sono modellati da classi (`Song` e `Podcast`) che ereditano da `AudioContent`, e aggiungono gli attributi genere e album per le canzoni, e data (`datetime.date`) e numero dell'episodio per i podcast. 
Tutti i contenuti possono essere organizzati in playlist (modellate dalla classe `Playlist`), che ha un nome e una lista di contenuti. Infine, un utente del sito è modellato dalla classe `User`, che contiene l'username e una lista di playlist.

Implementare le 5 classi menzionate, assicurandosi che:
- tutti gli attributi vengano inizialiizati nel costruttore utilizzando dei parametri, fatta eccezione per la lista di brani in una playlist e la lista di playlist di un utente che devono essere sempre inizializzati con una lista vuota (l'ordine dei parametri ricevuti dal costruttore è lo stesso con cui gli attributi vengono menzionati nel testo)
- tutte le classi (esclusa `AudioContent`) devono poter essere convertite in stringhe tramite la funzione `str()` (negli esempi, `<x>` corrisponde all'attributo `x` dell'oggetto):
  - convertendo una canzone, la stringa deve avere la struttura `<titolo> by <autore> (<durata>) in <album>, <genere>`
  - convertendo un podcast, la stringa deve avere la struttura `<titolo> by <autore> (<durata>), episodio <episodio> del <data>`
  - convertendo una playlist, la stringa deve avere la struttura `playlist <nome>:\n<lista di contenuti separati da \n>`
  - convertendo un utente, la stringa deve avere la struttura `utente <nome>, playlist: <lista dei nomi delle playlist separate da , e spazio>` (**attenzione: solo i nomi delle playlist!**)
- deve essere possibile aggiungere contenuti a una playlist utilizzando l'operatore `+` **in entrambi i sensi** (cioè, data una playlist `p` e un contenuto `c`, deve essere possibile fare sia `p = p + c` che `p = c + p`) definendo operatori solo all'interno della classe `Playlist`, che devono lanciare un `TypeError` se i tipi dei parametri non sono corretti
- deve essere possibile aggiungere playlist a un utente utilizzando `+=`, che lancia un `TypeError` se i tipi dei parametri non sono corretti

## Template

```py
if __name__ == '__main__':
    import datetime as dt
    n_content = int(input())
    contents = []
    for _ in range(n_content):
        c = input().split()
        if c[0] == 's':
            contents.append(Song(c[1], c[2], int(c[3]), c[4], c[5]))
        else:
            contents.append(Podcast(c[1], c[2], int(c[3]), dt.datetime.strptime(c[4], '%Y-%m-%d').date(), int(c[5])))
    
    n_playlist = int(input())
    playlists = []
    for _ in range(n_playlist):
        p = input().split()
        playlist = Playlist(p[0])
        for i in range(1, len(p)):
            if i % 2 == 0:
                playlist = playlist + contents[int(p[i])]
            else:
                playlist = contents[int(p[i])] + playlist
        playlists.append(playlist)
    
    u = input()
    user = User(u)
    for p in playlists:
        print(p)
        user += p
    print(user)
```

## Testcases

### 1

IN:
```
3
s Canzone1 Autore1 187 Pop Album1
p Podcast1 Autore1 1576 2025-01-31 4
s Canzone2 Autore1 121 Rock Album2
2
Preferiti 0 2
Podcast 1
Gianni
```

OUT:
```
playlist Preferiti:
Canzone1 by Autore1 (187) in Album1, Pop
Canzone2 by Autore1 (121) in Album2, Rock
playlist Podcast:
Podcast1 by Autore1 (1576) episodio 4 del 2025-01-31
utente Gianni, playlist: Preferiti, Podcast
```

### 2

IN:
```
5
s Canzone1 Autore1 187 Pop Album1
p Podcast1 Autore1 1576 2025-01-31 4
s Canzone2 Autore1 121 Rock Album2
p Podcast2 Autore1 376 2025-02-12 5
p Podcast3 Autore5 1900 2025-01-18 1
1
DaAscoltare 0 1 2 3 4
Paolo
```

OUT:
```
playlist DaAscoltare:
Canzone1 by Autore1 (187) in Album1, Pop
Podcast1 by Autore1 (1576) episodio 4 del 2025-01-31
Canzone2 by Autore1 (121) in Album2, Rock
Podcast2 by Autore1 (376) episodio 5 del 2025-02-12
Podcast3 by Autore5 (1900) episodio 1 del 2025-01-18
utente Paolo, playlist: DaAscoltare
```

## Solution

```py
class AudioContent:
  def __init__(self, title, author, duration):
    self.title = title
    self.author = author
    self.duration = duration

class Song(AudioContent):
  def __init__(self, title, author, duration, genre, album):
    super().__init__(title, author, duration)
    self.genre = genre
    self.album = album
  
  def __str__(self):
    return f'{self.title} by {self.author} ({self.duration}) in {self.album}, {self.genre}'

class Podcast(AudioContent):
  def __init__(self, title, author, duration, date, episode):
    super().__init__(title, author, duration)
    self.date = date
    self.episode = episode
  
  def __str__(self):
    return f'{self.title} by {self.author} ({self.duration}) episodio {self.episode} del {self.date}'

class Playlist:
  def __init__(self, name):
    self.name = name
    self.contents = []
  
  def __str__(self):
    content = '\n'.join([str(c) for c in self.contents])
    return f'playlist {self.name}:\n{content}'
  
  def __add__(self, other):
    if not isinstance(other, AudioContent):
      raise TypeError()
    p = Playlist(self.name)
    for c in self.contents:
      p.contents.append(c)
    p.contents.append(other)
    return p
  
  def __radd__(self, other):
    return self + other

class User:
  def __init__(self, name):
    self.name = name
    self.playlists = []
  
  def __str__(self):
    playlists = ', '.join([p.name for p in self.playlists])
    return f'utente {self.name}, playlist: {playlists}'
  
  def __iadd__(self, other):
    if not isinstance(other, Playlist):
      raise TypeError()
    self.playlists.append(other)
    return self
```
