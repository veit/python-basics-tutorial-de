Schleifen
=========

``while``-Schleife
------------------

Die ``while``-Schleife wird so lange ausgeführt, wie die Bedingung (hier: ``x >
y``) wahr ist:

.. code-block:: pycon
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

.. code-block:: pycon
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

.. _for-loop:

``for``-Schleife
----------------

Die ``for``-Schleife ist einfach, aber mächtig, weil sie über einen beliebigen
iterierbaren Typ, wie eine Liste oder ein Tupel, iterieren kann. Anders als in
vielen anderen Sprachen iteriert die ``for``-Schleife in Python über jedes
Element in einer Sequenz (:abbr:`z.B. (zum Beispiel)` eine :doc:`Liste
<../types/sequences-sets/lists>` oder ein :doc:`../types/sequences-sets/tuples`),
was sie eher zu einer *for each*-Schleife macht. Die folgende Schleife verwendet
den `Modulo <https://de.wikipedia.org/wiki/Division_mit_Rest#Modulo>`_-Operator
``%`` als Bedingung für das erste Vorkommen einer ganzen Zahl, die durch ``5``
teilbar ist:

.. code-block:: pycon

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

Schleifen mit einem Index
-------------------------

Ihr könnt in einer ``for``-Schleife auch den Index ausgeben, :abbr:`z.B. (zum
Beispiel)` mit :py:func:`enumerate`:

.. code-block:: pycon

   >>> data_types = ["Data types", "Numbers", "Lists"]
   >>> for index, title in enumerate(data_types):
   ...     print(index, title)
   ...
   0 Data types
   1 Numbers
   2 Lists

List Comprehensions
-------------------

Üblicherweise wird eine Liste folgendermaßen generiert:

.. code-block:: pycon

   >>> squares = []
   >>> for i in range(8):
   ...     squares.append(i**2)
   ...
   >>> squares
   [0, 1, 4, 9, 16, 25, 36, 49]

Anstatt eine leere Liste zu erstellen und jedes Element am Ende einzufügen,
definiert ihr mit List Comprehensions einfach die Liste und ihren Inhalt
gleichzeitig mit nur einer einzigen Code-Zeile:

.. code-block:: pycon

   >>> squares = [i**2 for i in range(8)]
   >>> squares
   [0, 1, 4, 9, 16, 25, 36, 49]

Das allgemeine Format hierfür ist:

:samp:`{NEW_LIST} = [{EXPRESSION} for {MEMBER} in {ITERABLE}]`

Jede List Comprehension in Python enthält drei Elemente:

:samp:`{EXPRESSION}`
    ist ein Aufruf einer Methode oder ein anderer gültiger Ausdruck, der einen
    Wert zurückgibt. Im obigen Beispiel ist der Ausdruck ``i ** 2`` das Quadrat
    des jeweiligen Mitgliedswertes.
:samp:`{MEMBER}`
    ist das Objekt oder der Wert in einem :samp:`{ITERABLE}`. Im obigen Beispiel
    ist der Wert ``i``.
:samp:`{ITERABLE}`
    ist eine :doc:`Liste <../types/sequences-sets/lists>`, ein :doc:`Set
    <../types/sequences-sets/sets>`, ein Generator oder ein anderes Objekt, das
    seine Elemente einzeln zurückgeben kann. Im obigen Beispiel ist die Iterable
    ``range(8)``.

Ihr könnt mit List Comprehensions auch optional Bedingungen verwenden, die
üblicherweise am Ende des Ausdruck angehängt werden:

.. code-block:: pycon

   >>> squares = [i**2 for i in range(8) if i >= 4]
   >>> squares
   [16, 25, 36, 49]

Checks
------

* Entfernt aus der Liste ``x = [ -2, -1, 0, 1, 2, 3]``, alle negativen Zahlen.

* Welche List-Comprehension würdet ihr verwenden, um zum selben Ergebnis zu
  kommen?

* Wie würdet ihr die Gesamtzahl der negativen Zahlen in der Liste ``[[-1, 0, 1],
  [-1, 1, 3], [-2, 0, 2]]`` zählen?

* Erstellt einen Generator, der nur ungerade Zahlen von 1 bis 10 liefert.

  .. tip::
     Eine Zahl ist ungerade, wenn bei der Division durch 2 ein Rest übrig
     bleibt; also wenn ``% 2`` wahr ist.

* Schreibt ein :doc:`Dict </types/dicts>` mit den Kantenlängen und Volumen von
  Würfeln.
