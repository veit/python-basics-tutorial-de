Dateien
=======

Öffnen von Dateien
------------------

In Python öffnet und lest ihr eine Datei, indem ihr die eingebaute Funktion
:func:`python3:open` und verschiedene eingebaute Leseoperationen verwendet. Das
folgende kurze Python-Programm liest eine Zeile aus einer Textdatei namens
:samp:`{myfile.txt}` ein:

.. code-block:: pycon

    >>> f = open("docs/types/myfile.txt", "r")
    >>> line = f.readline()

:func:`python3:open` liest nichts aus der Datei, sondern gibt ein :abbr:`sog.
(sogenanntes)` Datei-Objekt zurück, mit dem ihr auf die geöffnete Datei
zugreifen könnt. Es behält den Überblick über eine Datei und darüber, wie viel
von der Datei gelesen oder geschrieben wurde. Alle Dateieingaben in Python
werden mit Dateiobjekten und nicht mit Dateinamen durchgeführt.

Der erste Aufruf von :meth:`readline() <codecs.StreamReader.readline>` gibt die
erste Zeile des Datei-Objekts zurück, also alles bis einschließlich des ersten
Zeilenumbruchs oder die gesamte Datei, wenn es keinen Zeilenumbruch in der Datei
gibt; der nächste Aufruf von ``readline`` gibt die zweite Zeile zurück, wenn sie
existiert, :abbr:`usw (und so weiter)`.

Das erste Argument der Funktion ``open`` ist ein Pfadname. Im vorigen Beispiel
öffnet ihr eine Datei, von der ihr annehmt, dass sie sich im aktuellen
Arbeitsverzeichnis befindet. Das folgende Beispiel öffnet eine Datei an einem
absoluten Speicherort – :samp:`{C:\Meine Dokumente\\myfile.txt}`:

.. code-block:: pycon

    >>> import os
    >>> pathname = os.path.join("C:/", "Users", "Veit", "Documents", "myfile.txt")
    >>> with open(pathname, "r") as f:
    ...     line = f.readline()
    ...

.. note::

    In diesem Beispiel wird das Schlüsselwort ``with`` verwendet, :abbr:`d.h.
    (das heißt)`, dass die Datei mit einem Kontextmanager geöffnet wird, der
    in :doc:`/control-flow/with` näher erläutert wird. Diese Art des Öffnens
    von Dateien verwaltet mögliche I/O-Fehler besser und sollte im Allgemeinen
    bevorzugt werden.

Schließen von Dateien
---------------------

Nachdem alle Daten aus einem Datei-Objekt gelesen oder in dieses geschrieben
wurden, sollte das Datei-Objekt wieder geschlossen werden damit Systemressourcen
freigegeben werden, das Lesen oder Schreiben der zugrunde liegenden Datei durch
anderen Code ermöglicht wird und das Programm insgesamt zuverlässiger wird. Bei
kleinen Skripten hat dies in der Regel keine großen Auswirkungen, da
Dateiobjekte automatisch geschlossen werden, wenn das Skript oder Programm
beendet wird. Bei größeren Programmen können zu viele offene Datei-Objekte
jedoch die Systemressourcen erschöpfen, was zum Abbruch des Programms führt.
Ihr schließt ein Dateiobjekt mit der ``close``-Methode, wenn das Datei-Objekt
nicht mehr benötigt wird:

.. code-block:: pycon

    >>> f = open("docs/types/myfile.txt", "r")
    >>> line = f.readline()
    >>> f.close()

Die Verwendung eines :doc:`/control-flow/with` bleibt meist jedoch die bessere
Möglichkeit, um Dateien automatisch zu schließen, wenn ihr fertig seid:

.. code-block:: pycon

    >>> with open("docs/types/myfile.txt", "r") as f:
    ...     line = f.readline()
    ...

Öffnen von Dateien im Schreib- oder anderen Modi
------------------------------------------------

