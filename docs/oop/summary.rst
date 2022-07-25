Zusammenfassung
===============

Die bisher angesprochenen Punkte, sind die Grundlagen der Verwendung von Klassen
und Objekten in Python. Diese Grundlagen werde ich nun in einem einzigen
Beispiel zusammenfassen:

#. Zunächst erstellen wir eine Basisklasse: 

    .. literalinclude:: form.py
        :language: python
        :linenos:
        :lines: 1-9
        :lineno-start: 1

    Zeile 4
        Die ``__init__``-Methode benötigt eine Instanz (``self``) und zwei
        Parameter
    Zeilen 5 und 6
        Auf die beiden Instanzvariablen ``x`` und ``y``, auf die über ``self``
        zugegriffen wird.
    Zeile 7
        Die ``move``-Methode benötigt eine Instanz (``self``) und zwei
        Parameter.
    Zeilen 8 und 9
        Instanzvariablen, die in der ``move``-Methode gesetzt werden.

#. Als nächstes erstellt eine Unterklasse, die von der Basisklasse ``Form``
   erbt:

    .. literalinclude:: form.py
        :language: python
        :linenos:
        :lines: 11-17
        :lineno-start: 11

    Zeile 11
        Die Klasse ``Square`` erbt von der Klasse ``Form``.
    Zeile 13
        ``Square``’s ``__init__`` nimmt eine Instanz (``self``) und drei
        Parameter, alle mit Voreinstellungen.
    Zeile 14
        ``__init__`` von Circle verwendet ``super()``, um ``__init__`` von
        ``Form`` aufzurufen.

#. Schließlich erstellen wir eine weitere Unterklasse, die zudem eine statische
   Methode enthält:

    .. literalinclude:: form.py
        :language: python
        :linenos:
        :lines: 19-35
        :lineno-start: 19

    Zeilen 21 und 22
        ``pi`` und ``circles`` sind Klassenvariablen für ``Circle``.
    Zeile 26
        In der ``__init__``-Methode fügt sich die Instanz in die Liste
        ``circles`` ein.
    Zeilen 29 und 30
        ``circumferences`` ist eine Klassenmethode und nimmt die Klasse selbst
        (``cls``) als Parameter.
    Zeile 33
        verwendet den Parameter ``cls`` für den Zugriff auf die Klassenvariable
        ``circles``.

Jetzt könnt ihr einige Instanzen der Klasse ``Circle`` erstellen und sie
analysieren. Da die ``__init__``-Methode von ``Circle`` Standardparameter hat,
könnt ihr einen Kreis erstellen, ohne irgendwelche Parameter anzugeben:

    .. code-block:: python

        >>> import form
        >>> c1 = form.Circle()
        >>> c1.diameter, c1.x, c1.y
        (1, 0, 0)

Wenn ihr Parameter angebt, werden diese verwendet, um die Werte der Instanz
festzulegen:

    .. code-block:: python

        >>> c2 = form.Circle(2, 3, 4)
        >>> c2.diameter, c2.x, c2.y
        (2, 3, 4)

Wenn ihr die ``move()``-Methode aufruft, findet Python keine ``move()``-Methode
in der Klasse ``Circle``, also wird in der Vererbungshierarchie nach oben
gegangen und die ``move()``-Methode von ``Form`` verwendet:

    .. code-block:: python

        >>> c2.move(5, 6)
        >>> c2.diameter, c2.x, c2.y
        (2, 8, 10)

Ihr könnt auch die Klassenmethode ``circumferences()`` der Klasse ``Circle``
aufrufen, entweder über die Klasse selbst oder durch eine Instanz:

    .. code-block:: python

        >>> form.Circle.circumferences()
        9.424769999999999
        >>> c2.circumferences()
        9.424769999999999
