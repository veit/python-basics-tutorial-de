Boolesche Werte und Ausdrücke
=============================

In Python gibt es mehrere Möglichkeiten, boolesche Werte auszudrücken; die
boolesche Konstante ``False``, ``0``, der Python-Typ :doc:`../types/none` und
leere Werte (:abbr:`z.B. (zum Beispiel)` die leere Liste ``[]`` oder die leere
Zeichenkette ``""``) werden alle als ``False`` betrachtet. Die boolesche
Konstante ``True`` und alles andere wird als ``True`` betrachtet.

``<``, ``<=``, ``==``, ``>``, ``>=``
    vergleicht Werte:

    .. code-block:: pycon

       >>> x = 3
       >>> y = 3.0
       >>> z = [3, 4, 5]
       >>> x == y
       True

    Ihr solltet jedoch nie berechnete Fließkommazahlen miteinander vergleichen:

    .. code-block:: pycon

       >>> u = 0.6 * 7
       >>> v = 0.7 * 6
       >>> u == v
       False
       >>> u
       4.2
       >>> v
       4.199999999999999

``is``, ``is not``, ``in``, ``not in``
    überprüft die Identität:

    .. code-block:: pycon

       >>> x is y
       False
       >>> x is not y
       True
       >>> x in z
       True
       >>> id(x)
       4375911432
       >>> id(y)
       4367574480
       >>> id(z[0])
       4375911432

    Wenn ``x`` und ``z[0]`` die gleiche ID im Speicher haben, bedeutet das, dass
    wir an zwei Stellen auf dasselbe Objekt verweisen.

    Am häufigsten werden ``is`` und ``is not`` in Verbindung mit
    :doc:`../types/none` verwendet:

    .. code-block:: pycon

       >>> x is None
       False
       >>> x is not None
       True

    Der Python-Style-Guide in :pep:`8` besagt, dass ihr Identität verwenden
    solltet, um mit :doc:`../types/none` zu vergleichen. Ihr solltet also
    niemals ``x == None``, sondern stattdessen ``x is None``  verwenden.

``and``, ``not``, ``or``
    sind logische Operatoren, mit denen wir die oben genannten Überprüfungen
    verknüpfen können:

    .. code-block:: pycon

       >>> x is y and x is z[0]
       False
       >>> x is y or x is z[0]
       True
       >>> x is y and not x is z[0]
       False
       >>> x is z[0] and not x is y
       True
