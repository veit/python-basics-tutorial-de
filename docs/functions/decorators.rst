Dekoratoren
===========

Funktionen können auch als Argumente an andere Funktionen übergeben werden und
die Ergebnisse anderer Funktionen zurückgegeben. So ist es :abbr:`z.B. (zum
Beispiel)` möglich, eine Python-Funktion zu schreiben, die eine andere Funktion
als Parameter annimmt, sie in eine andere Funktion einbettet, die etwas
Ähnliches tut, und dann die neue Funktion zurückgibt. Diese neue Kombination
kann dann anstelle der ursprünglichen Funktion verwendet werden:

.. code-block:: pycon
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

.. code-block:: pycon
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

``functools``
-------------

Das Python-:mod:`functools`-Modul ist für Funktionen höherer Ordnung gedacht,
also Funktionen, die auf andere Funktionen wirken oder diese zurückgeben. Meist
könnt ihr sie als Dekoratoren verwenden, so :abbr:`u.a. (unter anderem)`:

:func:`functools.cache`
    Einfacher, leichtgewichtiger, Cache für Funktionen ab Python ≥ 3.9, der
    manchmal auch *memoize* genannt wird. Er gibt dasselbe zurück wie
    :func:`functools.lru_cache` mit dem Parameter ``maxsize=None``, wobei
    zusätzlich ein :doc:`/types/dicts` mit den Funktionsargumenten erstellt
    wird. Da alte Werte nie gelöscht werden müssen, ist diese Funktion dann
    auch kleiner und schneller. Ein Beispiel:

    .. code-block:: pycon
        :linenos:

        >>> from functools import cache
        >>> @cache
        ... def factorial(n):
        ...     return n * factorial(n - 1) if n else 1
        ...
        >>> factorial(8)
        40320
        >>> factorial(10)
        3628800

    Zeile 6
        Da es kein zuvor gespeichertes Ergebnis gibt, werden neun rekursive
        Aufrufe gemacht.
    Zeile 8
        macht nur zwei neue Aufrufe, da die anderen Ergebnisse aus dem
        Zwischenspeicher kommen.

:func:`functools.wraps`
    Dieser Dekorator lässt die Wrapper-Funktion so, so wie die ursprüngliche
    Funktion aussehen mit ihren Namen und ihren Eigenschaften.

    .. code-block:: pycon

        >>> from functools import wraps
        >>> def my_decorator(f):
        ...     @wraps(f)
        ...     def wrapper(*args, **kwargs):
        ...         """Wrapper docstring"""
        ...         print("Call decorated function")
        ...         return f(*args, **kwargs)
        ...     return wrapper
        ...
        >>> @my_decorator
        ... def example():
        ...     """Example docstring"""
        ...     print("Call example function")
        ...
        >>> example.__name__
        'example'
        >>> example.__doc__
        'Example docstring'

    Ohne ``@wraps``-Dekorator wäre stattdessen Name und Docstring der
    ``wrapper``-Methode zurückgegeben worden:

    .. code-block:: pycon

        >>> example.__name__
        'wrapper'
        >>> example.__doc__
        'Wrapper docstring'

.. tip::
   `cusy Seminar: Fortgeschrittenes Python
   <https://cusy.io/de/unsere-schulungsangebote/fortgeschrittenes-python>`_
