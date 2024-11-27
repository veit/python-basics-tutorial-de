Klassen und Vererbung
=====================

Die Vererbung in Python ist einfacher und flexibler als die Vererbung in
kompilierten Sprachen wie Java und C++, da die dynamische Natur von Python der
Sprache nicht so viele Einschränkungen auferlegt.

Um zu sehen, wie Vererbung in Python verwendet wird, beginnen wir mit den
Klassen ``Square`` und ``Circle``, die wir früher bereits besprochen haben, und
verallgemeinern sie.

Wenn wir diese Klassen nun in einem Zeichenprogramm verwenden wollen, müssen wir
definieren, wo auf der Zeichenfläche sich eine Instanz befindet soll. Wir können
dies tun, indem wir ``x``- und ``y``-Koordinaten für jede Instanz definieren:

.. code-block:: pycon
    :linenos:

    >>> class Square:
    ...     def __init__(self, length=1, x, y):
    ...         self.length = length
    ...         self.x = x
    ...         self.y = y
    ...
    >>> class Circle:
    ...     def __init__(self, diameter=1, x=0, y=0):
    ...         self.diameter = diameter
    ...         self.x = x
    ...         self.y = y
    ...

Dieser Ansatz funktioniert, führt aber zu einer Menge sich wiederholenden Codes,
wenn ihr die Anzahl der Form-Klassen erhöht, da ihr vermutlich wollt, dass jede
Form diese Positionsangabe hat. Dies ist eine Standardsituation für die
Verwendung von Vererbung in objektorientierten Sprachen. Anstatt die ``x``- und
``y``-Variablen in jeder Form-Klasse zu definieren, könnt ihr sie in eine
allgemeine Form-Klasse abstrahieren und jede Klasse, die eine bestimmte Form
definiert, von dieser allgemeinen Klasse erben lassen. In Python sieht diese
Technik wie folgt aus:

.. code-block:: pycon
    :linenos:

    >>> class Form:
    ...     def __init__(self, x, y):
    ...         self.x = x
    ...         self.y = y
    ...
    >>> class Square(Form):
    ...     def __init__(self, length=1, x=0, y=0):
    ...         super().__init__(x, y)
    ...         self.length = length
    ...
    >>> class Circle(Form):
    ...     def __init__(self, diameter=1, x=0, y=0):
    ...         super().__init__(x, y)
    ...         self.diameter = diameter
    ...

Zeilen 6 und 11
    ``Square`` und ``Circle`` erben von der ``Form``-Klasse.
Zeilen 8 und 13
    rufen die ``__init__``-Methode der ``Form``-Klasse auf.

Es gibt im Allgemeinen zwei Anforderungen bei der Verwendung einer geerbten
Klasse in Python, die ihr beide im Code der Klassen ``Circle`` und ``Square``
sehen könnt:

#. Die erste Anforderung besteht darin, die Vererbungshierarchie zu definieren,
   was ihr tut, indem ihr die Klassen, von denen geerbt wird, in Klammern
   unmittelbar nach dem Namen der Klasse angebt, die mit dem Schlüsselwort
   ``class`` definiert wird: ``Circle`` und ``Square`` erben beide von ``Form``.
#. Das zweite Element ist der explizite Aufruf der ``__init__``-Methode der
   geerbten Klasse. Dies erfolgt in Python nicht automatisch, sondern meist über
   die ``super``-Funktion, genauer durch die Zeilen ``super().__init__(x,y)``.
   Dieser Code ruft die Initialisierungsfunktion von ``Form`` mit der zu
   initialisierenden Instanz und den entsprechenden Argumenten auf. Andernfalls
   würden für die Instanzen von ``Circle`` und ``Square`` die Instanz-Variablen
   ``x`` und ``y`` nicht gesetzt.

Die Vererbung kommt auch dann zum Tragen, wenn ihr versucht, eine Methode zu
verwenden, die nicht in den Basisklassen, sondern in der Superklasse definiert
ist. Um diesen Effekt zu sehen, definiert eine weitere Methode in der Klasse
``Form`` mit dem Namen ``move``, die eine Form in den ``x``- und
``y``-Koordinaten verschiebt. Die Definition für ``Form`` lautet nun:

.. code-block:: pycon
    :linenos:
    :emphasize-lines: 5-7

    >>> class Form:
    ...     def __init__(self, x=0, y=0):
    ...         self.x = x
    ...         self.y = y
    ...     def move(self, delta_x, delta_y):
    ...         self.x = self.x + delta_x
    ...         self.y = self.y + delta_y
    ...

..
    .. code-block:: pycon

        >>> class Circle(Form):
        ...     def __init__(self, diameter=1, x=0, y=0, delta_x=0, delta_y=0):
        ...         super().__init__(x, y)
        ...         self.diameter = diameter
        ...

Wenn ihr die Parameter ``delta_x`` und ``delta_y`` der Methode ``move`` in den
``__init__``-Methoden von ``Circle`` und ``Square`` übernehmt, könnt ihr :abbr:`z.B. (zum Beispiel)` folgende interaktive Sitzung ausführen:

.. code-block:: pycon

    >>> c = Circle(3)
    >>> c.move(4, 5)
    >>> c.x
    4
    >>> c.y
    5

Die Klasse ``Circle`` im Beispiel hat nicht direkt eine ``move``-Methode in sich
selbst definiert, aber da sie von einer Klasse erbt, die ``move`` implementiert,
können alle Instanzen von ``Circle`` die ``move``-Methode verwenden. In
OOP-Begriffen könnte man sagen, dass alle Python-Methoden virtuell sind – :abbr:`d.h. (das heißt)`, wenn eine Methode in der aktuellen Klasse nicht existiert,
wird die Liste der Oberklassen nach der Methode durchsucht und die erste
gefundene verwendet.

Check
-----

* Schreibt den Code für eine :class:`Triangle`-Klasse um, sodass sie von
  :class:`Form` erbt.

* Wie würdet ihr den Code schreiben, um eine Methode :func:`area` für die Klasse
  :class:`Triangle` hinzuzufügen? Sollte die Methode :func:`area` in die
  Basisklasse :class:`Form` verschoben und an :class:`Circle`, :class:`Square`
  und :class:`Triangle` vererbt werden? Welche Probleme würde diese Änderung
  verursachen?
