Parameter
=========

Python bietet flexible Mechanismen zur Übergabe von Argumenten an Funktionen:

.. code-block:: pycon
   :linenos:

   >>> x, y = 2, 3
   >>> def func1(u, v, w):
   ...     value = u + 2 * v + w**2
   ...     if value > 0:
   ...         return u + 2 * v + w**2
   ...     else:
   ...         return 0
   ...
   >>> func1(x, y, 2)
   12
   >>> func1(x, w=y, v=2)
   15
   >>> def func2(u, v=1, w=1):
   ...     return u + 4 * v + w**2
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
    zurückzugeben. Dieser Wert kann von beliebigem Typ sein. Wird keine
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

Optionen für Funktionsparameter
-------------------------------

Die meisten Funktionen benötigen Parameter. Dabei bietet Python drei Optionen
für die Definition von Funktionsparametern.

Positionsbezogene Parameter
~~~~~~~~~~~~~~~~~~~~~~~~~~~

Die einfachste Art, Parameter an eine Funktion in Python zu übergeben, ist die
Übergabe an der Position. In der ersten Zeile der Funktion gebt ihr den
Variablennamen für jeden Parameter an; wenn die Funktion aufgerufen wird, werden
die im aufrufenden Code verwendeten Parameter den Parameter-Variablen der
Funktion auf der Grundlage ihrer Reihenfolge zugeordnet. Die folgende Funktion
berechnet ``x`` als Potenz von ``y``:

.. code-block:: pycon

    >>> def power(x, y):
    ...     p = 1
    ...     while y > 0:
    ...         p = p * x
    ...         y = y - 1
    ...     return p
    ...
    >>> power(2, 5)
    32

Diese Methode setzt voraus, dass die Anzahl der vom aufrufenden Code verwendeten
Parameter genau mit der Anzahl der Parameter in der Funktionsdefinition
übereinstimmt; andernfalls wird eine Type-Error-Exception ausgelöst:

.. code-block:: pycon

    >>> power(2)
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    TypeError: power() missing 1 required positional argument: 'y'

Funktionsparameter können Standardwerte haben, die ihr deklarieren könnt, indem
ihr in der ersten Zeile der Funktionsdefinition einen Standardwert zuweist, etwa
so:

.. code-block:: python

    def function_name(param1, param2=Standardwert2, param3=Standardwert3):
        pass

Es können beliebig viele Parameter mit Standardwerten versehen werden wobei
Parameter mit Standardwerten als letzte in der Parameter-Liste definiert werden
müssen.

Die folgende Funktion berechnet ``x`` ebenfalls als Potenz von ``y``. Wenn ``y``
jedoch nicht in einem Funktionsaufruf angegeben wird, wird der Standardwert
``5`` verwendet:

.. code-block:: pycon

    >>> def power(x, y=5):
    ...     p = 1
    ...     while y > 0:
    ...         p = p * x
    ...         y = y - 1
    ...     return p
    ...

Wie sich das Standardargument auswirkt, können ihr im folgenden Beispiel sehen:

.. code-block:: pycon

    >>> power(3, 6)
    729
    >>> power(3)
    243

Parameternamen
~~~~~~~~~~~~~~

ihr könnt auch Argumente an eine Funktion übergeben, indem ihr den Namen des
entsprechenden Funktionsparameters und nicht dessen Position verwendet. Ähnlich
dem vorherigen Beispiels könnt ihr Folgendes eingeben:

.. code-block:: pycon

    >>> power(y=6, x=2)
    64

Da die Argumente für die Potenz im letzten Aufruf mit ``x`` und ``y`` benannt
sind, ist ihre Reihenfolge irrelevant; die Argumente sind mit den gleichnamigen
Parametern in der Definition der Potenz verknüpft, und man erhält ``2^6``
zurück. Diese Art der Argument-Übergabe wird als Schlüsselwort-Übergabe
bezeichnet. Die Übergabe von Schlüsselwörtern kann in Kombination mit den
Standardargumenten von Python-Funktionen sehr nützlich sein, wenn ihr Funktionen
mit einer großen Anzahl von möglichen Argumenten definiert, von denen die
meisten gemeinsame Standardwerte haben.

Variable Anzahl von Argumenten
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Python-Funktionen können auch so definiert werden, dass sie mit einer variablen
Anzahl von Argumenten umgehen können. Dies ist auf zweierlei Arten möglich. Die
eine Methode sammelt eine unbekannte Anzahl von Argumenten in einer :doc:`Liste
</types/lists>`. Die andere Methode kann eine beliebige Anzahl von Argumenten,
die mit einem Schlüsselwort übergeben wurde und die keinen entsprechend
benannten Parameter in der Liste der Funktionsparameter hat, in einem
:doc:`Dict </types/dicts>` sammeln.

