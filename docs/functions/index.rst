Funktionen
==========

Die grundlegende Syntax für eine Python-Funktionsdefinition lautet

.. code-block:: python

    def function_name(param1, param2, ...):
        body

Wie bei :doc:`Kontrollströmen </control-flows/index>` verwendet Python
Einrückungen, um die Funktion von der Funktionsdefinition abzugrenzen. Das
folgende einfache Beispiel fügt den Code in eine Funktion ein, so dass ihr diese
aufrufen könnt, um die `Fakultät
<https://de.wikipedia.org/wiki/Fakult%C3%A4t_(Mathematik)>`_ einer Zahl zu
erhalten:

.. code-block:: python
   :linenos:

    >>> def fact(n):
    ...     """Return the factorial of the given number."""
    ...     f = 1
    ...     while n > 0:
    ...         f = f * n
    ...         n = n - 1
    ...     return f

Zeile 2
    Dies ist ein optionaler Dokumentationsstring, oder ``docstring``. Ihr könnt
    seinen Wert erhalten, indem ihr ``fact.__doc__`` aufruft. Der Zweck von
    Docstrings ist es, das Verhalten einer Funktion und die Parameter, die sie
    annimmt, zu beschreiben, während Kommentare interne Informationen über die
    Funktionsweise des Codes dokumentieren sollen. Docstrings sind
    :doc:`/types/strings`, die unmittelbar auf die erste Zeile einer
    Funktionsdefinition folgen und normalerweise in dreifachen Anführungszeichen
    stehen, um mehrzeilige Beschreibungen zu ermöglichen. Bei mehrzeiligen
    Dokumentationsstrings ist es üblich, in der ersten Zeile eine
    Zusammenfassung der Funktion zu geben, dieser Zusammenfassung eine leere
    Zeile folgen zu lassen und mit dem Rest der Informationen zu enden.

    .. seealso::
        * :doc:`../document/docstrings`

Zeile 7
    Der Wert wird nach dem Aufruf der Funktion zurückgegeben. Ihr könnt auch
    Funktionen schreiben, die keine Rückgabeanweisung haben und
    :doc:`/types/none` zurückgeben, und wenn ``return arg`` ausgeführt wird,
    wird der Wert ``arg`` zurückgegeben.

Obwohl alle Python-Funktionen Werte zurückgeben, liegt es an euch, wie der
Rückgabewert einer Funktion verwendet wird:

.. code-block:: python
   :linenos:

    >>> fact(3)
    6
    >>> x = fact(3)
    >>> x
    6

Zeile 1
    Der Rückgabewert ist nicht mit einer Variablen verknüpft.
Zeile 2
    Der Wert der ``fact``-Funktion wird nur im Interpreter ausgegeben.
Zeile 3
    Der Rückgabewert ist mit der Variablen ``x`` verknüpft.

Parameter
---------

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

.. toctree::
   :titlesonly:
   :hidden:

   params
   variables
   decorators
