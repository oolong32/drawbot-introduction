#  Programmieren für Designer — Python und DrawBot

-  *Python* ist eine Programmiersprache
-  *DrawBot* ist eine Art Grafik-Automat (für Python)

Zuerst geht es darum, einige grundlegende Prinzipien einzuführen. Wir schauen uns die Basics von Python an, fortgeschrittenere Konzepte wie *Schleifen* und natürlich auch schon einige DrawBot-Funktionen.

## Bevor es losgeht

-  [DrawBot](drawbot.com/content/download) installieren
-  „Terminal“ öffnen (in Programme/Dienstprogramme)

## Das Terminal

Das *Terminal* ist eine Software, in der mit Text-Kommandos mit dem Computer interagiert werden kann. Es ist ein sehr vielseitiges und mächtiges Werkzeug, wir brauchen es hier aber nur, um mit Python warm zu werden.

Im Terminal „python3“ eingeben.

*Es gibt auch das Kommando „python“. Auf Apple Rechnern sind zwei Python-Versionen installiert, zwischen denen es Unterschiede gibt. In Drawbot arbeitest Du mit Version 3.*

```
$ python3
Python 3.7.5 (default, Nov  1 2019, 02:16:32)
[Clang 11.0.0 (clang-1100.0.33.8)] on darwinType "help", "copyright", "credits" or "license" for more information.
>>> print(hello)
Traceback (most recent call last):
    File "<stdin>", line 1, in <module>
    NameError: name 'hello' is not defined
    >>> print('hello')
    hello
```

Die *Python-Konsole* ist ein Programm, das gestartet wird, wenn du im *Terminal* „python3“ eingibst. Es dient dazu, Python-Code auf die Schnelle zu testen.

Die *Python-Konsole* kann mit der Tastaturkombination **ctr D** beendet werden.

Dieser Text wird Details, deren Verständnis nicht unmittelbar notwendig ist, der Übersichtlichkeit halber nur streifen. Die Konsole ist ideal, um mit Befehlen, die nicht auf Anhieb verständlich sind, zu experimentieren. Probieren geht über studieren.

```
>>> 1 + 1
2
>>> 5 -  3
2
>>> 2 * 3
6
>>> 8 / 2
4
>>> 2 * (3 + 4)
14
>>> 7 % 2
1
```

## Rechnen mit Programmiersprachen

```
+ Addition  
- Subtraktion  
/ Division
// Division ohne Rest
* Multiplikation  
% Modulo (Rest der Division)
```

- Beachte die zwei Arten von Division: `/` ergibt immer ein Resultat mit Kommastelle, auch wenn die Rechnung aufgeht.
- Abstände zwischen den Zahlen und den Operatoren sind nicht zwingend, können den Code aber leichter lesbar machen.

```
>>> zwei = 2
>>> drei = 3
>>> zwei + drei
(fünf)
>>> etwas = 'hello'
>>> print(etwas)
(na sowas)
```

### Variablen und Wertzuweisung

*Zwei*, *drei*, *etwas* sind Variablen. Sie dienen dazu, Werte zu speichern.

Einer Variable wird mit einem Gleichzeichen (=) ein Wert zugewiesen.
Das kann eine Zahl sein (integer, float), eine Zeichenfolge (string) oder eine andere Variable.

### Datentypen, Terminologie

*Integer* sind ganze Zahlen, *float* sind Zahlen mit Kommastelle.
*Strings* müssen in Anführungszeichen gesetzt werden – egal ob Einfache oder Doppelte.

```
>>> name = 'Maria'
>>> print('hoi', name)
```

Mit dem Befehl *print* können Variablen, Zeichenfolge oder andere Werte in die Konsole geschrieben werden.

Man beachte die Funktionsweise des Kommas.

---

## Schleifen

```
>>> for z in [1, 'hoi', 5]:
…	print(z)
…
1
hoi
5
```

*Z* ist eine Variable.

`[1, 'hoi', 3]` ist eine *Liste* von Werten (zwei *integer* und ein *string*).

`for … in … :` definiert einen *Loop* (Schleife). Was darunter eingerückt steht, wird für jedes Element in der *Liste* wiederholt.

## Die Funktion range()

Die folgende Funktion werden wir in DrawBot so viel brauchen, dass ich sie gleich zu Beginn durchnehmen möchte. Was Funktionen sind, werden wir später genauer erfahren.

```
>>> range(10)
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
```

`range(N)` ist eine Funktion, die eine Liste von Zahlen von 0 bis N liefert.
Damit ist es möglich, in einem for-Loop eine Anzahl Wiederholungen (Iterationen) zu bestimmen.

Interessanterweise beginnt der Computer bei 0 zu zählen, hört dafür vor der in Klammern angegebenen Zahl auf.

### `range()` und ihre Argumente

