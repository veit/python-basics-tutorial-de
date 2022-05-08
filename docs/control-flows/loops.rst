Schleifen
=========

``while``-Schleife
------------------

Die ``while``-Schleife wird so lange ausgeführt, wie die Bedingung (hier: ``x >
y``) wahr ist:

.. code-block:: python
    :linenos:

    >>> x, y = 6, 3
    >>> while x > y:
    ...     x -= 1
    ...     if x == 4:
    ...         break
    ...     print(x)
    ... 
    5

Zeile 1
    Dies ist eine Kurzschreibweise, wobei ``x`` den Wert ``6`` und ``y`` den
    Wert ``3`` erhält.
Zeilen 2–10
    Dies ist die ``while``-Schleife mit der Anweisung ``x > y``, die wahr ist,
    solange ``x`` größer als ``y`` ist.
Zeile 3
    ``x`` wird um ``1`` reduziert
Zeile 4
    ``if``-Bedingung, bei der ``x`` exakt ``4`` sein soll.
Zeile 5
    ``break`` beendet die Schleife.
Zeilen 8 und 9
    gibt die Ergebnisse der ``while``-Schleife aus bevor die Ausführung mit
    ``break`` unterbrochen wurde.

.. code-block:: python
    :linenos:

    >>> x, y = 6, 3
    >>> while x > y:
    ...     x -= 1
    ...     if x == 4:
    ...         continue
    ...     print(x)
    ... 
    5
    3

Zeile 5
    ``continue`` bricht die aktuelle Iteration der Schleife ab.

``for``-Schleife
----------------

Die ``for``-Schleife ist einfach, aber mächtig, weil sie über einen beliebigen
iterierbaren Typ, wie eine Liste oder ein Tupel, iterieren kann. Anders als in
vielen anderen Sprachen iteriert die ``for``-Schleife in Python über jedes
Element in einer Sequenz (:abbr:`z.B. (zum Beispiel)  eine :doc:`Liste <lists>`
oder ein :doc:`../types/tuples`), was sie eher zu einer ``foreach``-Schleife
macht. Die folgende Schleife findet das erste Vorkommen einer ganzen Zahl, die
durch ``5`` teilbar ist:

.. code-block:: python
    :linenos:

    >>> items = [1, "fünf", 5.0, 10, 11, 15]
    >>> d = 5
    >>> for i in items:
    ...     if not isinstance(i, int):
    ...         continue
    ...     if not i % d:
    ...         print(f"Erste gefundene Ganzzahl, die durch {d} teilbar ist: {i}")
    ...         break
    ... 
    Erste gefundene Ganzzahl, die durch 5 teilbar ist: 10

``x`` wird nacheinander jeder Wert in der Liste zugewiesen. Wenn ``x`` keine
ganze Zahl ist, wird der Rest dieser Iteration durch die ``continue``-Anweisung
abgebrochen. Die Ablaufsteuerung wird fortgesetzt, wobei ``x`` auf den nächsten
Eintrag in der Liste gesetzt wird. Nachdem die erste passende ganze Zahl
gefunden wurde, wird die Schleife mit der ``break``-Anweisung beendet.
