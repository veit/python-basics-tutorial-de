Methoden
========

Eine Methode ist eine Funktion, die mit einer bestimmten Klasse verbunden ist.
Ihr habt bereits die spezielle ``__init__``-Methode kennengelernt, die bei einer
neuen Instanz aufgerufen wird, wenn diese erstellt wird. Im folgenden Beispiel
definiert ihr eine weitere Methode, ``circumference``, für die Klasse
``Square``; diese Methode kann verwendet werden, um den Umfang für eine
beliebige ``Square``-Instanz zu berechnen und zurückzugeben. Wie die meisten
benutzerdefinierten Methoden wird ``circumference`` mit einer Syntax aufgerufen,
die dem Zugriff auf Instanzvariablen ähnelt:

.. code-block:: python

    >>> class Square:
    ...     def __init__(self):
    ...         self.length = 1
    ...     def circumference(self):
    ...         return 4 * self.length
    ...
    >>> s = Square()
    >>> s.length = 5
    >>> print(s.circumference())
    20

Die Syntax für Methodenaufrufe besteht aus einer Instanz, gefolgt von einem
Punkt, gefolgt von der Methode, die auf der Instanz aufgerufen werden soll. Wenn
eine Methode auf diese Weise aufgerufen wird, handelt es sich um einen
gebundenen Methodenaufruf. Eine Methode kann jedoch auch als ungebundene Methode
aufgerufen werden, indem über ihre enthaltende Klasse auf sie zugegriffen wird.
Diese Praxis ist weniger praktisch und wird fast nie angewandt, da das erste
Argument einer Methode, die auf diese Weise aufgerufen wird, eine Instanz der
Klasse sein muss, in der die Methode definiert ist, und weniger klar ist:

.. code-block:: python

    >>> print(Square.circumference(s))
    20

Wie ``__init__`` wird auch die ``circumference``-Methode als Funktion innerhalb
der Klasse definiert. Das erste Argument jeder Methode ist die Instanz, von der
oder auf der sie aufgerufen wurde, konventionsgemäß ``self`` genannt. In vielen
Sprachen wird die Instanz ``this`` genannt und nie explizit übergeben.

Methoden können mit Argumenten aufgerufen werden, wenn die Methodendefinitionen
diese Argumente akzeptieren. Diese Version von ``Square`` fügt der
``__init__``-Methode ein Argument hinzu, so dass ihr Quadrate mit einer
bestimmten Kantenlänge erstellen könnt, ohne die Kantenlänge nach der Erstellung
eines Quadrats festlegen zu müssen:

.. code-block:: python

    >>> class Square:
    ...     def __init__(self, length):
    ...         self.length = length
    ...     def circumference(self):
    ...         return 4 * self.length

.. warning::

    ``self.length`` und ``length`` sind nicht dasselbe!

    * ``self.length`` ist die Instanzvariable namens ``length``
    * ``length`` ist der lokale Funktionsparameter

    In der Praxis würdet ihr den lokalen Funktionsparameter wahrscheinlich als
    ``lng`` oder ``l`` bezeichnen, um Verwechslungen zu vermeiden.

Mit dieser Definition von ``Square`` könnt ihr Quadrate mit beliebigen
Kantenlängen mit einem Aufruf der Klasse ``Square`` erstellen. Im Folgenden wird
ein Quadrat mit der Kantenlänge ``3`` erstellt:

.. code-block:: python

    s = Square(3)

Alle Standardfunktionen von Python – Standardargumente, zusätzliche Argumente,
Schlüsselwortargumente :abbr:`usw. (und so weiter)` – können mit Methoden
verwendet werden. Ihr hättet die erste Zeile von ``__init__`` wie folgt
definieren können:

.. code-block:: python

    ...     def __init__(self, length=1):

Dann würde der Aufruf von ``Square`` mit oder ohne zusätzliches Argument
funktionieren; ``Square()`` würde ein Quadrat mit der Kantenlänge ``1`` und
``Square(3)`` ein Quadratmit der Kantenlänge ``3`` zurückgeben.

Bei einem Methodenaufruf ``instance.method(arg1, arg2, …)`` wandelt Python
diesen in einen normalen Funktionsaufruf um, indem es die folgenden Regeln
anwendet:

#. Suche nach dem Methodennamen im Instanz-Namensraum. Wenn eine Methode für
   diese Instanz geändert oder hinzugefügt wurde, wird sie bevorzugt gegenüber
   Methoden in der Klasse aufgerufen. 
#. Wenn die Methode nicht im Namensraum der Instanz gefunden wird, wird die
   Methode in der Klasse gesucht. In den vorangegangenen Beispielen ist
   ``class`` der ``Square``-Typ der Instanz ``s``.
#. Wenn die Methode gefunden wurde, wird sie als normale Python-Funktion
   aufgerufen, wobei die Instanz als erstes Argument der Funktion verwendet und
   alle anderen Argumente im Methodenaufruf um ein Leerzeichen nach rechts
   verschoben werden. So wird ``instance.method(arg1, arg2, …)`` zu
   ``class.method(instance, arg1, arg2, …)``.

Statische Methoden
------------------

Genau wie in Java könnt ihr statische Methoden aufrufen, auch wenn keine Instanz
dieser Klasse erstellt wurde. Um eine statische Methode zu erstellen, verwendet den :func:`@staticmethod <python3:staticmethod>`-:doc:`Dekorator
</functions/decorators>`:

.. literalinclude:: circle.py
    :linenos:

Zeile 8
    definiert die Klassenvariable ``circles``  als zunächst leere Liste aller
    ``Circle``-Instanzen.
Zeile 14
    fügt initialisierte ``Circle``-Instanzen der ``circles``-Liste hinzu.

.. code-block:: python

    >>> import circle
    >>> c1 = circle.Circle(1)
    >>> c2 = circle.Circle(2)
    >>> circle.Circle.circumferences()
    9.424769999999999
    >>> c2.diameter = 3
    >>> circle.Circle.circumferences()
    12.56636

Klassenmethoden
---------------

:func:`Klassenmethoden <classmethod>` ähneln den statischen Methoden insofern,
als sie aufgerufen werden können, bevor ein Objekt der Klasse instanziiert
wurde. Allerdings wird den Klassenmethoden implizit die Klasse, zu der sie
gehören, als erster Parameter übergeben:

.. literalinclude:: circle_cm.py
    :language: python
    :linenos:
    :lines: 18-
    :lineno-start: 18

Zeile 18
    Der ``@classmethod``-Dekorator wird vor der Methode ``def`` verwendet.
Zeile 19
    Der Klassenparameter ist traditionell ``cls``.
Zeile 22
    Ihr könnt ``cls`` anstelle von ``self.__class__`` verwenden.

    Durch die Verwendung einer Klassenmethode anstelle einer statischen Methode
    müsst ihr den Klassennamen nicht hart in ``circumferences`` codieren.

.. code-block:: python

    >>> import circle_cm
    >>> c1 = circle_cm.Circle(1)
    >>> c2 = circle_cm.Circle(2)
    >>> circle_cm.Circle.circumferences()
    9.424769999999999
