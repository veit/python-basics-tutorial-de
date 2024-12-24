Module
======

Module werden in Python verwendet, um größere Projekte zu organisieren. Die
Python-Standardbibliothek ist in Module aufgeteilt, um sie überschaubarer zu
machen. Ihr müsst euren eigenen Code zwar nicht in Modulen organisieren, aber
wenn ihr umfangreichere Programme schreibt, oder Code, den ihr wiederverwenden
möchtet, solltet ihr dies tun.

Was ist ein Modul?
------------------

Ein Modul ist eine Datei, die Code enthält. Sie definiert eine Gruppe von
Python-Funktionen oder anderen Objekten, und der Name des Moduls wird vom Namen
der Datei abgeleitet. Module enthalten meist Python-Quellcode [#]_ und fassen
verwandte Python-Objekte zusammen und helfen, Namenskonflikte zu vermeiden. So
könnt ihr für euer Programm ein Modul namens ``mymodule`` schreiben, das eine
Funktion namens :func:`my_func` definiert. Im selben Programm möchtet ihr
vielleicht auch ein anderes Modul namens ``othermodule`` verwenden, das
ebenfalls eine Funktion namens :func:`my_func` definiert, aber etwas anderes tut
als :func:`mymodule.my_func`. Ohne Module wäre es unmöglich, zwei verschiedene
Funktionen mit demelben Namen zu verwenden. Mit Modulen könnt ihr in eurem
Hauptprogramm auf die Funktionen :func:`mymodule.my_func` und
:func:`othermodule.my_func` verweisen. Die Verwendung der Modulnamen sorgt
dafür, dass die beiden :func:`my_func`-Funktionen nicht verwechselt werden, da
Python :abbr:`sog. (sogenannte)` Namespaces verwendet. Ein Namespace ist im
Wesentlichen ein Wörterbuch mit Bezeichnungen für die dort zur Verfügung
stehenden Funktionen, Klassen, Module :abbr:`usw (und so weiter)`.

Module werden auch verwendet, um Python selbst überschaubarer zu machen. Die
meisten Standardfunktionen von Python sind nicht in den Kern der Sprache
integriert, sondern werden über spezielle Module bereitgestellt, die ihr bei
Bedarf laden könnt.

.. seealso::
   * :doc:`../libs/batteries`
   * :ref:`python3:py-modindex`

Erstellen von Modulen
---------------------

Vermutlich der beste Weg, um etwas über Module zu lernen, ist das Erstellen
eines eigenen Moduls. Hierzu erstellen wir eine Textdatei mit dem Namen
:file:`wc.py`, und geben in diese Textdatei den unten stehenden Python-Code ein.
Wenn ihr :ref:`idle` verwendet, wählt :menuselection:`File --> New Window` und
beginnt mit der Eingabe.

Es ist einfach, eigene Module zu erstellen, die auf die gleiche Weise
importiert und verwendet werden können wie die in Python eingebauten
Bibliotheksmodule. Das folgende Beispiel ist ein einfaches Modul mit einer
Funktion, die zur Eingabe eines Dateinamens auffordert und die Anzahl der in
dieser Datei vorkommenden Wörter ermittelt.

.. literalinclude:: wc.py
   :linenos:

Zeilen 1 und 5
    :doc:`../document/sphinx/docstrings` sind Standardmethoden zur Dokumentation
    von Modulen, Funktionen, Methoden und Klassen.
Zeile 10
    ``read`` gibt eine Zeichenkette zurück, die alle Zeichen in einer Datei
    enthält, und ``split`` gibt eine Liste der Wörter einer Zeichenkette zurück,
    die anhand von Leerzeichen *aufgespalten* wurde.
Zeilen 25 und 26
    Mit dieser ``if``-Anweisung könnt ihr das Programm auf zweierlei Arten
    nutzen:

    * zum Importieren in der Python-Shell oder einem anderen Python-Skript ist
      ``__name__`` der Dateiname:

      .. code-block:: pycon

         >>> import wc
         >>> wc.words_occur()
         Enter the name of the file: README.rst
         File README.rst has 332 words (191 are unique)
         {'Schnelleinstieg': 1, ...}

      Alternativ könnt ihr auch ``words_occur`` direkt importieren:

      .. code-block:: pycon

         >>> from wc import words_occur
         >>> words_occur()
         Enter the name of the file: README.rst
         File README.rst has 332 words (191 are unique)
         {'Schnelleinstieg': 1, ...}

      Ihr könnt den interaktiven Modus der Python-Shell oder von :ref:`idle`
      verwenden, um ein Modul während der Erstellung inkrementell zu testen.
      Wenn ihr jedoch euer Modul auf der Festplatte ändert, wird es durch die
      erneute Eingabe des Import-Befehls nicht erneut geladen. Zu diesem Zweck
      müsst ihr die Funktion ``reload`` aus dem
      :doc:`importlib <python3:library/importlib>`-Modul verwenden:

      .. code-block:: pycon

         >>> import wc, importlib
         >>> importlib.reload(wc)
         <module 'wc' from '/home/veit/.local/lib/python3.13/site-packages/wc.py'>

    * als Skript  wird es mit dem Namen ``__main__`` ausgeführt und die Funktion
      ``words_occur()`` aufgerufen:

      .. code-block:: console

         $ python3 wc.py
         Enter the name of the file: README.rst
         File README.rst has 332 words (191 are unique)
         {'Schnelleinstieg': 1, ...}

Speichert diesen Code zunächst in einem der Verzeichnisse des Modul-Suchpfads,
die in der Liste von ``sys.path`` zu finden ist. Als Ende des Dateinamens
empfiehlt sich ``.py``, da hierdurch die Datei als Python-Quellcode ausgewiesen
wird.

.. note::
   Die Liste von Verzeichnissen, die mit ``sys.path`` angezeigt wird, hängt von
   eurer Systemkonfiguration ab. Diese Liste von Verzeichnissen wird von Python
   in der Reihenfolge durchsucht, wenn eine Import-Anweisung ausgeführt wird.
   Das erste gefundene Modul, das die Importanforderung erfüllt, wird verwendet.
   Wenn es kein zutreffendes Modul in diesem  Suchpfad gibt, wird ein
   ``ImportError`` ausgelöst.

   Wenn ihr :ref:`idle`  verwendet, könnt ihr euch den Suchpfad und die darin
   enthaltenen Module grafisch ansehen, indem ihr das Fenster
   :menuselection:`File --> Path Browser` verwendet.

   Die Variable ``sys.path`` wird mit dem Wert der Umgebungsvariablen
   ``PYTHONPATH`` initialisiert, falls diese existiert. Wenn ihr ein
   Python-Skript ausführt, wird in die ``sys.path``-Variable für dieses Skript
   das Verzeichnis, in dem sich das Skript befindet, als erstes Element
   eingefügt, so dass ihr auf bequeme Weise feststellen könnt, wo sich das
   ausführende Python-Programm befindet.

Befehlszeilenargumente
----------------------

Wollt ihr in unserem Beispiel den Dateinamen als Befehlszeilenargument
übergeben, also mit

.. code-block:: console

    $ python3 wc.py README.rst

so könnt ihr dies einfach mit folgender Änderung unseres Scripts:

.. literalinclude:: wcargv.py
   :diff: wc.py

:data:`python3:sys.argv`
    gibt eine Liste der Befehlszeilenargumente zurück, die an ein Python-Skript
    übergeben wurden. ``argv[0]`` ist der Skriptname.
:ref:`.pop <python3:tut-morelists>`
    entfernt das Element an der angegebenen Position in der Liste und gibt es
    zurück. Wenn kein Index angegeben wird, entfernt ``.pop()`` das letzte
    Element in der Liste und gibt es zurück.

Das ``argparse``-Modul
----------------------

Ihr könnt ein Skript so konfigurieren, dass es sowohl Kommandozeilenoptionen als
auch Argumente akzeptiert. Das :doc:`argparse <python3:library/argparse>`-Modul
unterstützt beim Parsen verschiedener Argumenttypen und kann sogar Nachrichten
erzeugen. Um das ``argparse``-Modul zu verwenden, erstellt eine Instanz von
:class:`ArgumentParser <python3:argparse.ArgumentParser>`, füllt sie mit
Argumenten und lest dann sowohl die optionalen als auch die Positionsargumente.
Das folgende Beispiel veranschaulicht die Verwendung des Moduls:

.. literalinclude:: wcargparse.py
   :diff: wc.py

Dieser Code erzeugt eine Instanz von ``ArgumentParser`` und fügt dann das
Argument ``filename`` hinzu. Das ``argparse``-Modul gibt ein Namespace-Objekt
zurück, das die Argumente als Attribute enthält. Ihr könnt die Werte der
Argumente mit der Punktnotation abrufen, in unserem Fall mit ``args.filename``.

Ihr könnt das Skript nun aufrufen mit:

.. code-block:: console

   $ python3 wcargparse.py -f index.rst

Zudem wird automatisch eine Hilfeoption ``-h`` oder ``--help`` erzeugt:

.. code-block:: console

   $ python3 wcargparse.py -h
   usage: wcargparse.py [-h] [-f FILENAME]

   optional arguments:
     -h, --help            show this help message and exit
     -f FILENAME, --file FILENAME
                           read data from the file

Checks
------

* Wenn ihr ein Modul :mod:`my_math` erstellt habt, das eine Funktion
  :func:`divide` enthält, welche Möglichkeiten gibt es, diese Funktion zu
  importieren und dann zu verwenden? Was sind die Vor- und Nachteile der
  einzelnen Möglichkeiten?

* Eine Variable ``min`` ist im Modul :mod:`scope.py` enthalten. In welchem der
  folgenden Kontexte kann ``min`` verwendet werden?

  #. Mit dem Modul selbst
  #. Innerhalb der Funktion :func:`scope` des Moduls
  #. Innerhalb eines Skripts, das das Modul :mod:`scope.py` importiert hat

* Packt die Funktionen, die ihr am Ende von :doc:`/functions/decorators`
  erstellt haben, als eigenständiges Modul. Dabei soll die Funktionen zunächst
  lediglich von einem anderen Skript aus vollständig nutzbar sein.

* Macht euer Modul ausführbar.

* Schreibt eure Version des :mod:`wc`-Dienstprogramms so um, dass es sowohl die
  Unterscheidung zwischen Bytes und Zeichen als auch die Möglichkeit, aus
  Dateien und von der Standardeingabe zu lesen, implementiert.

----

.. [#] Module enthalten zwar meist Python-Quellcode, können aber auch
   kompilierte C- oder C++-Objektdateien sein. Kompilierte Module und
   Python-Source-Module werden auf die gleiche Weise verwendet.
