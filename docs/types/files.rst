Dateien
=======

Der Zugriff auf eine Datei erfolgt über ein Python-Dateiobjekt:

.. code-block:: python
    :linenos:

    >>> f = open("meine_datei", "w")
    >>> f.write("Meine erste Zeile")
    17
    >>> f.write("\nZusätzliche Zeile mit vorangestelltem Zeilenumbruchzeichen")
    59
    >>> f.close()
    >>> f = open("meine_datei", "r")
    >>> line1 = f.readline()
    >>> line2 = f.readline()
    >>> f.close()
    >>> print(line1, line2)
    Meine erste Zeile
     Zusätzliche Zeile mit vorangestelltem Zeilenumbruchzeichen
    >>> import os
    >>> print(os.getcwd())
    /home/veit
    >>> os.chdir(os.path.join("/home/", "veit/", "Dokumente/"))
    >>> filename = os.path.join("/home", "veit/", "meine_datei")
    >>> print(filename)
    /home/veit/meine_datei
    >>> f = open(filename, "r")
    >>> print(f.readline())
    Meine erste Zeile
    >>> f.close()

Zeile 1
    erzeugt ein ein Dateiobjekt. Hier wird die Datei ``meine_datei`` im
    aktuellen Home-Verzeichnis im Schreibmodus (``"w"``) geöffnet.
Zeilen 2, 4 und 6
    schreibt zwei Zeilen in die Datei und schließt die Datei.
Zeile 7
    öffnet die Datei erneut, diesmal im Lesemodus (``"r"``).
Zeile 14
    importiert das eingebaute :doc:`os <python3:library/os>`-Modul, das mehrere
    Funktionen bietet, um sich im Dateisystem zu bewegen und mit den Pfadnamen
    von Dateien und Verzeichnissen zu arbeiten.
Zeile 17
    wechselt in ein anderes Verzeichnis.
Zeile 18
    spricht die Datei mit absolutem Pfadnamen an, sodass ihr trotzdem auf sie
    zugreifen könnt.

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
