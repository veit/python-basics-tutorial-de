Module und Pakete
=================

Module werden in Python verwendet, um größere Projekte zu organisieren. Die
Python-Standardbibliothek ist in Module aufgeteilt, um sie überschaubarer zu
machen. Ihr müsst euren eigenen Code zwar nicht in Modulen organisieren, aber
wenn ihr umfangreichere Programme schreibt, oder Code, den ihr wiederverwenden
möchten, sollten ihr dies tun.

Was ist ein Modul?
------------------

Ein Modul ist eine Datei, die Code enthält. Sie definiert eine Gruppe von
Python-Funktionen oder anderen Objekten, und der Name des Moduls wird vom Namen
der Datei abgeleitet. Module enthalten meist Python-Quellcode, können aber auch
kompilierte C- oder C++-Objektdateien sein. Kompilierte Module und
Python-Source-Module werden auf die gleiche Weise verwendet.

Module fassen nicht nur verwandte Python-Objekte zusammen, sondern helfen auch, Namenskonflikte zu vermeiden. Do könnt ihr für euer Programm ein Modul namens
``mymodule`` schreiben, das eine Funktion namens ``my_func`` definiert. Im
selben Programm möchtet ihr vielleicht auch ein anderes Modul namens
``othermodule`` verwenden, das ebenfalls eine Funktion namens ``my_func``
definiert, aber etwas anderes tut als eure ``my_func``-Funktion. Ohne Module
wäre es unmöglich, zwei verschiedene Funktionen mit demelben Namen zu verwenden.
Mit Modulen könnt ihr in eurem Hauptprogramm auf die Funktionen
``mymodule.my_func`` und ``othermodule.my_func`` verweisen. Die Verwendung der
Modulnamen sorgt dafür, dass die beiden ``my_func``-Funktionen nicht verwechselt
werden, da Python :abbr:`sog. (sogenannte)` Namespaces verwendet. Ein Namespace
ist im Wesentlichen ein Wörterbuch mit Bezeichnungen für die dort zur Verfügung
stehenden Funktionen, Klassen, Module :abbr:`usw. (und so weiter)`.

Module werden auch verwendet, um Python selbst überschaubarer zu machen. Die
meisten Standardfunktionen von Python sind nicht in den Kern der Sprache
integriert, sondern werden über spezielle Module bereitgestellt, die ihr bei
Bedarf laden könnt.

Erstellen von Modulen
---------------------

Vermutlich der beste Weg, um etwas über Module zu lernen, ist das Erstellen
eines eigenen Moduls. Hierzu erstellen wir eine Textdatei mit dem Namen
:file:`wc.py`, und geben in diese Textdatei den unten stehenden Python-Code ein.
Wenn ihr :ref:`idle` verwendet, wählt :menuselection:`File --> New Window` und
beginnt mit der Eingabe.

Es ist einfach, eigene Module zu erstellen, die auf die gleiche Weise
importiert und verwendet werden können wie die in Python eingebauten
Bibliotheksmodule. Das Beispiel in dieser Auflistung ist ein einfaches Modul
mit einer Funktion, die zur Eingabe eines Dateinamens auffordert und die Anzahl
der in dieser Datei vorkommenden Wörter ermittelt.

.. literalinclude:: wc.py
   :linenos:

Zeilen 1,2 und 4
    :doc:`../document/docstrings` sind Standardmethoden zur Dokumentation von
    Modulen, Funktionen, Methoden und Klassen.
Zeile 9
    ``read`` gibt eine Zeichenkette zurück, die alle Zeichen in einer Datei
    enthält, und ``split`` gibt eine Liste der Wörter einer Zeichenkette zurück,
    die anhand von Leerzeichen *aufgespalten* wurde.
Zeile 17
    Ihr könnt einen ``\`` verwenden, um eine lange Anweisung über mehrere
    Zeilen zu verteilen.
Zeilen 20 und 21
    Mit dieser ``if``-Anweisung könnt ihr das Programm als Skript ausführen,
    indem ihr ``python3 wc.py`` in der Befehlszeile eingebt.

