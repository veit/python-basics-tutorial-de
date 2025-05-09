Funktionen
==========

Die grundlegende Syntax für eine Python-Funktionsdefinition lautet

.. code-block:: python

    def function_name(param1, param2):
        body

Wie bei :doc:`Kontrollströmen </control-flow/index>` verwendet Python
Einrückungen, um die Funktion von der Funktionsdefinition abzugrenzen. Das
folgende einfache Beispiel fügt den Code in eine Funktion ein, so dass ihr diese
aufrufen könnt, um die `Fakultät
<https://de.wikipedia.org/wiki/Fakult%C3%A4t_(Mathematik)>`_ einer Zahl zu
erhalten:

.. code-block:: python
   :linenos:

    def fact(n):
        """Return the factorial of the given number."""
        f = 1
        while n > 0:
            f = f * n
            n = n - 1
        return f

Zeile 2
    Dies ist ein optionaler Dokumentationsstring, oder ``docstring``. Ihr könnt
    seinen Wert erhalten, indem ihr ``fact.__doc__`` aufruft. Der Zweck von
    Docstrings ist es, das Verhalten einer Funktion und die Parameter, die sie
    annimmt, zu beschreiben, während Kommentare interne Informationen über die
    Funktionsweise des Codes dokumentieren sollen. Docstrings sind
    :doc:`/types/strings/index`, die unmittelbar auf die erste Zeile einer
    Funktionsdefinition folgen und normalerweise in dreifachen Anführungszeichen
    stehen, um mehrzeilige Beschreibungen zu ermöglichen. Bei mehrzeiligen
    Dokumentationsstrings ist es üblich, in der ersten Zeile eine
    Zusammenfassung der Funktion zu geben, dieser Zusammenfassung eine leere
    Zeile folgen zu lassen und mit dem Rest der Informationen zu enden.

    .. seealso::
        * :doc:`../document/sphinx/docstrings`
        * :pep:`257`

Zeile 7
    Der Wert wird nach dem Aufruf der Funktion zurückgegeben. Ihr könnt auch
    Funktionen schreiben, die keine Rückgabeanweisung haben und
    :doc:`/types/none` zurückgeben, und wenn ``return arg`` ausgeführt wird,
    wird der Wert ``arg`` zurückgegeben.

Obwohl alle Python-Funktionen Werte zurückgeben, liegt es an euch, wie der
Rückgabewert einer Funktion verwendet wird:

.. code-block:: pycon
   :linenos:

    >>> fact(3)
    6
    >>> x = fact(3)
    >>> x
    6

Zeile 1
    Der Rückgabewert ist nicht mit einer Variablen verknüpft.
Zeile 2
    Der Wert der :func:`fact`-Funktion wird nur im Interpreter ausgegeben.
Zeile 3
    Der Rückgabewert ist mit der Variablen ``x`` verknüpft.

Inspiriert durch :doc:`Python4DataScience:productive/qa/mypy` wurden in Python
:abbr:`sog. (sogenannte)` *Type Hints* eingeführt, womit die Typen für
:doc:`params` und Rückgabewerte definiert werden können, in unserem
:func:`fact`-Beispiel mit:

.. blacken-docs:off
.. code-block:: python

   def fact(n: int) -> int:
       ...
.. blacken-docs:on

oder:

.. blacken-docs:off
.. code-block:: python

   def factlist(flist: list[float]) -> list[float]:
       ...
.. blacken-docs:on

Wir erhalten zur Laufzeit die Typen mit dem ``__annotations__``-Attribut:

.. code-block:: pycon

   >>> fact.__annotations__
   {'n': <class 'int'>, 'return': <class 'int'>}
   >>> factlist.__annotations__
   {'list': list[float], 'return': list[float]}

Es findet jedoch zur Laufzeit **keine** Typ-Überprüfung statt.

.. seealso::
   * :pep:`484`
   * :doc:`python3:library/typing`

.. toctree::
   :titlesonly:
   :hidden:

   params
   variables
   decorators
   lambda
