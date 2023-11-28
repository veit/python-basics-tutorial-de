Listen
======

Python hat einen mächtigen eingebauten Listentyp:

.. code-block:: python
   :linenos:

    []
    [1]
    [1, "2.", 3.0, ["4a", "4b"], (5.1,5.2)]

Eine Liste kann eine Mischung anderer Typen als Elemente enthalten, darunter
Zeichenketten, Tupel, Listen, Dictionaries, Funktionen, Dateiobjekte und jede
Art von Zahl.

Eine Liste kann von vorne oder hinten indiziert werden. Ihr könnt euch auch auf
ein Teilsegment einer Liste beziehen, indem ihr die Slice-Notation verwendet:

.. code-block:: python
   :linenos:

    >>> x = [1, "2.", 3.0, ["4a", "4b"], (5.1,5.2)]
    >>> x[0]
    '1'
    >>> x[1]
    '2.'
    >>> x[-1]
    (5.1, 5.2)
    >>> x[-2]
    ['4a', '4b']
    >>> x[1:-1]
    ['2.', 3.0, ['4a', '4b']]
    >>> x[0:3]
    [1, '2.', 3.0]
    >>> x[:3]
    [1, '2.', 3.0]
    >>> x[-4:-1]
    ['2.', 3.0, ['4a', '4b']]
    >>> x[-4:]
    ['2.', 3.0, ['4a', '4b'], (5.1, 5.2)]

Zeilen 2 und 4
    Index von vorne unter Verwendung positiver Indizes beginnend mit ``0`` als
    erstem Element.
Zeilen 6 und 8
    Index von hinten unter Verwendung negativer Indizes beginnend mit ``-1`` als
    letztem Element.
Zeilen 10 und 12
    Slice mit ``[m:n]``, wobei ``m`` der inklusive Startpunkt und ``n`` der
    exklusive Endpunkt ist.
Zeilen 14, 16 und 18
    Ein ``[:n]``-Slice beginnt am Anfang und ein ``[m:]``-Slice geht bis zum
    Ende einer Liste.

Ihr könnt diese Notation verwenden, um Elemente in einer Liste hinzuzufügen, zu
entfernen und zu ersetzen oder um ein Element oder eine neue Liste zu erhalten, die ein Slice davon ist, :abbr:`z.B. (zum Beispiel)`:

.. code-block:: python
   :linenos:

    >>> x = [1, "2.", 3.0, ["4a", "4b"], (5.1,5.2)]
    >>> x[1] = "zweitens"
    >>> x[2:3] = []
    >>> x
    [1, 'zweitens', ['4a', '4b'], (5.1, 5.2)]
    >>> x[2] = [3.1, 3.2, 3.3]
    >>> x
    [1, 'zweitens', [3.1, 3.2, 3.3], (5.1, 5.2)]
    >>> x[2:]
    [[3.1, 3.2, 3.3], (5.1, 5.2)]

Zeile 3
    Die Größe der Liste erhöht oder verringert sich, wenn das neue Slice größer
    oder kleiner ist als das Slice, das es ersetzt.

Slices erlauben auch eine stufenweise Auswahl zwischen den Start- und
Endindizes. Der Standardwert für ein nicht spezifiziertes Stride ist ``1``,
womit jedes Element aus einer Sequenz zwischen den Indizes genommen wird. Bei
einem Stride von ``2`` wird jedes zweite Element übernommen :abbr:`usw. (und so
weiter)`:

.. code-block:: python
   :linenos:

   >>> x[0:3:2]
   [1, [3.1, 3.2, 3.3]]
   >>> x[::2]
   [1, [3.1, 3.2, 3.3]]
   >>> x[1::2]
   ['zweitens', (5.1, 5.2)]

Der Stride-Wert kann auch negativ sein. Ein ``-1``-Stride bedeutet, von rechts
nach links gezählt wird:

.. code-block:: python
   :linenos:

   >>> x[3:0:-2]
   [(5.1, 5.2), 'zweitens']
   >>> x[::-2]
   [(5.1, 5.2), 'zweitens']
   >>> x[::-1]
   [(5.1, 5.2), [3.1, 3.2, 3.3], 'zweitens', 1]

Zeile 1
    Um eine negative Schrittweite zu verwenden, sollte das Start-Slice größer
    sein als das End-Slice.
Zeile 3
    Die Ausnahme ist, wenn ihr die Start- und Endindizes weglasst.
Zeile 5
    Ein Stride von ``-1`` kehrt die Reihenfolge um.

Einige Funktionen der Slice-Notation können auch mit speziellen Operationen wausgeführt werden, wodurch die Lesbarkeit des Codes verbessert wird:

.. code-block:: python
   :linenos:

   >>> x.reverse()
   >>> x
   [(5.1, 5.2), [3.1, 3.2, 3.3], 'zweitens', 1]

Darüberhinaus könnt ihr die fogenden eingebauten Funktionen (``len``, ``max``
und ``min``), einige Operatoren (``in``, ``+`` und ``*``), die ``del``-Anweisung
und die Listenmethoden (``append``, ``count``, ``extend``, ``index``,
``insert``, ``pop``, ``remove``, ``reverse`` und ``sort``) für Listen verwenden:

.. code-block:: python
   :linenos:

    >>> len(x)
    4
    >>> x + [0, -1]
    [(5.1, 5.2), [3.1, 3.2, 3.3], 'zweitens', 1, 0, -1]
    >>> x.reverse()
    >>> x
    [1, 'zweitens', [3.1, 3.2, 3.3], (5.1, 5.2)]

Zeile 3
    Die Operatoren ``+`` und ``*`` erzeugen jeweils eine neue Liste, wobei die
    ursprüngliche Liste unverändert bleibt.
Zeile 5
    Die Methoden einer Liste werden mit Hilfe der Attributschreibweise für die
    Liste selbst aufgerufen: :samp:`{LISTE}.{METHODE}({ARGUMENTE})`.

.. seealso::
   * :doc:`Daten auswählen und filtern mit pandas
     <Python4DataScience:workspace/pandas/select-filter>`

Zusammenfassung
---------------

+---------------+---------------+---------------+---------------+---------------+
| Datentyp      | veränderlich  | geordnet      | indiziert     | Duplikate     |
+===============+===============+===============+===============+===============+
| Liste         | ✅            | ✅            | ✅            | ✅            |
+---------------+---------------+---------------+---------------+---------------+
