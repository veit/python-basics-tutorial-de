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

    >>> x = 5
    >>> y = 3
    >>> z = [3, 4, 5]
    >>> x is y
    False
    >>> x is not y
    True
    >>> x in z
    True
