Dateien
=======

Öffnen von Dateien
------------------

In Python öffnet und lest ihr eine Datei, indem ihr die eingebaute Funktion
:func:`python3:open` und verschiedene eingebaute Leseoperationen verwendet. Das
folgende kurze Python-Programm liest eine Zeile aus einer Textdatei namens
:samp:`{myfile}` ein:

.. code-block:: python

    >>> f = open('docs/types/myfile', 'r')
    >>> line = f.readline()

:func:`python3:open` liest nichts aus der Datei, sondern gibt ein :abbr:`sog.
(sogenanntes)` Datei-Objekt zurück, mit dem ihr auf die geöffnete Datei
zugreifen könnt. Es behält den Überblick über eine Datei und darüber, wie viel
von der Datei gelesen oder geschrieben wurde. Alle Dateieingaben in Python
werden mit Dateiobjekten und nicht mit Dateinamen durchgeführt.

Der erste Aufruf von :mod:`python3:readline` gibt die erste Zeile des
Datei-Objekts zurück, also alles bis einschließlich des ersten Zeilenumbruchs
oder die gesamte Datei, wenn es keinen Zeilenumbruch in der Datei gibt; der
nächste Aufruf von ``readline`` gibt die zweite Zeile zurück, wenn sie
existiert, :abbr:`usw (und so weiter)`.

Das erste Argument der Funktion ``open`` ist ein Pfadname. Im vorigen Beispiel
öffnet ihr eine Datei, von der ihr annehmt, dass sie sich im aktuellen
Arbeitsverzeichnis befindet. Das folgende Beispiel öffnet eine Datei an einem
absoluten Speicherort – :samp:`{C:\Meine Dokumente\\myfile}`:

.. code-block:: python

    >>> import os
    >>> pathname = os.path.join('C:', 'Users', 'Veit', 'Documents', 'myfile')
    >>> with open(pathname, 'r') as f:
    ...     line = f.readline()

.. note::

    In diesem Beispiel wird das Schlüsselwort ``with`` verwendet, :abbr:`d.h.
    (das heißt)`, dass die Datei mit einem Kontextmanager geöffnet wird, der
    in :doc:`/control-flows/with` näher erläutert wird. Diese Art des Öffnens
    von Dateien verwaltet mögliche I/O-Fehler besser und sollte im Allgemeinen
    bevorzugt werden.

Schließen von Dateien
---------------------

Nachdem alle Daten aus einem Datei-Objekt gelesen oder in dieses geschrieben
wurden, sollte das Datei-Pbjekt wieder geschlossen werden damit Systemressourcen
freigegeben werden, das Lesen oder Schreiben der zugrunde liegenden Datei durch
anderen Code ermöglicht wird und das Programm insgesamt zuverlässiger wird. Bei
kleinen Skripten hat dies in der Regel keine großen Auswirkungen, da
Dateiobjekte werden automatisch geschlossen, wenn das Skript oder Programm
beendet wird. Bei größeren Programmen können zu viele offene Datei-Objekte
jedoch die Systemressourcen erschöpfen, was zum Abbruch des Programms führen.
Ihr schließt ein Dateiobjekt mit der ``close``-Methode, wenn das Datei-Objekt
nicht mehr benötigt wird:

.. code-block:: python

    >>> f = open('docs/types/myfile', 'r')
    >>> line = f.readline()
    >>> f.close()

Die Verwendung eines :doc:`/control-flows/with` bleibt meist jedoch die bessere
Möglichkeit, um Dateien automatisch zu schließen, wenn ihr fertig seid:

.. code-block:: python

    >>> with open('docs/types/myfile', 'r') as f:
    ...     line = f.readline()

Öffnen von Dateien im Schreib- oder anderen Modi
------------------------------------------------

Das zweite Argument des Befehls :func:`python3:open` ist eine Zeichenkette, die
angibt, wie die Datei geöffnet werden soll. ``'r'`` öffnet die Datei zum Lesen
(engl. *read*), ``'w'`` öffnet die Datei zum Schreiben (engl. *write*) und
``'a'`` offnet die Datei zum Anhängen (engl. *attach*). Wenn ihr die Datei zum
Lesen öffnen wollen, könnt ihr das zweite Argument weglassen, da ``'r'`` der
Standardwert ist. Das folgende kurze Programm schreibt :samp:`Hi, Pythonistas!`
in eine Datei:

