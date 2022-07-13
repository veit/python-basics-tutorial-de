Objektorientierung
==================

Python bietet volle Unterstützung für `Objektorientierte Programmierung
<https://de.wikipedia.org/wiki/Objektorientierte_Programmierung>`_ :abbr:`OOP
(Objektorientierte Programmierung)`. Das folgende Beispiel könnte der Anfang
eines einfachen *Shapes*-Moduls für ein Zeichenprogramm sein.

.. literalinclude:: form.py
    :linenos:

Zeile 2
    Klassen werden mit dem Schlüsselwort ``class`` definiert.
Zeile 4
    Methoden werden, wie Funktionen, mit dem Schlüsselwort ``def`` definiert.

    Die Instanzinitialisierungsmethode (Konstruktor) für eine Klasse heißt
    immer ``__init__``.

Zeilen 5—6
    Das erste Argument einer Methode wird per Konvention ``self`` genannt. Wenn
    die Methode aufgerufen wird, wird ``self`` auf die Instanz gesetzt, die die
    Methode aufgerufen hat.

    Hier werden die Instanzvariablen ``x`` und ``y`` angelegt und
    initialisiert.

Zeile 15
    Die Klasse ``Circle`` erbt von der Klasse ``Shape``.
Zeile 19
    Eine Klasse muss in ihrem Initialisierer explizit den Initialisierer ihrer
    Basisklasse aufrufen.
Zeile 24
    Die Methode ``__str__`` wird von der Funktion ``print`` verwendet.

Andere spezielle Methodenattribute ermöglichen das Überladen von Operatoren
oder werden von eingebauten Methoden wie der Funktion *length* (``len``)
verwendet.

.. code-block:: python

    >>> import form
    >>> c1 = form.Circle()
    >>> c2 = form.Circle(3, 5, 8)
    >>> print(c1)
    Circle of radius 1 at coordinates (0, 0)
    >>> print(c2)
    Circle of radius 3 at coordinates (5, 8)
    >>> c2.area()
    28.27431
    >>> c2.move(6, 10)
    >>> print(c2)
    Circle of radius 3 at coordinates (11, 18)
