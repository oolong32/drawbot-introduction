# Programmieren für Designer — Python und DrawBot, Tag 2

# Was bisher geschah

Wir haben am ersten Abend gelernt:

-  Benutzung der Python-Konsole im Terminal
-  mathematische Operationen: +-\*/%
-  Wertzuweisung an Variable: x = n
-  Werte: integer, float, string, list
-  das Statement `print`
-  Schleifen: `for var in list:`
-  importieren aus Modulen: `from random import randrange`
-  Zufallszahlen: `random()`, `randrange()`, `uniform()`
-  Kommentare: #
-  Funktionen und Argumente: `function(arg)`
-  Grundlegende DrawBot-Funktionen: `newPage()`, `width()`, `height()`, `oval()`, `fill()`, `stroke()`, `saveImage()`
-  Fehlermeldungen
-  Grundlegende Python-Funktionen: `range()`, `min()`, `max()`

## Themen heute

-  Raster generieren mit Schleifen
-  weitere DrawBot-Funktionen: `translate()`, `rotate()`, `save()`, `restore()`
-  Bedingungen: if/else

## Zum Aufwärmen

1. Zeichne einen Kreis mit variablem Duchmesser in die Mitte der Seite.
2. Unterteile die Seite in 10 Spalten. Zeichne in jede Spalte einen Kreis.

## Raster

Raster dienen dazu, eine Seite in Zellen zu unterteilen. Es gibt zwei Möglichkeiten, das zu tun:

-  Den Raster über die einzelne Zelle definieren
-  Den Raster über die Anzahl Spalten und Zeilen definieren

Wir schauen uns nur an, wie wir über eine Zelle zu einem Raster kommen. Die zweite Variante lässt sich vergleichsweise leicht davon ableiten.

### Raster nach Zelle

Wir gehen zuerst von einer quadratischen Zelle aus, damit wir für Höhe und Breite mit dem gleichen Wert arbeiten können.

Wir müssen herausfinden:

-  wieviele Zellen auf eine Seite passen
-  wieviele Zellen auf eine Zeile gehen
-  wieviele Zellen in eine Spalte gehen


    newPage(300, 400)

    cell = 24

    cols = width() / cell
    rows = height() / cell

    print rows, 'Zeilen und', cols, 'Spalten'

    # 16.667 Zeilen und 12.5 Spalten

Resultate im Float-Format können wir nicht so gut brauchen, weil wir

-  keine angeschnittenen Zellen möchten.
-  die Variablen cols und rows als Argumente für range() benötigen.

Mit der Funktion `int()` werden aus Float-Zahlen Integer-Zahlen.

    cols = int(width() / cell)
    rows = int(height() / cell)

    print rows, 'Zeilen und', cols, 'Spalten'

# 16 Zeilen und 12 Spalten

Mit den Variablen cols und rows können wir Loops schreiben:

    fill(None)
    stroke(0)

    for row in range(rows):
        rect(0, row * cell, cell, cell)

    for col in range(cols):
        rect(col * cell, 0, cell, cell)

Jeder dieser Loops zeichnet eine Spalte, respektive eine Zeile mit Zellen. Jetzt müssten wir noch die Seite füllen.

    for row in range(rows):
        for col in range(cols):
            rect(col * cell, row * cell, cell, cell)

Jetzt haben wir einen Raster. Schön wäre, wenn er eingemittet wäre. Dazu müssen wir herausfinden, wie viel Platz oben und rechts übrig bleibt.

    offset_x = (width() % cell) / 2
    offset_y = (height() % cell) / 2

Mit dem Prozent-Zeichen (*modulo*) kriegt man den Rest einer Division. Der Rest der Division von `width()` und `height()` durch cell entspricht dem weissen Rand rechts und oben unseres Rasters.

Die Hälfte dieser beiden Reste ergibt die Distanz, um welche wir alle Zellen einrücken müssen, um den Raster auf der Seite einzumitten.

    for row in range(rows):
        for col in range(cols):
            rect(col * cell + offset_x, row * cell + offset_y, cell, cell)

Das gibt eine lange, schwer leserliche Zeile. Und wenn wir mehr als ein Rechteck zeichnen müssten, müssten die Positionswerte für alles andere ebenfalls angepasst werden.

Gut wäre darum eine Möglichkeit, das Einrücken unabhängig vom Zeichnen zu behandeln.

### translate()

Die DrawBot-Funktion `translate()` dient dazu, den Nullpunkt des Koordinatensystems auf der Seite zu verschieben.

#### Nullpunkt auf Seite zentrieren

    translate(width()/2, height()/2)
    oval(-50, -50, 100, 100)

Für unser Raster-Script bedeutet dies eine Vereinfachung: Zu Beginn rücken wir den Nullpunkt etwas ein und zeichnen dann den Raster wie gehabt. (Vorher musste jedes gezeichnete Element separat eingerückt werden.)

    newPage(300, 400)
    fill(None)
    stroke(0)

    cell = 24
    cols = int(width() / cell)
    rows = int(height() / cell)

    offset_x = (width() % cell) / 2
    offset_y = (height() % cell) / 2

    translate(offset_x, offset_y)

    for row in range(rows):
        for col in range(cols):
            rect(col * cell, row * cell, cell, cell)


Es drängt sich die Idee auf, `translate()` für jede einzelne Zelle zu verwenden.

    for row in range(rows):
        for col in range(cols):
            translate(col * cell, row * cell)
            rect(0, 0, cell, cell)

Oje, oje! Was ist passiert?