```
>>> range(3, 10)
[3, 4, 5, 6, 7, 8, 9]

>>> range(3, 10, 2)
[3, 5, 7, 9]

>>> range(10, 0, -1)
[10, 9, 8, 7, 6, 5, 4, 3, 2, 1]
```

-  es muss nicht immer bei 0 beginnen
-  es kann in grösseren Schritten als 1 gezählt werden
-  es geht auch rückwärts

Eine Gelegenheit, einmal in der [Python-Dokumentation](https://docs.python.org/2/library/functions.html#range) nachzuschauen.

### Loop mit range()

```
>>> for a in range(6):
…	print(a)
…
0
1
2
3
4
5
```

---

## DrawBot

DrawBot öffnen.

Die Applikation besteht aus drei Teilen:

1. Im Eingabefenster werden Programme geschrieben.
2. In der Konsole lassen sich mit dem Befehl `print` Werte ausgeben.
3. Im Ausgabefenster erscheinen die Früchte unserer Arbeit.

Ins Eingabefenster wird in der Sprache Python geschrieben.

Mit der Tastenkombination `cmd r` wird das Programm ausgeführt.

Zum Aufwärmen schreiben wir ein paar Kommandos ins Eingabefenster und geben sie in der Konsole aus.

```
x = 100
y = 200

print(x + y)
```

Neu ist, dass stehen bleibt, was wir schreiben.

Den Inhalt des Eingabefensters können wir als Datei abspeichern. Das Suffix „py“ steht für Python.

---

## Funktionen

Der Begriff *Funktion* bezeichnet in Programmiersprachen eine bestimmte Art von Befehlen. Mit der Funktion `range()` hatten wir ja schon das Vergnügen.
Wir erkennen Funktionen daran, dass sie am Ende eine Klammer haben. Manchmal steht da *Argumente* drin, manchmal nicht.

### Argumente

Ein Argument bezeichnet einen Wert, der in die Klammern einer Funktion geschrieben wird.

Allerdings benötigen nicht alle Funktionen ein Argument. Besipielsweise akzeptiert die Funktion `range()` eines, zwei oder drei davon.

### DrawBot-eigene Funktionen

DrawBot verfügt über eine grosse Anzahl eigener Funktionen.

Eine davon heisst `newPage()`, sie generiert eine neue leere Seite.

```
newPage(400, 400)
```

Eine weitere heisst `oval()`, sie zeichnet eine Ellipse.

```
oval(0, 0, 100, 100)
```

Eine Übersicht aller Funktionen in DrawBot gibt es in der [DrawBot Referenz](http://www.drawbot.com/content/quickReference.html).

## Aufgabe 1

In die Funktion `oval()` verschiedene Werte einfügen, ausführen und beobachten, was sich ändert.

### Frage

Wofür stehen die Werte in den Klammern?

## Koordinatensystem und Masseinheiten in DrawBot

Die Position eines Elements wird mit Koordinaten angegeben. Der Nullpunkt liegt in DrawBot unten links (Processing/p5: oben links). 

Die Masseinheit in DrawBot ist je nach Ausgabeformat Pixel (Screen) oder DTP-Punkt (Print).

## Aufgabe 2

-  was machen die Funktionen height() und width()?
-  Einen Kreis ins rechte obere Seitenviertel zeichnen.

```
newPage(400, 400)
oval(width()/2, height()/2, 200, 200)
```

oder

```
oval(width()/2, height()/2, width()/2, height()/2)
```

## Aufgabe 3

-  für jedes Argument in der Funktion `oval()` eine Variable schreiben: X-Position, Y-Position, und Radius
-  Argumente gegen Variablen tauschen
-  Wie bringst du das Kreiszentrum in die Mitte der Seite?

```
newPage(400, 400)

x_pos = height()/2
y_pos = width()/2
rad = 50 # Achtung, die oval-Funktion will Durchmesser, nicht Radius

oval(x_pos -  rad, y_pos - rad, 2 * rad, 2 * rad)
```

## Kommentare

Kommentare sind Abschnitte in Programmen, die nicht ausgeführt werden. In Python wird nicht ausgeführt, was nach einer Raute (#) steht.

-  Damit lassen sich Zeilen im Code ab- oder anschalten.
-  Damit lassen sich Kommentare schreiben, um ein Programm übersichtlicher zu machen und um sich später daran zu erinnern, zu was einzelne Abschnitte/Zeilen dienen.

---

P A U S E ?

---

## Zufallsgenerator

Zufällige Zahlenwerte lassen sich in Python auf verschiedene Arten generieren. Es gibt eine ganze Reihe von Funktionen dafür, die in einem separaten Modul namens „random“ befinden. Die Funktion `random()` wird in DrawBot automatisch geladen, alle anderen müssen zu Beginn eines Programms aus dem Modul importiert werden. 

```
# nur die Funktion randrange() importieren
from random import randrange

# alle Funktionen des Moduls 'random' importieren
from random import *
```

-  `random()` generiert zufällige Zahlen zwischen 0 und 1.
-  `randrange(a, b)` generiert ganze Zahlen (*integer*) zwischen a und b.
-  `uniform(a, b)` generiert reelle Zahlen (*float*) zwischen a und b.

Auch hier kann es sich lohnen, einmal in die [Python-Doku](https://docs.python.org/2/library/random.html) zu schauen:

## Ein Versuch mit Zufallszahlen, Errors und Debugging

Wir schreiben ein Programm, das ein Kreis zufälliger Grösse an  zufälliger Position auf einer A4 Seite zeichnet.
Es wird ein exemplarisches Probleme auftreten, darum schauen wir uns gleich auch mal an, was du tun kannst, wenn Fehlermeldungen auftreten.

```
from random import randrange
newPage('A4Landscape')

# Durchmesser
d = randrange(10, width())

x = randrange(width())
y = randrange(height())

oval(x, y, d, d)
```

Der Kreis soll bitte nicht über den rechten Seitenrand reichen.

```
from random import randrange
newPage('A4Landscape')

# Durchmesser
d = randrange(10, width())

x = randrange(width() -  d)
y = randrange(height() -  d)

oval(x, y, d, d)
```

### Error

Es gibt Fehlermeldungen. Fehlermeldungen zeigen, dass es mit einem Teil des Programms Probleme gibt. Meist enthalten sie Information, die darauf hindeuten, was schief gelaufen sein könnte.

```
Traceback (most recent call last):
  File "<untitled>", line 10, in <module>
  File "random.pyc", line 193, in randrange
ValueError: empty range for randrange()
```

-  Der Fehler passiert auf Zeile 10.
-  Es gibt ein Problem mit der Funktion randrange()

### Debugging

Wir printen `height()` und die Variable d in die DrawBot-Konsole. Und versuchen zu ermitteln, in welchen Fällen der Fehler auftritt.

Nach einigen Versuchen sehen wir, dass die Fehlermeldung immer dann erscheint, wenn d grösser wird als `height()`.

### Ansätze zur Lösung

Wir berechnen den Durchmesser als Wert zwischen 10 und der Seitenbreite, aber die Seite ist ein Querformat. Der Fehler tritt auf, wenn das erste Argument von `randrange()` grösser ist als das Zweite.

Eine Lösung wäre, den Kreisdurchmesser von der `height()` abhängig zu machen, anstatt von `width().` Das funktioniert aber nur solange, bis wir der Funktion `newPage()` ein Hochformat als Argument geben.

Die Python-Funktionen `min(a, b)` und `max(a, b)` liefern den Tieferen, respektive den Höheren von zwei Werten. 

```
from random import randrange
newPage('A4')

# der Durchmesser soll nicht breiter/höher als die Seite werden
max_width = min(width(), height())
d = randrange(10, max_width)

x = randrange(width() -  d)
y = randrange(height() -  d)

oval(x, y, d, d)
```

Der hier beschriebene Ablauf ist typisch für Problemstellungen, die beim Programmieren auftreten können: Ein Plan geht nicht so auf, wie gedacht – es treten unerwartete Fehler auf. Du versuchst, die Ursache des Fehlers zu finden und zu korrigieren.
Eine Lösung sollte den Fehler komplett zum Verschwinden bringen, damit er später nicht in einer Variante wieder auftreten kann (wenn z.B. das Ausgabeformat wechselt).

## Output in Datei schreiben

Mit der DrawBot-Funktion `saveImage()` lässt sich der Output in eine oder mehrere Dateien schreiben.

```
# als PDF speichern
saveImage('~/Desktop/datei.pdf')

# als PDF und GIF speichern
saveImage(['~/Desktop/datei.pdf', '~/Desktop/gif'])
```

## Loops

Eine Übung zum Thema Loops.

### Beachten

-  Doppelpunkt nach dem `for … in` Statement.
-  Den Abschnitt nach dem `for … in` Statement einrücken.

### Aufgabe 1

Mit dem Programm, das einen variablen Kreis auf eine Seite zeichnet, produzieren wir ein hundertseitiges Dokument.

```
for n in range(100):
    newPage('A4')
    # Kreis zeichnen etc.

saveImage('~/Desktop/datei.pdf')
```

### Aufgabe 2

-  Ändere den Code in der Schleife so, dass auf jede Seite 100 Kreise gezeichnet werden.
-  Und so, dass dass jede Seite eine andere Hintergrundfarbe hat.

```
r = random()
g = random()
b = random()
fill(r, g, b)
rect(posX, posY, width(), height())
```

---

- [Tag 2](tag2.md)
- [Tag 3](tag3.md)

---

![Creative Commons License](https://i.creativecommons.org/l/by-sa/4.0/80x15.png)

Dieser Text kann unter den Bedingungen der [«CC BY-SA» Creative Commons Lizenz](https://creativecommons.org/licenses/by-sa/4.0/legalcode.de) weiterverwendet werden.
