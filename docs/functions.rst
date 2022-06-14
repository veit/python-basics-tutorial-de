Funktionen
==========

Grundlegende Funktionsdefinitionen
----------------------------------

Die grundlegende Syntax für eine Python-Funktionsdefinition lautet

.. code-block:: python

    def function_name(param1, param2, ...):
        body

Wie bei :doc:`Kontrollströmen <control-flows/index>` verwendet Python
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
        * :ref:`napoleon`

Zeile 7
    Der Wert wird nach dem Aufruf der Funktion zurückgegeben. Ihr könnt auch
    Funktionen schreiben, die keine Rückgabeanweisung haben und
    :doc:`types/none` zurückgeben, und wenn ``return arg`` ausgeführt wird, wird
    der Wert ``arg`` zurückgegeben.

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

Optionen für Funktionsparameter
-------------------------------

Die meisten Funktionen benötigen Parameter. Dabei bietet Python drei Optionen
für die Definition von Funktionsparametern.

Positionsbezogene Parameter
~~~~~~~~~~~~~~~~~~~~~~~~~~~

Die einfachste Art, Parameter an eine Funktion in Python zu übergeben, ist die
Übergabe an der Position. In der ersten Zeile der Funktion gebt ihr den
Variablennamen für jeden Parameter an; wenn die Funktion aufgerufen wird, werden
die im aufrufenden Code verwendeten Parameter den Parametervariablen der
Funktion auf der Grundlage ihrer Reihenfolge zugeordnet. Die folgende Funktion
berechnet ``x`` als Potenz von ``y``:

.. code-block:: python

    >>> def power(x, y):
    ...     p = 1
    ...     while y > 0:
    ...             p = p * x
    ...             y = y - 1
    ...     return p
    ...
    >>> power(2, 5)
    32

Diese Methode setzt voraus, dass die Anzahl der vom aufrufenden Code verwendeten
Parameter genau mit der Anzahl der Parameter in der Funktionsdefinition
übereinstimmt; andernfalls wird eine Type-Error-Exception ausgelöst:

.. code-block:: python

    >>> power(2)
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    TypeError: power() missing 1 required positional argument: 'y'

Funktionsparameter können Standardwerte haben, die ihr deklarieren könnt, indem
ihr in der ersten Zeile der Funktionsdefinition einen Standardwert zuweist, etwa
so:

.. code-block:: python

    def function_name(param1, param2=Standardwert2, param3=Standardwert3, ...)

Es können beliebig viele Parameter mit Standardwerten versehen werden wobei 
Parameter mit Standardwerten als letzte in der Parameterliste definiert werden
müssen.

Die folgende Funktion berechnet ``x`` ebenfalls als Potenz von ``y``. Wenn ``y``
jedoch nicht in einem Funktionsaufruf angegeben wird, wird der Standardwert
``5`` verwendet:

.. code-block:: python

    >>> def power(x, y=5):
    ...     p = 1
    ...     while y > 0:
    ...             p = p * x
    ...             y = y - 1
    ...     return p

Wie sich das Standardargument auswirkt, können ihr im folgenden Beispiel sehen:

.. code-block:: python

    >>> power(3, 6)
    729
    >>> power(3)
    243

Parameternamen
~~~~~~~~~~~~~~

ihr könnt auch Argumente an eine Funktion übergeben, indem ihr den Namen des
entsprechenden Funktionsparameters und nicht dessen Position verwendet. Ähnlich
dem vorherigen Beispiels könnt ihr Folgendes eingeben:

.. code-block:: python

    >>> power(y=6, x=2)
    64

Da die Argumente für die Potenz im letzten Aufruf mit ``x`` und ``y`` benannt
sind, ist ihre Reihenfolge irrelevant; die Argumente sind mit den gleichnamigen
Parametern in der Definition der Potenz verknüpft, und man erhält ``2^6``
zurück. Diese Art der Argumentübergabe wird als Schlüsselwortübergabe
bezeichnet. Die Übergabe von Schlüsselwörtern kann in Kombination mit den
Standardargumenten von Python-Funktionen sehr nützlich sein, wenn ihr Funktionen
mit einer großen Anzahl von möglichen Argumenten definiert, von denen die
meisten gemeinsame Standardwerte haben.

Variable Anzahl von Argumenten
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Python-Funktionen können auch so definiert werden, dass sie mit einer variablen
Anzahl von Argumenten umgehen können. Dies ist auf zweierlei Arten möglich. Die
eine Methode sammelt eine unbekannte Anzahl von Argumenten in einer :doc:`Liste
<types/lists>`. Die andere Methode kann eine beliebige Anzahl von Argumenten,
die mit einem Schlüsselwort übergeben wurde und die keinen entsprechend
benannten Parameter in der Funktionsparameterliste hat, in einem :doc:`Dict
<types/dicts>` sammeln.

Bei einer unbestimmten Anzahl von Positionsargumenten bewirkt das Voranstellen
eines ``*`` vor den endgültigen Parameternamen der Funktion, dass alle
überschüssigen Nicht-Schlüsselwort-Argumente in einem Funktionsaufruf,
(:abbr:`d.h. (das heißt)` die Positionsargumente, die keinem anderen Parameter
zugewiesen sind, gesammelt und als Tupel dem angegebenen Parameter zugewiesen
werden. Dies ist :abbr:`z.B. (zum Beispiel)` eine einfache Möglichkeit, eine
Funktion zu implementieren, die den Mittelwert in einer Liste von Zahlen findet:

.. code-block:: python

    >>> def mean(*numbers):
    ...     if len(numbers) == 0:
    ...         return None
    ...     else:
    ...         m = sum(numbers) / len(numbers)
    ...     return m

Nun könnt ihr das Verhalten der Funktion testen, :abbr:`z.B. (zum Beispiel` mit:

.. code-block:: python

    >>> mean(3, 5, 2, 4, 6)
    4.0

Eine beliebige Anzahl von Schlüsselwortargumenten kann ebenfalls verarbeitet
werden, wenn dem letzten Parameter in der Parameterliste das Präfix ``**``
vorangestellt ist. Dann werden alle Argumente, die mit einem Schlüsselwort
übergeben wurden, in einem :doc:`Dict <types/dicts>` gesammelt. Der Schlüssel
für jeden Eintrag im Dict ist das Schlüsselwort (Parametername) für das
Argument. Der Wert dieses Eintrags ist das Argument selbst. Ein per
Schlüsselwort übergebenes Argument ist in diesem Zusammenhang überflüssig, wenn
das Schlüsselwort, mit dem es übergeben wurde, nicht mit einem der
Parameternamen in der Funktionsdefinition übereinstimmt, :abbr:`z.B. (zum
Beispiel)`:

.. code-block:: python

    >>> def server(ip, port, **other):
    ...     print("ip: {0}, port: {1}, keys in 'other': {2}".format(ip,
    ...           port, list(other.keys())))
    ...     total = 0
    ...     for k in other.keys():
    ...         total = total + other[k]
    ...     print("The sum of the other values is {0}".format(total))

Das Ausprobieren dieser Funktion zeigt, dass sie  die Argumente addieren kann,
die unter den Schlüsselwörtern ``foo``,  ``bar`` und ``baz`` übergeben werden,
obwohl ``foo``,  ``bar`` und ``baz`` in der Funktionsdefinition keine
Parameternamen sind:

.. code-block:: python

    >>> server("127.0.0.1", port = "8080", foo = 3, bar = 5, baz = 2)
    ip: 127.0.0.1, port: 8080, keys in 'other': ['foo', 'bar', 'baz']
    The sum of the other values is 10
