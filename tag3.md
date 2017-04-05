# Programmieren für Designer — Python und DrawBot, Tag 3


## Was wir letztes Mal angeschaut haben

- Raster generieren mit Schleifen
- die DrawBot-Funktionen `translate()`, `rotate()`, `save()`, `restore()`
- Boolsche Werte: True/False
- Operatoren zum Vergleichen: <, >, >=, <=, ==, !=, and, or
- Bedingungen: if/else

## Themen heute

- Listen, Schleifen und Einzelwerte: list[index]
- Funktionen DIY: def und return
- Schreiben in DrawBot

---

## Kapitel 1: Listen, Listen, Listen

Wir haben Listen kennengelernt, als wir uns zum ersten Mal mit Schleifen befasst haben.

```
liste = [12, 7, 'hoi']

for ding in liste:
    print ding
```

Die Funktion `range()` generiert Listen von Zahlen.

```
>>> print range(5)
[0, 1, 2, 3, 4]
```

Mit `range()` können wir bestimmen, wie oft eine Schleife wiederholt wird. Ist die Liste es eine fortlaufende Zahlenreihe, so erhalten wir auch einen Zähler (n im folgenden Beispiel).


```
for n in range(3):
    print n
```

Listen können noch mehr, als Zähler für Schleifen-Durchläufe aufzustellen. Sie können beispielsweise einen Haufen Werte speichern und bei Bedarf verfügbar machen.

```
randomValues = []

for n in range(10):
    neuerWert = random()
    randomValues.append(neuerWert)

for wert in randomValues:
    print wert
```

Die Funktion `append()` ist eine Funktion, die einen Wert ans Ende einer Liste anfügt. 
Syntax: `liste.append(wert)`
Beachte, dass die Funktion `append()` mit einem Punkt an den Listennamen angefügt wird.
Die Funktion `append()` existiert nur für Listen.

Fachterminologie: «Append ist eine Methode von Listen.»

#### Einzelwert aus Liste (jetzt kommts)

Jeder Wert einer Liste hat einen *Index*. Damit ist die Reihenfolge in der Liste gemeint. Der *Index* des ersten Elements ist 0, das zweite Element hat Index 1, das dritte 2 etc.
Auch hier wird bei 0 mit Zählen begonnen.

```
>>> team = ['lilo', 'samson', 'jamadu', 'lillibiggs']
>>> print team[2]
jamadu
>>> print team[0]
lilo
```

Mit Angabe des *Index* lassen sich einzelne Werte aus einer Liste abfragen. Dazu hängt man eine eckige Klammer mit dem gewünschten *Index* an die Liste.

Im folgenden Beispiel werden die x- und y-Koordinaten in einer Liste gespeichert. Das kann sehr nützlich sein, wenn die gleichen Punkte mehrmals verwendet werden müssen.

```
position = [100, 200]

pos_x = position[0]
pos_y = position[1]

rect(pos_x, pos_y, 20, 20)
```

#### Beispiel 1: Listen in der Liste.

In diesem Beispiel sollen auf zwei Seiten jeweils an den gleichen Positionen unterschiedliche Formen gezeichnet werden, z.B. um Vorlagen für zwei Siebdruck-Siebe zu erhalten.

```
from random import randrange
newPage(400, 600)

# eine leere Liste
pos = []

for i in range(10):
    # drei Zufallswerte
    x = randrange(width())
    y = randrange(height())
    d = randrange(50)
    # eine Liste davon
    new_pos = [x, y, d]
    # kleine Liste in grosse Liste
    pos.append(new_pos)

print pos

for p in pos:
    pos_x = p[0]
    pos_y = p[1]
    side = p[2]
    rect(pos_x, pos_y, side, side)

newPage(400, 600)
for p in pos:
    pos_x = p[0]
    pos_y = p[1]
    side = p[2]
    oval(pos_x, pos_y, side, side)

saveImage('~/Desktop/riso.pdf')
```

#### Beispiel 2: Farbwerte in DrawBot in Schritten von 0–255.

