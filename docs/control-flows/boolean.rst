Boolesche Werte und Ausdrücke
=============================

In Python gibt es mehrere Möglichkeiten, boolesche Werte auszudrücken; die
boolesche Konstante ``False``, ``0``, der Python-Wert ``None`` und leere Werte
(:abbr:`z.B. (zum Beispiel)` die leere Liste ``[]`` oder die leere Zeichenkette
``""``) werden alle als ``False`` betrachtet. Die boolesche Konstante ``True``
und alles andere wird als ``True`` betrachtet.

Ihr könnt Vergleichsausdrücke erstellen, indem ihr die Vergleichsoperatoren
(``<``, ``<=``, ``==``, ``>``, ``>=``, ``!=``, ``is``, ``is not``, ``in``, ``not
in``) und die logischen Operatoren (``and``, ``not``, ``or``) verwendet, die
alle ``True`` oder ``False`` zurückgeben:

.. code-block:: python

    >>> x = 3
    >>> y = 3.0
    >>> z = [3, 4, 5]
    >>> x == y
    True
    >>> x is y
    False
    >>> x is not y
    True
    >>> x in z
    True

Ihr solltet jedoch nie berechnete Fließkommazahlen miteinander vergleichen:

.. code-block:: python

    >>> u = 0.6 * 7
    >>> v = 0.7 * 6
    >>> u == v
    False
    >>> u
    4.2
    >>> v
    4.199999999999999
