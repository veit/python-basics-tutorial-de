Private Variablen und Methoden
==============================

Eine private Variable oder private Methode ist eine Variable, die außerhalb der
Methoden der Klasse, in der sie definiert ist, nicht sichtbar ist. Private
Variablen und Methoden sind aus zwei Gründen nützlich:

#. sie erhöhen die Sicherheit und Zuverlässigkeit, indem sie selektiv den
   Zugriff auf wichtige Teile der Implementierung eines Objekts verweigern
#. sie verhindern Namenskonflikte, die durch die Verwendung von Vererbung
   entstehen können.

Eine Klasse kann eine private Variable definieren und von einer Klasse erben,
die eine private Variable mit demselben Namen definiert. Private Variablen
erleichtern die Lesbarkeit von Code, da sie explizit angeben, was in einer
Klasse nur intern verwendet wird. Alles andere ist die Schnittstelle der Klasse.

Die meisten Sprachen, die private Variablen definieren, tun dies durch die
Verwendung des Schlüsselworts *private* :abbr:`o.ä. (oder Ähnlichem)`. Die
Konvention in Python ist einfacher und macht es auch leichter, sofort zu
erkennen, was privat ist und was nicht. Jede Methode oder Instanzvariable, deren
Name mit einem doppelten Unterstrich (``__``) beginnt, aber nicht endet, ist
privat; alles andere ist nicht privat.

Betrachten wir als Beispiel die folgende Klassendefinition:

.. code-block:: python

    >>> class MyClass:
    ...     def __init__(self):
    ...         self.x = 1
    ...         self.__y = 2
    ...     def print_y(self):
    ...         print(self.__y)
    ...
    >>> m = MyClass()
    >>> print(m.x)
    1
    >>> print(m.__y)
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    AttributeError: 'MyClass' object has no attribute '__y'

Die ``print_y``-Methode ist nicht privat, und da sie sich in der
``MyClass``-Klasse befindet, kann sie auf ``__y`` zugreifen und ausgeben:

.. code-block:: python

    >>> m.print_y()
    2

.. note::

    Der Mechanismus, der zur Gewährleistung der Privatsphäre verwendet wird,
    verfälscht den Namen privater Variablen und privater Methoden, wenn der Code
    zu Bytecode kompiliert wird. Konkret bedeutet dies, dass
    :samp:`_{ClassName}` dem Variablennamen vorangestellt wird:

    .. code-block:: python

        >>> dir(m)
        ['_MyClass__y', '__class__', …]

    Damit soll also lediglich ein versehentlicher Zugriff verhindert werden.