Bei einer unbestimmten Anzahl von Positionsargumenten bewirkt das Voranstellen
eines ``*`` vor den endgültigen Parameter-Namen der Funktion, dass alle
überschüssigen Nicht-Schlüsselwort-Argumente in einem Funktionsaufruf,
:abbr:`d.h. (das heißt)` die Positionsargumente, die keinem anderen Parameter
zugewiesen sind, gesammelt und als Tupel dem angegebenen Parameter zugewiesen
werden. Dies ist :abbr:`z.B. (zum Beispiel)` eine einfache Möglichkeit, eine
Funktion zu implementieren, die den Mittelwert in einer Liste von Zahlen findet:

.. code-block:: pycon

    >>> def mean(*numbers):
    ...     if len(numbers) == 0:
    ...         return None
    ...     else:
    ...         m = sum(numbers) / len(numbers)
    ...     return m
    ...

Nun könnt ihr das Verhalten der Funktion testen, :abbr:`z.B. (zum Beispiel)`
mit:

.. code-block:: pycon

    >>> mean(3, 5, 2, 4, 6)
    4.0

Eine beliebige Anzahl von Schlüsselwort-Argumenten kann ebenfalls verarbeitet
werden, wenn dem letzten Parameter in der Parameterliste das Präfix ``**``
vorangestellt ist. Dann werden alle Argumente, die mit einem Schlüsselwort
übergeben wurden, in einem :doc:`Dict </types/dicts>` gesammelt. Der Schlüssel
für jeden Eintrag im Dict ist das Schlüsselwort (Parametername) für das
Argument. Der Wert dieses Eintrags ist das Argument selbst. Ein per
Schlüsselwort übergebenes Argument ist in diesem Zusammenhang überflüssig, wenn
das Schlüsselwort, mit dem es übergeben wurde, nicht mit einem der
Parameternamen in der Funktionsdefinition übereinstimmt, :abbr:`z.B. (zum
Beispiel)`:

.. code-block:: pycon

   >>> def server(ip, port, **other):
   ...     print(f"ip: {ip}, port: {port}, other: {other}")
   ...     total = 0
   ...     for k in other.keys():
   ...         total = total + other[k]
   ...     print(f"The sum of the other values is {total}")
   ...

Das Ausprobieren dieser Funktion zeigt, dass sie  die Argumente addieren kann,
die unter den Schlüsselwörtern ``foo``,  ``bar`` und ``baz`` übergeben werden,
obwohl ``foo``,  ``bar`` und ``baz`` in der Funktionsdefinition keine
Parameternamen sind:

.. code-block:: pycon

   >>> server("127.0.0.1", port="8080", foo=3, bar=5, baz=2)
   ip: 127.0.0.1, port: 8080, other: {'foo': 3, 'bar': 5, 'baz': 2}
   The sum of the other values is 10

Techniken zur Argument-Übergabe mischen
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Ihr könnt alle Möglichkeiten zur Argument-Übergabe von Python-Funktionen
gleichzeitig zu verwenden, obwohl dies verwirrend sein kann, wenn ihr es nicht
sorgfältig macht. Dabei sollten  die Positionsargumente an erster Stelle stehen,
dann benannte Argumente, gefolgt von unbestimmten Positionsargumenten mit einem
einfachen ``*`` und zuletzt unbestimmte Schlüsselwortargumente mit ``**``.

Veränderliche Objekte als Argumente
-----------------------------------

Argumente werden per Objektreferenz übergeben. Der Parameter wird zu einem neuen
Verweis auf das Objekt. Bei unveränderlichen Objekten wie :doc:`/types/tuples`,
:doc:`/types/strings/index` und :doc:`/types/numbers` hat das, was mit einem
Parameter gemacht wird, keine Auswirkungen außerhalb der Funktion. Wenn ihr
jedoch ein veränderliches Objekt übergeben, :abbr:`z.B. (zum Beispiel)` eine
:doc:`Liste </types/lists>`, ein :doc:`Dict </types/dicts>` oder eine
Klasseninstanz, ändert jede Änderung des Objekts, worauf das Argument außerhalb
der Funktion verweist. Die Neuzuweisung des Parameters hat keine Auswirkungen
auf das Argument.

.. code-block:: pycon

    >>> def my_func(n, l):
    ...     l.append(1)
    ...     n = n + 1
    ...
    >>> x = 5
    >>> y = [2, 4, 6]
    >>> my_func(x, y)
    >>> x, y
    (5, [2, 4, 6, 1])

Die Variable ``x`` wird nicht geändert, da sie unveränderlich ist. Stattdessen
wird der Funktionsparameter ``n`` so gesetzt, dass er auf den neuen Wert ``6``
verweist. Bei ``y`` gibt es jedoch eine Änderung, weil die Liste, auf die sie
verweist, geändert wurde.

Checks
------

* Schreibt eine Funktion, die eine beliebige Anzahl von unbenannten Argumenten
  annehmen und deren Werte in umgekehrter Reihenfolge ausgeben kann?
