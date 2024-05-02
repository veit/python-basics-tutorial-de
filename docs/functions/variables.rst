Variablen
=========

Lokale, nicht-lokale und globale Variablen
------------------------------------------

Hier kehren wir zur Definition von ``fact`` vom Anfang dieses
:doc:`index`-Kapitels zurück:

.. code-block:: pycon

    >>> def fact(n):
    ...     """Return the factorial of the given number."""
    ...     f = 1
    ...     while n > 0:
    ...         f = f * n
    ...         n = n - 1
    ...     return f
    ...

Sowohl die Variablen ``f`` als auch ``n`` sind lokal für einen bestimmten Aufruf
der Funktion ``fact``; Änderungen an ihnen, die während der Ausführung der
Funktion vorgenommen werden, haben keine Auswirkungen auf Variablen außerhalb
der Funktion. Alle Variablen in der Parameterliste einer Funktion und alle
Variablen, die innerhalb einer Funktion durch eine Zuweisung erzeugt werden, wie
:abbr:`z.B. (zum Beispiel)` ``f = 1``, sind für die Funktion lokal.

Ihr könnt eine Variable explizit zu einer globalen Variable machen, indem ihr
sie mit der ``global``-Anweisung deklariert, bevor sie verwendet wird. Globale
Variablen können von der Funktion angesprochen und geändert werden. Sie
existieren außerhalb der Funktion und können auch von anderen Funktionen, die
sie als global deklarieren, oder von Code, der sich nicht innerhalb einer
Funktion befindet, aufgerufen und geändert werden. Hier ein Beispiel, das den
Unterschied zwischen lokalen und globalen Variablen verdeutlicht:

.. code-block:: pycon

    >>> def my_func():
    ...     global x
    ...     x = 1
    ...     y = 2
    ...

.. code-block:: pycon

    >>> x = 3
    >>> y = 4
    >>> my_func()
    >>> x
    1
    >>> y
    4

In diesem Beispiel wird eine Funktion definiert, die ``x`` als globale Variable
und ``y`` als lokale Variable behandelt und versucht, sowohl ``x`` als auch
``y`` zu ändern. Die Zuweisung an ``x`` innerhalb von ``my_func`` ist eine
Zuweisung an die globale Variable ``x``, die auch außerhalb von ``my_func``
existiert. Da ``x`` in ``my_func`` als global bezeichnet wird, ändert die
Zuweisung diese globale Variable so, dass sie den Wert ``1`` anstelle des Wertes
``3`` beibehält. Dasselbe gilt jedoch nicht für ``y``; die lokale Variable ``y``
innerhalb von ``my_func`` verweist zunächst auf denselben Wert wie die Variable
``y`` außerhalb von ``my_func``, aber die Zuweisung bewirkt, dass ``y`` auf
einen neuen Wert verweist, der für die Funktion ``my_func`` lokal ist.


.. seealso::

    * :ref:`python3:global`

Während ``global`` für eine Variable der obersten Ebene verwendet wird, bezieht
sich ``nonlocal`` auf jede Variable in einem umschließenden Bereich.

.. seealso::

    * :ref:`python3:nonlocal`
    * :pep:`3104`