Speichert diesen Code zunächst in einem der Verzeichnisse des Modulsuchpfads,
die mit ``sys.path`` zu finden sind. Als Dateinamensendung empfiehlt sich
``.py``, da hierdurch die Datei als Python-Quellcode ausgewiesen wird.

Startet nun die Python-Shell und gebt das Folgendes ein:

.. code-block:: python

    >>> import wc
    >>> wc.words_occur()
    Enter the name of the file: README.rst
    File README.rst has 332 words (191 are unique)
    {'Schnelleinstieg': 1, ...}

Alternativ könnt ihr auch ``words_occur`` direkt importieren:

.. code-block:: python

    >>> from wc import words_occur
    >>> words_occur()

Ihr könnt den interaktiven Modus der Python-Shell oder von :ref:`idle`
verwenden, um ein Modul während der Erstellung inkrementell zu testen. Wenn ihr
jedoch euer Modul auf der Festplatte ändert, wird es durch die erneute Eingabe
des Import-Befehls nicht erneut geladen. Zu diesem Zweck müsst ihr die Funktion ``reload`` aus dem :doc:`importlib <python3:library/importlib>`-Modul verwenden:

.. code-block:: python

    >>> import wc, importlib
    >>> importlib.reload(wc)
    <module 'wc' from '/home/veit/.local/lib/python3.8/site-packages/wc.py'>

Pakete
------

Für größere Projekte gibt es eine Verallgemeinerung des Modulkonzepts, die
Pakete genannt wird und die es euch ermöglicht, Module in einem Verzeichnis
oder einem Unterverzeichnis zu gruppieren und dann zu importieren und
hierarchisch auf sie zu verweisen, indem ihr eine
``package.subpackage.module``-Syntax verwendet. Dies erfordert nicht viel mehr
als die Erstellung einer möglicherweise leeren Initialisierungsdatei für jedes
Paket oder Unterpaket.

Wheel
~~~~~

Das derzeitige Standardformat zur Verteilung von Python-Modulen und -Anwendungen
ist die Verwendung von `Wheels <https://pythonwheels.com/>`_. Wheels wurden
entwickelt, um die Installation von Python-Code zuverlässiger zu machen und die
Verwaltung von Abhängigkeiten zu erleichtern. Die Details zur Erstellung von
Wheels würden jedoch den Rahmen dieses Abschnitts sprengen, aber alle Details zu
den Anforderungen und dem Prozess zur Erstellung von Wheels findet ihr in
:doc:`jupyter-tutorial:productive/packaging/distribution`.

``py2exe`` und ``py2app``
~~~~~~~~~~~~~~~~~~~~~~~~~

`py2exe <https://www.py2exe.org/>`_ erstellt eigenständige Windows-Programme und `py2app <https://py2app.readthedocs.io/en/latest/>`_ dasselbe für macOS. In
beiden Fällen handelt es sich um einzelne ausführbare Dateien, die auch auf
Rechnern laufen können, auf denen Python nicht installiert ist. In vielerlei
Hinsicht sind jedoch eigenständige ausführbare Dateien nicht ideal, da sie
tendenziell größer und weniger flexibel sind als native Python-Anwendungen, aber
in manchen Situationen können sie auch die beste oder einzige Lösung sein.

``freeze``
~~~~~~~~~~

Auch das ``freeze``-Tool erstellt ein ausführbares Python-Programm, das auf
Rechnern läuft, auf denen Python nicht installiert ist. Wenn ihr das
``freeze``-Tool verwenden möchtet, müsst ihr wahrscheinlich den
Python-Quellcode herunterladen.

Beim *Einfrieren* eines Python-Programms werden C-Dateien erstellt , die dann mit
einem C-Compiler kompiliert und gelinkt werden, den ihr auf eurem System
installiert haben müsst. Die so eingefrorene Anwendung läuft nur auf Plattformen,
für die der verwendete C-Compiler seine ausführbaren Dateien bereitstellt.

.. seealso::

    * `Tools/freeze <https://github.com/python/cpython/tree/main/Tools/freeze>`_
