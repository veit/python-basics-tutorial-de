Variablen
=========

.. _local_variables:

Lokale Variablen
----------------

Hier kehren wir zur Definition von ``fact`` vom Anfang dieses
:doc:`index`-Kapitels zurück:

.. code-block:: python

    def fact(n):
        """Return the factorial of the given number."""
        f = 1
        while n > 0:
            f = f * n
            n = n - 1
        return f

Sowohl die Variablen ``f`` als auch ``n`` sind lokal für einen bestimmten Aufruf
der Funktion ``fact``; Änderungen an ihnen, die während der Ausführung der
Funktion vorgenommen werden, haben keine Auswirkungen auf Variablen außerhalb
der Funktion. Alle Variablen in der Parameter-Liste einer Funktion und alle
Variablen, die innerhalb einer Funktion durch eine Zuweisung erzeugt werden, wie
:abbr:`z.B. (zum Beispiel)` ``f = 1``, sind für die Funktion lokal:

.. code-block:: pycon

   >>> fact(3)
   6
   >>> f
   Traceback (most recent call last):
     File "<python-input-27>", line 1, in <module>
       f
   NameError: name 'f' is not defined
   >>> n
   Traceback (most recent call last):
     File "<python-input-28>", line 1, in <module>
       n
   NameError: name 'n' is not defined

.. _global_variables:

Globale Variablen
-----------------

Ihr könnt eine Variable explizit zu einer globalen Variable machen, indem ihr
sie mit der :ref:`global <python3:global>`-Anweisung deklariert, bevor sie
verwendet wird. Globale Variablen können von der Funktion angesprochen und
geändert werden. Sie existieren außerhalb der Funktion und können auch von
anderen Funktionen, die sie als global deklarieren, oder von Code, der sich
nicht innerhalb einer Funktion befindet, aufgerufen und geändert werden. Hier
ein Beispiel, das den Unterschied zwischen lokalen und globalen Variablen
verdeutlicht:

.. code-block:: python

    def my_func():
        global x
        x = 1
        y = 2

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

.. _nonlocal_variables:

Nicht-lokale Variablen
----------------------

Während :ref:`global <python3:global>` für eine Variable der obersten Ebene
verwendet wird, bezieht sich :ref:`nonlocal <python3:nonlocal>` auf jede
Variable in einem umschließenden Bereich:

.. code-block:: python

   def enclosing():
       x = "Enclosing function variable"

       def enclosed():
           nonlocal x
           x = "Enclosed function variable"

       enclosed()
       print(x)

.. code-block:: pycon

   >>> enclosing()
   Enclosed function variable

.. seealso::

    * :pep:`3104`

Checks
------

* Angenommen, ``x = 1``, :func:`func` setze die lokale Variable ``x`` auf ``2``
  und :func:`gfunc` die globale Variable ``x`` auf ``3``, welchen Wert nimmt
  ``x`` an, nachdem :func:`func` und :func:`gfunc` durchlaufen wurden?