Das Problem ist, dass `translate()` den Nullpunkt jedes Mal ab dem letzten bekannten Nullpunkt verschiebt. Dadurch geraten die Zellen, die von der Schleife gezeichnet werden, schon auf der ersten Zeile in einen Bereich, der ausserhalb der Seite liegt.

### save() und restore()

Mit `save()` und `restore()` lassen sich Umgebungsvariablen wie Nullpunkt, Farbe und Strichstärke speichern und wieder zurücksetzen.

    for row in range(rows):
        for col in range(cols):
            save()
            translate(col * cell, row * cell)
            rect(0, 0, cell, cell)
            restore()

Mit `save()` wird der Nullpunkt gespeichert, und mit `restore()` wieder auf den gespeicherten Wert zurückgesetzt.

### Warum save(), translate() und restore()?

Um in die Zellen des Rasters zu zeichnen, braucht es nur noch  Werte zwischen 0, 0 und cell, cell. Dadurch werden Funktionen wie `rect()` und `oval()` viel leichter lesbar.

### Aufgaben

-  Raster zeichnen
-  Jede Zelle mit zufälligen Werten leicht verschieben
-  Statt Rechtecke in jede Zelle einen Kreis zufälliger Grösse zeichnen
-  Mehr als eine Form pro Zelle zeichnen

## Bedingungen

Ein fundamentales Element von Programmiersprachen ist die Möglichkeit, Teile eines Programms von Bedingungen abhängig zu machen.

Um die Grundlage für eine „Bedingung“ zu verstehen, kann man in der Python-Konsole (im Terminal) Folgendes versuchen:

    >>> 5 > 4
    True
    >>> 5 < 4
    False
    >>> 1 == 1
    True
    >>> 2 != 1
    True

Es stimmt, dass fünf grösser als vier ist. Es stimmt nicht, dass fünf kleiner als vier ist. Es stimmt, dass eins gleich eins ist. Es stimmt, dass zwei nicht gleich eins ist.

*True* und *False* sind sogenannte *Boolsche Werte* (‘boolean values’). Man kann sie sich wie zwei Zustände eines Kippschalters vorstellen.

### if 

Ein sogenanntes *if-statement* führt einen Abschnitt eines Programms nur aus, wenn eine Bedingung *True* ist.

    >>> if True:
    …	print 'Hoi.'
    …
    Hoi.
    >>> if False:
    …	print 'Ciao.'
    …
    # gar nix

Die beiden Beispiele oben zeigen zwei bereits evaluierte Bedingungen: Nur wenn *True* auf *if* folgt, wird der darunter eingerückte Code-Block ausgeführt.

    if 5 > 4:
    …	print 'war ja klar.'
    …
    war ja klar.

Python evaluiert die Bedingung (zwischen *if* und Doppelpunkt). Wenn dies *True* ergibt, wird der eingerückte Teil unter dem *if* Statement ausgeführt. Ergibt die Evaluation *False*, wird der eingerückte Teil ausgelassen.

### Operatoren zum formulieren einer Bedingung

Zum Formulieren einer Bedingung können folgende *Operatoren* verwendet werden:

    <	kleiner	 
    <=	kleiner oder gleich	 
    >	grösser	 
    >=	grösser oder gleich	 
    ==	gleich
    !=	nicht gleich

### else

Mit *else* lässt sich ein Abschnitt definieren, der ausgeführt wird, wenn die Bedingung für das *if-Statement* als *False* evaluiert wird.

    from random import random
    >>> if random() > 0.5:
    …	print 'Kopf'
    … else:
    …	print 'Zahl'
    …
    # Kopf oder Zahl?

### if/else in DrawBot

In DrawBot: Je nach Wert einer zufällig generierten Zahl wird ein Rechteck oder ein Kreis gezeichnet.

    from random import random
    r = random()

    if r > 0.5:
        line((0, 0), (cell, cell))
    else:
        line((0, cell), (cell, 0))

Wie bei *loops* muss auf ein *if/else Statement* ein eingerückter Block folgen.

### Kombinierte Bedingungen

    x and y # x und y müssen True sein
    x or y  # x oder y muss True sein

Mit *and/or* lassen sich mehrere Bedingungen kombinieren. In der Python-Konsole (Terminal):

    >>> 2 != 1 or 10 == 10
    >>> 1 == 1 and 2 == 2

In DrawBot (Fortsetzung von «if/else in DrawBot»):

    if r > 0.6:
        rect(0, 0, 200, 200)
    elif r <= 0.6 and r > 0.3:
        oval(0, 0, 200, 200)
    else:
        stroke(0)
        line((0, 0), (200, 200))

Das Statement für weitere Bedingungen zwischen *if* und *else* lautet *elif*.

Eine [Übersicht über die möglichen *Operatoren*, die zum Formulieren von Bedingungen verwendet werden können.](https://docs.python.org/2/reference/expressions.html#not-in)

## Speichern mit Zeitstempel

Damit gespeicherter Output nicht jedes Mal überschrieben wird, wenn das Programm ausgeführt wird, kann man einen sog. *Zeitstempel* in den Dateinamen schreiben.

    from time import strftime

    timestr = strftime('%Y%m%d-%H%M%S')
    saveImage('~/Desktop/name_' + timestr + '.jpg')

---

[Tag 1](README.md)

---

![Creatice Commons License](https://i.creativecommons.org/l/by-sa/4.0/80x15.png)

Dieser Text kann unter den Bedingungen der [«CC BY-SA» Creative Commons Lizenz](https://creativecommons.org/licenses/by-sa/4.0/legalcode.de) weiterverwendet werden.
