Dekoratoren
===========

Funktionen können auch als Argumente an andere Funktionen übergeben werden und
die Ergebnisse anderer Funktionen zurückgegeben. So ist es :abbr:`z.B. (zum
Beispiel)` möglich, eine Python-Funktion zu schreiben, die eine andere Funktion
als Parameter annimmt, sie in eine andere Funktion einbettet, die etwas
Ähnliches tut, und dann die neue Funktion zurückgibt. Diese neue Kombination
kann dann anstelle der ursprünglichen Funktion verwendet werden:

.. code-block:: python
   :linenos:

    >>> def inf(func):
    ...     print("Information about", func.__name__)
    ...     def details(*args):
    ...         print("Execute function", func.__name__, "with the argument(s)")
    ...         return func(*args)
    ...     return details
    ...
    >>> def my_func(*params):
    ...     print(params)
    ...
    >>> my_func = inf(my_func)
    Information about my_func
    >>> my_func("Hello", "Pythonistas!")
    Execute function my_func with the argument(s)
    ('Hello', 'Pythonistas!')

Zeile 2
    Die ``inf``-Funktion gibt den Namen der Funktion, die sie umhüllt, aus.
Zeile 6
    Wenn sie fertig ist, gibt die ``inf``-Funktion die umhüllte Funktion zurück.

Ein Dekorator ist `syntaktischer Zucker
<https://de.wikipedia.org/wiki/Syntaktischer_Zucker>`_ für diesen Prozess und
ermöglicht euch, eine Funktion mit einem einzeiligen Zusatz in eine andere zu
packen. Ihr erhaltet immer noch genau den gleichen Effekt wie beim vorherigen
Code, aber der resultierende Code ist viel sauberer und leichter zu lesen. Die
Verwendung eines Dekorators besteht ganz einfach aus zwei Teilen:

#. der Definition der Funktion, die andere Funktionen umhüllen oder
   *dekorieren* soll, und
#. der Verwendung eines ``@``, gefolgt von dem Dekorator, unmittelbar bevor die
   umhüllte Funktion definiert wird.

Die Dekorfunktion sollte eine Funktion als Parameter annehmen und eine Funktion
zurückgeben, wie folgt:

.. code-block:: python
   :linenos:

    >>> @inf
    ... def my_func(*params):
    ...     print(params)
    ...
    Information about my_func
    >>> my_func("Hello", "Pythonistas!")
    Execute function my_func with the argument(s)
    ('Hello', 'Pythonistas!')

Zeile 1
    Die Funktion ``my_func`` wird mit ``@inf`` dekoriert.
Zeile 7
    Die umhüllte Funktion wird aufgerufen, nachdem die Dekorator-Funktion fertig
    ist.
