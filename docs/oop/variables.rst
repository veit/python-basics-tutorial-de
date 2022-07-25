Variablen
=========

Instanzvariablen 
----------------

Im vorigen Beispiel ist ``length`` eine Instanzvariable von
``Square``-Instanzen, :abbr:`d.h. (das heißt)`, jede Instanz der Klasse
``Square`` hat ihre eigene Kopie von ``length``, und der in dieser Kopie
gespeicherte Wert kann sich von den Werten unterscheiden, die in der
``length``-Variable in anderen Instanzen gespeichert sind. In Python könnt ihr
Instanzvariablen nach Bedarf erstellen, indem ihr sie dem Feld einer
Klasseninstanz zuweist. Wenn die Variable noch nicht existiert, wird sie
automatisch erstellt.

Alle Verwendungen von Instanzvariablen, sowohl die Zuweisung als auch der
Zugriff, erfordern die explizite Erwähnung der enthaltenen Instanz, :abbr:`d.h.
(das heißt)` ``instance.variable``. Ein Verweis auf eine Variable an sich ist
kein Verweis auf eine Instanzvariable, sondern auf eine lokale Variable in der
ausführenden Methode. Dies ist ein Unterschied zu C++ und Java, wo
Instanzvariablen auf die gleiche Weise referenziert werden wie lokale
Funktionsvariablen der Methode. Python schreibt hier die explizite Erwähnung der
enthaltenen Instanz vor, und dies ermöglicht eine klare Unterscheidung zwischen
Instanzvariablen und lokalen Funktionsvariablen.

Klassenvariablen
----------------

Eine Klassenvariable ist eine Variable, die mit einer Klasse verbunden ist,
nicht mit einer Instanz einer Klasse, und auf die alle Instanzen der Klasse
zugreifen können. Eine Klassenvariable kann verwendet werden, um einige
Informationen auf Klassenebene zu speichern, :abbr:`z.B. (zum Beispiel)` wie
viele Instanzen der Klasse zu einem bestimmten Zeitpunkt erstellt wurden. Python
stellt Klassenvariablen zur Verfügung, obwohl deren Verwendung etwas mehr
Aufwand erfordert als in den meisten anderen Sprachen. Außerdem müsst ihr auf
eine Wechselwirkung zwischen Klassen- und Instanzvariablen achten.

Eine Klassenvariable wird durch eine Zuweisung in der Klasse, jedoch außerhalb
der ``__init__``-Funktion, erzeugt. Nachdem sie erstellt wurde, kann sie von
allen Instanzen der Klasse gesehen werden. ihr könnt eine Klassenvariable
verwenden, um einen Wert für ``pi`` für alle Instanzen der Klasse ``Circle``
zugänglich zu machen:

.. code-block:: python

    >>> class Circle:
    ...     pi = 3.14159
    ...     def __init__(self, diameter):
    ...         self.diameter = diameter
    ...     def circumference(self):
    ...         return self.diameter * Circle.pi

Wenn ihr diese Definition eingegeben habt, könnt ihr ``pi`` abfragen mit:

.. code-block:: python

    >>> Circle.pi
    3.14159

.. note::

    Die Klassenvariable ist mit der Klasse, die sie definiert, verknüpft und in
    ihr enthalten. Ihr greift in diesem Beispiel auf ``Circle.pi`` zu, bevor
    irgendwelche ``Circle``-Instanzen erstellt wurden. Es ist offensichtlich,
    dass ``Circle.pi`` unabhängig von bestimmten Instanzen der Klasse ``Circle``
    existiert.

Ihr könnt auch von einer Methode einer Klasse aus über den Klassennamen auf eine
Klassenvariable zugreifen. Ihr tut dies in der Definition von
``Circle.circumference``, wo die Funktion ``circumference`` einen speziellen
Verweis auf ``Circle.pi`` enthält:

.. code-block:: python

    >>> c = Circle(3)
    >>> c.circumference()
    9.424769999999999

Unschön ist jedoch, dass der Klassenname ``Circle`` in der Methode
``circumference`` verwendet wird, um die Klassenvariable ``pi`` anzusprechen.
Ihr könnt dies vermeiden, indem ihr das spezielle ``__class__``-Attribut
verwendet, das für alle Python-Klasseninstanzen verfügbar ist. Dieses Attribut
gibt die Klasse zurück, zu der die Instanz gehört, :abbr:`z.B. (zum Beispiel)`:

.. code-block::

    >>> Circle
    <class '__main__.Circle'>
    >>> c.__class__
    <class '__main__.Circle'>

Die Klasse ``Circle`` wird intern durch eine abstrakte Datenstruktur
repräsentiert, und diese Datenstruktur ist genau das, was durch das
``__class__``-Attribut von ``c``, einer Instanz der Klasse ``Circle``, erhalten
wird. In diesem Beispiel könnt ihr den Wert von ``Circle.pi`` von ``c`` abrufen,
ohne sich explizit auf den Namen der Klasse ``Circle`` zu beziehen:

.. code-block::

    >>> c.__class__.pi
    3.14159

Ihr könnt diesen Code intern in der Methode ``circumference`` verwenden, um den
expliziten Verweis auf die Klasse ``Circle`` loszuwerden; ersetzt ``Circle.pi``
durch ``self.__class__.pi``.

Es gibt eine kleine Merkwürdigkeit bei Klassenvariablen, die euch verwirren
könnte, wenn ihr euch dessen nicht bewusst seid.

.. warning::

    Wenn Python eine Instanzvariable sucht und keine Instanzvariable mit diesem
    Namen findet, wird der Wert in einer Klassenvariablen mit demselben Namen
    gesucht und zurückzugeben. Nur wenn keine passende Klassenvariable gefunden
    werden kann, gibt Python einen Fehler aus. Damit können zwar effizient
    Standardwerte für Instanzvariablen implementiert werden;  dies führt jedoch
    auch leicht dazu, versehentlich auf eine Instanzvariable statt auf eine
    Klassenvariable zu verweisen, ohne dass ein Fehler gemeldet wird.

    Zunächst könnt ihr euch auf die Variable ``c.pi`` beziehen, obwohl ``c``
    keine zugehörige Instanzvariable namens ``pi`` hat. Python versucht
    zunächst, eine solche Instanzvariable zu finden und erst, wenn es keine
    Instanzvariable finden kann, wird eine Klassenvariable ``pi`` in ``Circle``
    gesucht:

    .. code-block:: python

        >>> c1 = Circle(1)
        >>> c1.pi
        3.14159

    Wenn ihr nun feststellt, dass eure Angabe für ``pi`` zu früh gerundet wurde
    und ihr sie durch eine präzisere Angabe ersetzen wollt, könntet ihr geneigt
    sein, dies folgendermaßen zu ändern:

    .. code-block:: python

        >>> c1.pi = 3.141592653589793
        >>> c1.pi
        3.141592653589793

    Ihr habt jetzt jedoch lediglich ``c1`` eine neue Instanzvariable ``pi``
    hinzugefügt. Die Klassenvariable ``Circle.pi`` und alle anderen daraus
    abgeleiteten Instanzen haben weiterhin nur fünf Nachkommastellen:

    .. code-block:: python

        >>> Circle.pi
        3.14159
        >>> c2 = Circle(2)
        >>> c1.pi
        3.14159