```
hues = []
frac = 1/256

for i in range(256):
    hue = i * frac
    hues.append(hue)

print hues

# Eine Liste mit 256 regelmässigen Schritten von 0–1
# 0, 0.00392156862745, … , 0.996078431373, 1
# Jetzt lassen sich Farbmischungen wie in InDesign etc. definieren

r = hues[160]
g = hues[10]
b = hues[255]

fill(r, g, b)
rect(0, 0, width(), height())
```

[Mehr zu Listen](https://docs.python.org/2/tutorial/datastructures.html)
[Noch mehr zu Listen](http://www.effbot.org/zone/python-list.htm)

## Kapitel 2: Funktionen

Selbstgeschriebene *Funktionen* können sehr hilfreich sein, und es wäre fahrlässig, Euch ohne dieses Werkzeug aus diesem Kurs zu entlassen.
Es ist keine grosse Sache, solange wir nicht zu tief eintauchen.

Was wir bis jetzt wissen: *Funktionen* sind daran zu erkennen, dass sie am Ende Klammern aufweisen – mal mit, mal ohne *Argumente*.

Einige *Funktionen* haben Auswirkungen, z.B. ändert `fill()` die Farbe. Andere liefern bestimmte Werte, wie `width()` und `height()`.

Bisher waren wir auf bestehende *Funktionen* angewiesen. Jetzt schauen wir uns an, wie wir selber *Funktionen* schreiben können.

#### Definieren von Funktionen, Syntax

Funktionen werden mit dem *Schlüsselwort* `def` eingeleitet, darauf folgt der Funktionsname mit Klammer und *Argumenten* (optional) und schliesslich ein Doppelpunkt. Die Zeilen nach dem Doppelpunkt werden mit einem Tabulator eingezogen – sie werden ausgeführt, wenn die Funktion aufgerufen wird.

Die Definition der Funktion stellt einen Code-Baustein bereit, der später an beliebiger Stelle verwendet werden kann.


```
def funktionsname(argumente):
	# etwas passiert
```

#### Einige Beispiele …

```
# Funktion ohne Argument
def xyz():
    print 'xyz'

# Aufruf der Funktion
xyz()
```

Das ist nicht besonders interessant. Aber immerhin gibt es eine Auswirkung – es wird «xyz» geschrieben.

```
# Funktion mit Argument
def sagmal(etwas):
    print etwas

sagmal('Hoi')
```

Das ist etwas besser: Die Funktion akzeptiert ein Argument und tut etwas damit.

Wir können prüfen, ob das Argument zu dem passt, was wir mit der Funktion anstellen wollen.

```
# Funktion mit Fehlermeldung
def sagmal(etwas):
    # ist das Argument eine Zeichenfolge?
    if type(etwas) is str:
        print etwas
    else:
        # Fehlermeldungen in Konsole:
        raise IOError('Schwaches Argument')

sagmal('top!')
sagmal(7)
```

#### Return

Für uns nützlicher als das Schreiben in die Konsole ist das «Liefern» von Werten. Dafür gibt es das Schlüsselwort `return`.
Das englische ‘return’ wird in diesem Zusammenhang meist mit «zurückgeben» übersetzt – etwas hölzern, aber so gehts halt in Zeiten des angelsächsischen Kulturimperialismus.

```
def multDiv(wert, faktor, divisor):
	return wert * faktor / divisor

multDiv(3, 4, 5)
```

Puh, da passiert ja gar nichts.

```
def multDiv(wert, faktor, divisor):
	return wert * faktor / divisor

print multDiv(3, 4, 5)
```

`Return` gibt einen Wert zurück, aber man sieht keine Auswirkungen. Die Funktion wird, wenn sie ausgeführt wird, durch den Wert ersetzt. Diesen kann man mit `print` in die Konsole schreiben oder in eine Variable speichern.

### Wann braucht man selbstgebastelte Funktionen?

Hier ein DrawBot-spezifisches Problem, das den Nutzen von selbstgeschriebenen Funktionen illustriert: Manchmal möchte ich Formen ausgehend von ihrem Mittelpunkt zeichen. Dafür muss ich jedes Mal die Hälfte des Durchmessers von den Koordinaten subtrahieren. Das macht den Code unleserlich und ist mühselig.

```
# Kreis aus der Mitte
def circle(x, y, rad):
    oval(x-rad, y-rad, 2*rad, 2*rad)

# Quadrat aus der Mitte
def square(x, y, diameter):
    rect(x-diameter/2, y-diameter/2, diameter, diameter)

# gedrehtes Quadrat aus der Mitte
def diamond(x, y, diameter):
    save()
    translate(x, y)
    rotate(45) # könnte auch per Argument definiert werden 
    rect(-diameter/2, -diameter/2, diameter, diameter)
    restore()

circle(100, 100, 50)
square(100, 100, 50)
diamond(100, 100, 100)
```

Das auslagern komplizierter Abschnitte eines Scripts in Funktionen kann die Lesbarkeit stark verbessern. «drawRandomShape(x, y, w, h)» ist auf Anhieb verständlich – eine mehrzeilige Schleife mit translate, newPath, moveTo, lineTo, closePath, drawPath muss hingegen zuerst einmal entziffert werden.

[Details zu Funktionen in der Python-Dokumentation](https://docs.python.org/2/tutorial/controlflow.html#defining-functions)

## Kapitel 3: Schreiben mit DrawBot

Siehe [DrawBot-Dokumentation zum Thema Text](http://www.drawbot.com/content/text.html)

In DrawBot gibt es einige Funktionen, die zum Schreiben dienen. Etwas präziser: Schreiben von Text auf eine Seite mit Fonts.

Die Funktion `font()` lädt einen bestimmten Font. Als Argument wird der *PostScript-Name* des Fonts angegeben. Das ist in der Regel der Dateiname ohne die Endung. Als optionales zweites Argument kann eine Schriftgrösse angegeben werden.

Tipp: Der *PostScript-Name* lässt sich in Applikationen wie der Schriftsammlung oder dem FontExplorer anzeigen. Meist ist es der Dateiname des Fonts ohne Endung.

Die Funktion `text()` schreibt einen *String* auf die Seite. Es gibt zwei Argumente: Den String, und dann eine Klammer mit Koordinaten.


```
font('ComicSansMS')
text('Ja, hallihalllooooo!', (100, 100))
```

### Text-Eigenschaften

```
fontSize(30)	# Schriftgrösse
lineHeight(100)	# Zeilenabstand
tracking(0.5)	# Zeichenabstand
```

[Weitere Texteigenschaften in der DrawBot-Dokumentation](http://www.drawbot.com/content/text/textProperties.html)

### Alle installierten Fonts

Die Funktion `installedFonts()` liefert eine Liste aller installierten (aktivierten) Fonts.

```
fonts = installedFonts()

for font in fonts:
    print font
```

## KAPITEL 1000

### Was wir nicht behandelt haben, aber auch noch nützlich wäre

Irgendwann muss auch mal gut sein. Aber folgende Themen könntest du dir mal zu Gemüte führen, wenn dir danach ist.

- while – eine andere Art Schleife als der for-Loop. Gefährlich.
- break – ein Schlüsselwort, das Loops und Funktionen unterbricht.
- pass – ein Schlüsselwort, das nichts tut.
- tuple – eine spezielle Listenform (Werte nicht manipulierbar)
- dictionary – eine Liste mit Schlüssel/Wert-Paaren (heisst in JavaScript *Objekt*)
- Klassen, Methoden, objektorientiertes Programmieren (OOP) – wie Funktionen eine extrem nützliche Angelegenheit. Hier tut sich eine Welt auf.

---

[Tag 1](README.md)
[Tag 2](tag2.md)

---

![Creatice Commons License](https://i.creativecommons.org/l/by-sa/4.0/80x15.png)

Dieser Text kann unter den Bedingungen der [«CC BY-SA» Creative Commons Lizenz](https://creativecommons.org/licenses/by-sa/4.0/legalcode.de) weiterverwendet werden.