Das zweite Argument des Befehls :func:`python3:open` ist eine Zeichenkette, die
angibt, wie die Datei geöffnet werden soll. ``"r"`` öffnet die Datei zum Lesen
(engl. *read*), ``"w"`` öffnet die Datei zum Schreiben (engl. *write*) und
``"a"`` öffnet die Datei zum Anhängen (engl. *attach*). Wenn ihr die Datei zum
Lesen öffnen wollen, könnt ihr das zweite Argument weglassen, da ``"r"`` der
Standardwert ist. Das folgende kurze Programm schreibt :samp:`Hi, Pythonistas!`
in eine Datei:

.. code-block:: pycon

    >>> f = open("docs/types/myfile.txt", "w")
    >>> f.write("Hi, Pythonistas!\n")
    17
    >>> f.close()

Je nach Betriebssystem kann :func:`python3:open` auch Zugang zu weiteren
Dateimodi haben. Diese Modi sind jedoch für die meisten Zwecke nicht notwendig.

``open`` kann ein optionales drittes Argument annehmen, das definiert, wie
Lese- oder Schreibvorgänge für diese Datei gepuffert werden. Beim Puffern werden
Daten so lange im Speicher gehalten, bis genügend Daten angefordert oder
geschrieben wurden, um die Zeitaufwände für einen Plattenzugriff zu
rechtfertigen. Andere Parameter für ``open`` steuern die Kodierung für
Textdateien und die Behandlung von Zeilenumbrüchen in Textdateien. Auch hier
gilt, dass ihr euch in der Regel keine Gedanken über diese Funktionen machen
müsst, aber wenn ihr mit Python fortgeschrittener werdet, solltet ihr euch
vielleicht darüber informieren.

Lese- und Schreib-Funktionen
----------------------------

``readline``
~~~~~~~~~~~~

Die häufigste Funktion zum Lesen von Textdateien, :meth:`readline()
<codecs.StreamReader.readline>`, habe ich bereits vorgestellt. Diese Funktion
liest eine einzelne Zeile aus einem Datei-Objekt und gibt sie zurück,
einschließlich aller Zeilenumbrüche am Ende der Zeile. Wenn es nichts mehr zu
lesen gibt, gibt readline einen leeren String zurück, was es einfach macht,
:abbr:`z.B. (zum Beispiel)` die Anzahl der Zeilen in einer Datei zu ermitteln:

.. code-block:: pycon

    >>> f = open("docs/types/myfile.txt", "r")
    >>> lc = 0
    >>> while f.readline() != "":
    ...     lc = lc + 1
    ...
    >>> print(lc)
    2
    >>> f.close()

``readlines``
~~~~~~~~~~~~~

Ein kürzerer Weg, alle Zeilen zu zählen, gibt es mit der ebenfalls eingebauten
:meth:`readlines() <codecs.StreamReader.readlines>`-Methode, die alle Zeilen
einer Datei liest und sie als Liste von Strings mit einen String pro Zeile
zurückgibt:

.. code-block:: pycon

    >>> f = open("docs/types/myfile.txt", "r")
    >>> print(len(f.readlines()))
    1
    >>> f.close()

Wenn ihr alle Zeilen einer großen Datei zählt, kann diese Methode dazu führen,
dass der Speicher vollläuft, weil die gesamte Datei auf einmal gelesen wird. Es
ist auch möglich, dass der Speicher mit :meth:`readline()
<codecs.StreamReader.readline>` überläuft, wenn ihr versucht, eine Zeile aus
einer großen Datei zu lesen, die keine Zeilenumbruchzeichen enthält. Um mit
solchen Situationen besser umgehen zu können, haben beide Methoden ein
optionales Argument, das die Menge der zu einem Zeitpunkt gelesenen Daten
beeinflusst. Eine andere Möglichkeit, über alle Zeilen einer Datei zu iterieren,
besteht darin, das Dateiobjekt als Iterator in einer :ref:`for-loop` zu
behandeln:

.. code-block:: pycon

    >>> f = open("docs/types/myfile.txt", "r")
    >>> lc = 0
    >>> for l in f:
    ...     lc = lc + 1
    ...
    >>> print(lc)
    1
    >>> f.close()