.. code-block:: python

    >>> f = open('docs/types/myfile', 'w')
    >>> f.write('Hi, Pythonistas!\n')
    18
    >>> f.close()

Je nach Betriebssystem kann :func:`python3:open` auch Zugang zu weiteren
Dateimodi haben. Diese Modi sind jedoch für die meisten Zwecke nicht notwendig.

``open`` kann ein optionales drittes Argument annehmen, das definiert, wie Lese-
oder Schreibvorgänge für diese Datei gepuffert werden. Beim Puffern werden Daten
so lange im Speicher gehalten, bis genügend Daten angefordert oder geschrieben
wurden, um die Zeitaufwände für einen Plattenzugriff zu rechtfertigen. Andere
Parameter für ``open`` steuern die Kodierung für Textdateien und die Behandlung
von Zeilenumbrüchen in Textdateien. Auch hier gilt, dass ihr euch in der Regel
keine Gedanken über diese Funktionen machen müsst, aber wenn ihr mit Python
fortgeschrittener werdet, solltet ihr euch vielleicht darüber informieren.

Lese- und Schreib-Funktionen 
----------------------------

Die häufigste Funktion zum Lesen von Textdateien, :mod:`python3:readline`, habe
ich bereits vorgestellt. Diese Funktion liest eine einzelne Zeile aus einem
Datei-Objekt und gibt sie zurück, einschließlich aller Zeilenumbrüche am Ende
der Zeile. Wenn es nichts mehr zu lesen gibt, gibt readline einen leeren String
zurück, was es einfach macht, :abbr:`z.B. (zum Beispiel)` die Anzahl der Zeilen
in einer Datei zu ermitteln:

.. code-block:: python

    >>> f = open('docs/types/myfile', 'r')
    >>> lc = 0
    >>> while f.readline() != '':
    ...     lc = lc + 1
    ... 
    >>> print(lc)
    2
    >>> f.close()

Ein kürzerer Weg, alle Zeilen zu zählen, gibt es mit der ebenfalls eingebauten
``readlines``-Methode, die alle Zeilen einer Datei liest und sie als Liste von
Strings mit einen String pro Zeile zurückgibt:

.. code-block:: python

    >>> f = open('docs/types/myfile', 'r')
    >>> print(len(f.readlines()))
    2
    >>> f.close()

Wenn ihr alle Zeilen einer großen Datei zählt, kann diese Methode dazu führen,
dass der Speicher vollläuft, weil die gesamte Datei auf einmal geliesen wird. Es
ist auch möglich, dass der Speicher mit :mod:`python3:readline` überläuft, wenn
ihr versucht, eine Zeile aus einer großen Datei zu lesen, die keine
Zeilenumbruchzeichen enthältist. Um mit solchen Situationen besser umgehen zu
können, haben beide Methoden ein optionales Argument, das die Menge der zu einem
Zeitpunkt gelesenen Daten beeinflusst. Eine andere Möglichkeit, über alle Zeilen
einer Datei zu iterieren, besteht darin, das Dateiobjekt als Iterator in einer
:ref:`for-loop` zu behandeln:

.. code-block:: python

    >>> f = open('docs/types/myfile', 'r')
    >>> lc = 0
    >>> for l in f:
    ...     lc = lc + 1
    ... 
    >>> print(lc)
    2
    >>> f.close()

Diese Methode hat den Vorteil, dass die Zeilen je nach Bedarf in den Speicher
eingelesen werden, so dass selbst bei großen Dateien kein Speicherplatzmangel zu
befürchten ist. Der andere Vorteil dieser Methode ist, dass sie einfacher und
lesbarer ist.

Ein mögliches Problem mit der Lesemethode kann jedoch entstehen, wenn auf
Windows- und macOS Übersetzungen im Textmodus erfolgen, wenn ihr den Befehl
:func:`open` im Textmodus verwenden, :abbr:`d.h. (das heißt)` ohne ein ``b``
anzuhängen. Im Textmodus wird auf macOS jedes ``\r`` in ``\n`` umgewandelt,
während unter Windows ``\r\n``-Paare in ``\n`` umgewandelt werden. Ihr könnt die
Behandlung von Zeilenumbrüchen festlegen, indem ihr beim Öffnen der Datei den
Parameter ``newline`` verwendet und ``newline='\n'``, ``\r`` oder ``\r\n``
angebt, wodurch nur diese Zeichenfolge als Zeilenumbruch verwendet wird:

