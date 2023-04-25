Funktionen
==========

Python bietet flexible Mechanismen zur Übergabe von Argumenten an Funktionen:

.. code-block:: python
    :linenos:

    >>> x, y = 2, 3
    >>> def func1(u, v, w):
    ...     value = u + 2*v + w**2
    ...     if value > 0:
    ...         return u + 2*v + w**2
    ...     else:
    ...         return 0
    ...
    >>> func1(x, y, 2)
    12
    >>> func1(x, w=y, v=2)
    15
    >>> def func2(u, v=1, w=1):
    ...     return u + 4 * v + w ** 2
    ...
    >>> func2(5, w=6)
    45
    >>> def func3(u, v=1, w=1, *tup):
    ...     print((u, v, w) + tup)
    ...
    >>> func3(7)
    (7, 1, 1)
    >>> func3(1, 2, 3, 4, 5)
    (1, 2, 3, 4, 5)
    >>> def func4(u, v=1, w=1, **kwargs):
    ...     print(u, v, w, kwargs)
    ...
    >>> func4(1, 2, s=4, t=5, w=3)
    1 2 3 {'s': 4, 't': 5}

Zeile 2
    Funktionen werden mit Hilfe der ``def``-Anweisung definiert.
Zeile 5
    Die ``return``-Anweisung wird von einer Funktion verwendet, um einen Wert
    urückzugeben. Dieser Wert kann von beliebigem Typ sein. Wird keine
    ``return``-Anweisung gefunden, wird der Wert ``None`` von Python
    zurückgegeben.
Zeile 11
    Funktionsargumente können entweder nach Position oder nach Name
    (Schlüsselwort) eingegeben werden. ``z`` und ``y`` werden in unserem
    Beispiel mit dem Namen angegeben.
Zeile 13
    Funktionsparameter können mit Standardwerten definiert werden, die
    verwendet werden, wenn ein Funktionsaufruf sie auslässt.
Zeile 18
    Es kann ein spezieller Parameter definiert werden, der alle zusätzlichen
    Positionsargumente in einem Funktionsaufruf in einem Tupel zusammenfasst.
Zeile 25
    Ebenso kann ein spezieller Parameter definiert werden, der alle
    zusätzlichen Schlüsselwortargumente in einem Funktionsaufruf in einem
    Dictionary zusammenfasst.