Diese Methode hat den Vorteil, dass die Zeilen je nach Bedarf in den Speicher
eingelesen werden, so dass selbst bei großen Dateien kein Speicherplatzmangel zu
befürchten ist. Der andere Vorteil dieser Methode ist, dass sie einfacher und
lesbarer ist.

Ein mögliches Problem mit der Lesemethode kann jedoch entstehen, wenn auf
Windows und macOS Übersetzungen im Textmodus erfolgen, wenn ihr den Befehl
:func:`open` im Textmodus verwendet, :abbr:`d.h. (das heißt)` ohne ein ``b``
anzuhängen. Im Textmodus wird auf macOS jedes ``\r`` in ``\n`` umgewandelt,
während unter Windows ``\r\n``-Paare in ``\n`` umgewandelt werden. Ihr könnt die
Behandlung von Zeilenumbrüchen festlegen, indem ihr beim Öffnen der Datei den
Parameter ``newline`` verwendet und ``newline="\n"``, ``\r`` oder ``\r\n``
angebt, wodurch nur diese Zeichenfolge als Zeilenumbruch verwendet wird:

.. code-block:: pycon

    >>> f = open("docs/types/myfile.txt", "r", newline="\r\n")

In diesem Beispiel wird nur ``\n`` als Zeilenumbruch gewertet. Wenn die Datei
jedoch im Binärmodus geöffnet wurde, ist der Parameter ``newline`` nicht
erforderlich, da alle Bytes genau so zurückgegeben werden, wie sie in der Datei
stehen.

``write`` und ``writelines``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Die Schreibmethoden, die den Methoden :meth:`readline()
<codecs.StreamReader.readline>` und :meth:`readlines()
<codecs.StreamReader.readlines>` entsprechen, sind :meth:`write()
<codecs.StreamWriter.write>` und :meth:`writelines()
<codecs.StreamWriter.writelines>`. Beachtet, dass es keine
``writeline``-Funktion gibt. :meth:`write() <codecs.StreamWriter.write>`
schreibt eine einzelne Zeichenkette, die sich über mehrere Zeilen erstrecken
kann, wenn Zeilenumbruchzeichen in die Zeichenkette eingebettet sind, wie im
folgenden Beispiel:

.. code-block:: python

   f.write("Hi, Pythinistas!\n\n")

Die Methode :meth:`writelines() <codecs.StreamWriter.writelines>` ist jedoch
verwirrend, weil sie nicht unbedingt mehrere Zeilen schreibt; sie nimmt eine
Liste von Zeichenketten als Argument und schreibt sie nacheinander in das
angegebene Datei-Objekt, ohne Zeilenumbrüche zwischen den Listenelementen
einzufügen; nur wenn die Zeichenketten in der Liste Zeilenumbrüchen enthalten,
kommen Zeilenumbrüche im Datei-Objekt hinzu; andernfalls werden sie
aneinandergereiht. :meth:`writelines() <codecs.StreamWriter.writelines>` ist
damit die genaue Umkehrung von :meth:`readlines()
<codecs.StreamReader.readlines>`, da sie auf die von :meth:`readlines()
<codecs.StreamReader.readlines>` zurückgegebene Liste angewendet werden kann, um
eine Datei zu schreiben, die identisch mit der Ausgangsdatei ist. Unter der
Annahme, dass :file:`myfile.txt` existiert und eine Textdatei ist, erzeugt das
folgende Beispiel eine exakte Kopie von :file:`myfile.txt` mit dem Namen
:file:`myfile2.txt`:

.. code-block:: pycon

    >>> input_file = open("myfile.txt", "r")
    >>> lines = input_file.readlines()
    >>> input_file.close()
    >>> output_file = open("myfile2.txt", "w")
    >>> output_file.writelines(lines)
    >>> output_file.close()

Verwendung des Binärmodus
~~~~~~~~~~~~~~~~~~~~~~~~~

