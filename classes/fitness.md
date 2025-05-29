# Sistema di Tracciamento Fitness

Si vuole modellare un sistema per il tracciamento delle attività fisiche degli utenti. Un generico allenamento è rappresentato dalla classe `Workout`, che contiene informazioni di base come il nome dell'attività, la data in cui è stata svolta e le calorie bruciate durante la sessione.

Esistono due tipi di attività: `CardioWorkout` e `StrengthWorkout`, entrambe sottoclassi di `Workout`. La prima rappresenta un allenamento cardio e aggiunge attributi specifici come la durata in minuti e la frequenza cardiaca media registrata durante l’attività. La seconda rappresenta un allenamento di forza e include il peso degli attrezzi sollevati e il numero di ripetizioni effettuate. `StrengthWorkout` deve definire anche un metodo `get_total_kg_lifted`, che ritorna il peso totale sollevato durante l'allenamento moltiplicando il peso per il numero di ripetizioni.

Un utente del sistema è modellato dalla classe `User`, che contiene un nome e la lista dei workout svolti. Questa classe dovrà implementare due metodi: `get_avg_cardio_heart_rate`, che ritorna il battito medio durante tutti gli esercizi cardio effettuati, e `get_total_weights_lifted`, che ritorna la somma dei valori ritornati da `get_total_kg_lifted` degli allenamenti di forza. In entrambi i metodi, è richiesto l'utilizzo del **duck-typing** per capire il tipo dell'allenamento.
Inoltre, dovrà essere possibile aggiungere nuovi workout alla routine dell'utente utilizzando l'operatore `+=`. 

Deve essere possibile convertire in stringa sia i workout che gli utenti (nelle stringhe di seguito, gli attributi da utilizzare sono racchiusi tra `< >`):
- convertendo un allenamento cardio bisogna ottenere la stringa `cardio workout <nome> del <data>, <durata> minuti (<calorie> calorie, battito medio <frequenza>)`
- convertendo un allenamento di forza bisogna ottenere la stringa `workout di forza <nome> del <data>, <ripetizioni>x<peso>kg (<calorie> calorie)`
- convertendo un utente bisogna ottenere la stringa `<nome>:\n<lista dei workout separati da \n>`

Tutti gli attributi delle classi vanno inizializzati tramite parametri del costruttore, nello stesso ordine in cui vengono specificati nel testo dell'esercizio. L'unica eccezione è la lista di workout svolti da un utente, che va creata vuota senza utilizzare un parametro.

Link utili:
- [Documentazione Python](https://docs.python.org/3/)
- [Google Colab](https://colab.google.com)
- [Moodle](https://moodle.unive.it)

## Template

```py
if __name__ == '__main__':
  from datetime import date
  nworks = int(input())
  user = User(input())
  for _ in range(nworks):
    i = input().split()
    if i[0] == 'c':
      user += CardioWorkout(i[1], date.today(), int(i[2]), int(i[3]), int(i[4]))
    else:
      user += StrengthWorkout(i[1], date.today(), int(i[2]), int(i[3]), int(i[4]))
  
  print(user)
  print('Avg Heart Rate:', user.get_avg_cardio_heart_rate())
  print('Total Weights Lifted', user.get_total_weights_lifted())
```

## Testcases

### 1

IN:
```
3
Luca
c Corsa 400 30 150
s Panca 200 80 10
c Bici 350 45 140
```

OUT:
```
Luca:
cardio workout Corsa del 2025-05-27, 30 minuti (400 calorie, battito medio 150)
workout di forza Panca del 2025-05-27, 10x80kg (200 calorie)
cardio workout Bici del 2025-05-27, 45 minuti (350 calorie, battito medio 140)
Avg Heart Rate: 145.0
Total Weights Lifted 800
```

## Solution

```py
class Workout:
  def __init__(self, name, date, kcal):
    self.name = name
    self.date = date
    self.kcal = kcal

class CardioWorkout(Workout):
  def __init__(self, name, date, kcal, duration, avg_heart_rate):
    super().__init__(name, date, kcal)
    self.duration = duration
    self.avg_heart_rate = avg_heart_rate

  def __str__(self):
    return f"cardio workout {self.name} del {self.date}, {self.duration} minuti ({self.kcal} calorie, battito medio {self.avg_heart_rate})"

class StrengthWorkout(Workout):
  def __init__(self, name, date, kcal, kgs, reps):
    super().__init__(name, date, kcal)
    self.kgs = kgs
    self.reps = reps
  
  def get_total_kg_lifted(self):
    return self.kgs * self.reps

  def __str__(self):
    return f"workout di forza {self.name} del {self.date}, {self.reps}x{self.kgs}kg ({self.kcal} calorie)"

class User:
  def __init__(self, name):
    self.name = name
    self.workouts = []

  def __iadd__(self, workout):
    self.workouts.append(workout)
    return self

  def get_avg_cardio_heart_rate(self):
    total = 0
    count = 0
    for w in self.workouts:
      if hasattr(w, 'avg_heart_rate'):
        total += w.avg_heart_rate
        count += 1
    return total / count

  def get_total_weights_lifted(self):
    total = 0
    for w in self.workouts:
      if hasattr(w, 'get_total_kg_lifted'):
        total += w.get_total_kg_lifted()
    return total

  def __str__(self):
    ws = "\n".join(str(w) for w in self.workouts)
    return f"{self.name}:\n{ws}"
```
