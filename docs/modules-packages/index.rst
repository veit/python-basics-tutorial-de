Erstellen von Modulen
=====================

Es ist einfach, eigene Module zu erstellen, die auf die gleiche Weise
importiert und verwendet werden können wie die in Python eingebauten
Bibliotheksmodule. Das Beispiel in dieser Auflistung ist ein einfaches Modul
mit einer Funktion, die zur Eingabe eines Dateinamens auffordert und die Anzahl
der in dieser Datei vorkommenden Wörter ermittelt.

.. literalinclude:: wc.py
   :linenos:

:doc:`../document/docstrings` sind Standardmethoden zur Dokumentation von
Modulen, Funktionen, Methoden und Klassen. ``read`` gibt eine Zeichenkette
zurück, die alle Zeichen in einer Datei enthält, und ``split`` gibt eine Liste
der Wörter einer Zeichenkette zurück, die anhand von Leerzeichen *aufgespalten*
wurde. Ihr könnt einen ``\`` verwenden, um eine lange Anweisung über mehrere
Zeilen zu verteilen. Mit der ``if``-Anweisung kann das Programm als Skript
ausgeführt werden, indem man ``python3 wc.py`` in der Befehlszeile eingibt.

Wenn ihr eine Datei in einem der Verzeichnisse des Modulsuchpfads ablegt, der
mit ``sys.path`` zu finden ist, kann sie wie jedes der eingebauten
Bibliotheksmodule mit der ``import``-Anweisung importiert werden:

.. code-block:: python

    >>> import wc
    >>> wc.words_occur()

Diese Funktion wird mit der gleichen Attributsyntax aufgerufen, die für
Bibliotheksmodulfunktionen verwendet wird.

.. note::
    Wenn ihr die Datei ``wc.py`` auf der Festplatte ändert, bringt ``import``
    die Änderungen nicht in dieselbe interaktive Sitzung. In diesem Fall
    solltet ihr stattdessen die Funktion ``reload`` aus der
    :doc:`imp <python3:library/imp>`-Bibliothek:

    .. code-block:: python

        >>> import imp
        >>> imp.reload(wc)
        <module 'wc'>

Für größere Projekte gibt es eine Verallgemeinerung des Modulkonzepts, die
Pakete genannt wird und die es euch ermöglicht, Module in einem Verzeichnis
oder einem Unterverzeichnis zu gruppieren und dann zu importieren und
hierarchisch auf sie zu verweisen, indem ihr eine
``package.subpackage.module``-Syntax verwendet. Dies erfordert nicht viel mehr
als die Erstellung einer möglicherweise leeren Initialisierungsdatei für jedes
Paket oder Unterpaket. Eine Vorlage hierfür findet ihr in meinem
`ccokiecutter-namespace-template
<https://github.com/veit/cookiecutter-namespace-template>`_.