Wenn ihr alle Daten in einer Datei in ein einziges Byte-Objekt (partiell)
einlesen und in den Speicher übertragen möchtet um sie als Byte-Sequenz
behandeln zu können, könnt ihr die :meth:`read()
<codecs.StreamReader.read>`-Methode verwenden. Ohne ein Argument liest sie die
gesamte Datei ab der aktuellen Position ein und gibt die Daten als Bytes-Objekt
zurück. Mit einem ganzzahligen Argument liest sie maximal diese Anzahl von Bytes
und gibt ein Bytes-Objekt der angegebenen Größe zurück:

.. code-block:: pycon
    :linenos:

    >>> f = open("myfile.txt", "rb")
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

Checks
------

* Verwendet die Funktionen des :mod:`python3:os`-Moduls, um einen Pfad zu einer
  Datei namens :file:`example.log` zu nehmen und einen neuen Dateipfad im selben
  Verzeichnis für eine Datei namens :file:`example.log1` zu erstellen.

* Welche Bedeutung hat das Hinzufügen von ``b`` als Parameter von
  :func:`python3:open`?

* Öffnet eine Datei :file:`my_file.txt` und fügt zusätzlichen Text am Ende der
  Datei ein. Welchen Befehl würdet ihr verwenden, um :file:`my_file.txt` zu
  öffnen? Welchen Befehl würdet ihr verwenden, um die Datei erneut zu öffnen und
  von Anfang an zu lesen?

* Welche Anwendungsfälle könnt ihr euch vorstellen, in denen das
  :mod:`python3:struct`-Modul für das Lesen oder Schreiben von Binärdaten
  nützlich wäre?

* Warum könnte :doc:`pickle <python3:library/pickle>` für die folgenden
  Anwendungsfälle geeignet sein oder auch nicht:

  #. Speichern einiger Zustandsvariablen von einem Durchlauf zum nächsten
  #. Aufbewahren von Auswertungsergebnissen
  #. Speichern von Benutzernamen und Passwörtern
  #. Speichern eines großen Wörterbuchs mit englischen Begriffen

* Wenn ihr euch die `Manpage für das wc-Dienstprogramm
  <https://linux.die.net/man/1/wc>`_ anseht, seht ihr zwei
  Befehlszeilenoptionen:

  ``-c``
      zählt die Bytes in der Datei
  ``-m``
      zählt die Zeichen, die im Falle einiger Unicode-Zeichen zwei oder mehr
      Bytes lang sein können

  Außerdem sollte unser Modul, wenn eine Datei angegeben wird, aus dieser Datei
  lesen und sie verarbeiten, aber wenn keine Datei angegeben wird, sollte es aus
  ``stdin`` lesen und verarbeiten.

* Schreibt eure Version des :mod:`wc`-Dienstprogramms so um, dass es sowohl die
  Unterscheidung zwischen Bytes und Zeichen als auch die Möglichkeit, aus
  Dateien und von der Standardeingabe zu lesen, implementiert.

* Wenn ein Kontext-Manager in einem Skript verwendet wird, das mehrere Dateien
  liest und/oder schreibt, welche der folgenden Ansätze wäre eurer Meinung nach
  am besten?

  #. Legt das gesamte Skript in einen Block, der von einer ``with``-Anweisung
     verwaltet wird.
  #. Verwendet eine ``with``-Anweisung für alle Lesevorgänge und eine weitere
     für alle Schreibvorgänge.
  #. Verwendet jedes Mal eine ``with``-Anweisung, wenn ihr eine Datei lest oder
     schreibt, :abbr:`d.h. (das heißt)` für jede Zeile.
  #. Verwendet für jede Datei, die ihr lest oder schreibt, eine
     ``with``-Anweisung.

* Archiviert :file:`*.txt`-Dateien aus dem aktuellen Verzeichnis im Verzeichnis
  :file:`archive` als :file:`*.zip`-Dateien mit dem aktuellen Datum als
  Dateiname.

  * Welche Module benötigt ihr hierfür?
  * Schreibt eine mögliche Lösung.