.. code-block:: python

    >>> f = open('docs/types/myfile', newline='\n')

In diesem Beispiel wird nur ``\n`` als Zeilenumbruch gewertet. Wenn die Datei
jedoch im Binärmodus geöffnet wurde, ist der Parameter ``newline`` nicht
erforderlich, da alle Bytes genau so zurückgegeben werden, wie sie in der Datei
stehen.

Die Schreibmethoden, die den Methoden ``readline`` und ``readlines``
entsprechen, sind ``write`` und ``writelines``. Beachtet, dass es keine
``writeline``-Funktion gibt. ``write`` schreibt eine einzelne Zeichenkette, die
sich über mehrere Zeilen erstrecken kann, wenn Zeilenumbruchzeichen in die
Zeichenkette eingebettet sind, wie im folgenden Beispiel:

.. code-block:: python

    f.write('Hi, Pythinistas!\n\n')

Die Methode ``writelines`` ist jedoch verwirrend, weil sie nicht unbedingt
mehrere Zeilen schreibt; sie nimmt eine Liste von Zeichenketten als Argument und
schreibt sie nacheinander in das angegebene Datei-Objekt, ohne Zeilenumbrüche
zwischen den Listenelementen einzufügen; nur wenn die Zeichenketten in der Liste
Zeilenumbrüchen enthalten, kommen Zeilenumbrüche im Datei-Objekt hinzu;
andernfalls werden sie aneinandergereiht. ``writelines`` ist damit die genaue
Umkehrung von ``readlines``, da sie auf die von ``readlines`` zurückgegebene
Liste angewendet werden kann, um eine Datei zu schreiben, die identisch mit der Ausgangsdatei ist. Unter der Annahme, dass myfile.txt existiert und eine
Textdatei ist, erzeugt das folgende Beispiel eine exakte Kopie von
:file:`myfile` mit dem Namen :file:`myfile2`:

Verwendung des Binärmodus
~~~~~~~~~~~~~~~~~~~~~~~~~

Wenn ihr alle Daten in einer Datei in ein einziges Byte-Objekt (partiell)
einlesen und in den Speicher übertragen möchtet um sie als Byte-Sequenz
behandeln zu können, könnt ihr die ``read``-Methode verwenden. Ohne ein Argument
liest sie die gesamte Datei ab der aktuellen Position ein und gibt die Daten als
Bytes-Objekt zurück. Mit einem ganzzahligen Argument liest sie maximal diese
Anzahl von Bytes und gibt ein Bytes-Objekt der angegebenen Größe zurück:

.. code-block:: python
    :linenos:

    >>> f = open('myfile', 'rb')
    >>> head = f.read(16)
    >>> print(head)
    b'Hi, Pythonistas!'
    >>> body = f.read()
    >>> print(body)
    b'\n\n'
    >>> f.close()

Zeile 1
    öffnet eine Datei zum Lesen im Binärmodus
Zeile 2
    liest die ersten 16 Bytes als ``head``-String
Zeile 3
    gibt den ``head``-String aus
Zeile 5
    liest den Rest der Datei

.. note::

   Dateien, die im Binärmodus geöffnet werden, arbeiten nur mit Bytes und nicht
   mit Zeichenketten. Um die Daten als Zeichenketten zu verwenden, müsst ihr
   alle Byte-Objekte in String-Objekte dekodieren. Dieser Punkt ist oft wichtig
   im Umgang mit Netzwerkprotokollen, wo sich Datenströme oft wie Dateien
   verhalten, aber als Bytes und nicht als Strings interpretiert werden müssen.

Es gibt noch mehrere andere Ein- und Ausgabemöglichkeiten:

:doc:`fileinput <python3:library/fileinput>`
    erlaubt euch, schnell eine Schleife über die Standardeingabe oder eine Liste
    von Dateien zu schreiben. 
:doc:`sys <python3:library/sys>`
    ermöglicht den Zugriff auf ``stdin``, ``stdout`` und ``stderr``.
:doc:`struct <python3:library/struct>`
    bietet Unterstützung für das Lesen und Schreiben von Dateien, die von
    C-Programmen erzeugt wurden oder von diesen verwendet werden sollen.
:doc:`pickle <python3:library/pickle>`
    persistiert Python-Datentypen, :abbr:`s.a. (siehe auch)`
    :doc:`../save-data/pickle`
