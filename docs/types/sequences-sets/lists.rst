Listen
======

Eine Liste in Python ist ähnlich wie ein Array in Java oder C: eine geordnete
Kollektion von Objekten. Anders als Listen in vielen anderen Sprachen können
Python-Listen jedoch verschiedene Arten von Elementen enthalten; ein
Listenelement kann ein beliebiges Python-Objekt sein, darunter
:doc:`../strings/index`, :doc:`tuples`, :doc:`lists`, :doc:`../dicts`,
:doc:`../../functions/index`, :doc:`../../save-data/files` und jede Art von
:doc:`../numbers/index`. Ihr erstellt eine Liste, indem ihr kein oder durch Komma
getrennte Elemente in eckige Klammern einschließt, etwa so:

.. code-block:: python
   :linenos:

    []
    [1]
    [1, "2.", 3.0, ["4a", "4b"], (5.1, 5.2)]

.. tip::
   Ich empfehle euch, **nicht** den in Python verfügbaren
   :class:`python3:ctypes.Array`-Typ zu verwenden, sondern, wenn es numerische
   Berechnungen erfordern, :doc:`Python4DataScience:workspace/numpy/index` in
   Betracht ziehen, das in unserem :doc:`Python4DataScience:index`-Tutorial
   beschrieben ist.

Indizes
-------

Elemente können aus einer Python-Liste extrahiert werden, indem eine Notation
verwendet wird, die der Array-Indizierung in C ähnelt und mit ``0`` beginnt; die
Frage nach Element ``0`` gibt das erste Element der Liste zurück, die Frage nach
Element ``1`` gibt das zweite Element zurück :abbr:`u.s.w. (und so weiter)`.
Hier sind ein paar Beispiele:

.. code-block:: pycon
   :linenos:

    >>> x = [1, "2.", 3.0, ["4a", "4b"], (5.1, 5.2)]
    >>> x[0]
    '1'
    >>> x[1]
    '2.'

Eine Liste kann von vorne oder hinten indiziert werden. Ihr könnt euch auch auf
ein Teilsegment einer Liste beziehen, indem ihr die *Slice*-Notation verwendet:

.. code-block:: pycon
   :lineno-start: 6

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
    *Slice* mit ``[m:n]``, wobei ``m`` der inklusive Startpunkt und ``n`` der
    exklusive Endpunkt ist.
Zeilen 14, 16 und 18
    Ein ``[:n]``-*Slice* beginnt am Anfang und ein ``[m:]``-*Slice* geht bis zum
    Ende einer Liste.

*Slices* erlauben auch eine stufenweise Auswahl zwischen den Start- und
Endindizes. Der Standardwert für ein nicht spezifiziertes *Stride* ist ``1``,
womit jedes Element aus einer Sequenz zwischen den Indizes genommen wird. Bei
einem *Stride* von ``2`` wird jedes zweite Element übernommen :abbr:`usw. (und
so weiter)`:

.. code-block:: pycon
   :linenos:

   >>> x[0:3:2]
   [1, [3.1, 3.2, 3.3]]
   >>> x[::2]
   [1, [3.1, 3.2, 3.3]]
   >>> x[1::2]
   ['zweitens', (5.1, 5.2)]

Der *Stride*-Wert kann auch negativ sein. Ein ``-1``-*Stride* bedeutet, dass von
rechts nach links gezählt wird:

.. code-block:: pycon
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
    Ein *Stride* von ``-1`` kehrt die Reihenfolge um.

    .. tip::
       Zum Umkehren der Reihenfolge dürfte jedoch :func:`list.reverse` besser
       lesbar sein als ein  *Stride* von ``-1``, :abbr:`s.a. (siehe auch)`
       :ref:`list.reverse() <reverse>`.

.. seealso::
   * :doc:`Daten auswählen und filtern mit pandas
     <Python4DataScience:workspace/pandas/select-filter>`

Ändern von Listen
-----------------

Ihr könnt diese Notation verwenden, um Elemente in einer Liste hinzuzufügen, zu
entfernen und zu ersetzen oder um ein Element oder eine neue Liste zu erhalten, die ein *Slice* davon ist, :abbr:`z.B. (zum Beispiel)`:

.. code-block:: pycon
   :linenos:

   >>> x = [1, "2.", 3.0, ["4a", "4b"], (5.1, 5.2)]
   >>> x[1] = "zweitens"
   >>> x
   [1, 'zweitens', 3.0, ['4a', '4b'], (5.1, 5.2)]
   >>> x[5:] = [6, 7]
   >>> x
   [1, 'zweitens', 3.0, ['4a', '4b'], (5.1, 5.2), 6, 7]
   >>> x[:0] = [-1, 0]
   >>> x
   [-1, 0, 1, 'zweitens', 3.0, ['4a', '4b'], (5.1, 5.2), 6, 7]
   >>> x[2:3] = []
   >>> x
   [-1, 0, 'zweitens', 3.0, ['4a', '4b'], (5.1, 5.2), 6, 7]

Zeile 2
    ersetzt das zweite Element der Liste.
Zeile 5
    fügt Elemente am Ende der Liste hinzu.
Zeile 8
    fügt Elemente am Anfang der Liste hinzu.
Zeile 11
    entfernt Elemente aus der Liste.

Einige Funktionen der Slice-Notation können auch mit speziellen Operationen
ausgeführt werden, wodurch die Lesbarkeit des Codes verbessert wird:

.. _reverse:

.. code-block:: pycon
   :linenos:

   >>> x.reverse()
   >>> x
   [(5.1, 5.2), [3.1, 3.2, 3.3], 'zweitens', 1]

Darüberhinaus könnt ihr die eingebauten Funktionen (:func:`python3:len`,
:func:`max` und :func:`min`), einige Operatoren (:ref:`in, not in <python3:in>`,
``+`` und ``*``), die ``del``-Anweisung und die Listenmethoden (``append``,
``count``, ``extend``, ``index``, ``insert``, ``pop``, ``remove``, ``reverse``,
:meth:`sort <python3:list.sort>` und ``sum``) für Listen verwenden:

.. code-block:: pycon
   :linenos:

   >>> len(x)
   4
   >>> x[len(x) :] = [0, -1]
   >>> x
   [(5.1, 5.2), [3.1, 3.2, 3.3], 'zweitens', 1, 0, -1]
   >>> x.append(-2)
   >>> x
   [(5.1, 5.2), [3.1, 3.2, 3.3], 'zweitens', 1, 0, -1, -2]
   >>> y = [-3, -4, -5]
   >>> x.append(y)
   >>> x
   [(5.1, 5.2), [3.1, 3.2, 3.3], 'zweitens', 1, 0, -1, -2, [-3, -4, -5]]
   >>> x[7:8] = []
   >>> x
   [(5.1, 5.2), [3.1, 3.2, 3.3], 'zweitens', 1, 0, -1, -2]
   >>> x.extend(y)
   >>> x
   [(5.1, 5.2), [3.1, 3.2, 3.3], 'zweitens', 1, 0, -1, -2, -3, -4, -5]
   >>> x + [-6, -7]
   [(5.1, 5.2), [3.1, 3.2, 3.3], 'zweitens', 1, 0, -1, -2, -3, -4, -5, -6, -7]
   >>> x.reverse()
   >>> x
   [-5, -4, -3, -2, -1, 0, 1, 'zweitens', [3.1, 3.2, 3.3], (5.1, 5.2)]

Zeile 1
    gibt die Anzahl der Listenelemente aus.
Zeile 3
    hängt an das Ende der Liste eine neue Liste an.
Zeile 6
    hängt mit ``append`` am Ende der Liste ein neues Element an.
Zeile 10
    hängt mit ``append`` ans Ende der Liste **nicht** die Elemente der
    ``y``-Liste an, sondern das Element ``y``-Liste.
Zeile 16
    hängt mit ``extend`` die Elemente der ``y``-Liste an.
Zeile 19
    Die Operatoren ``+`` und ``*`` erzeugen jeweils eine neue Liste, wobei die
    ursprüngliche Liste unverändert bleibt.
Zeile 21
    Die Methoden einer Liste werden mit Hilfe der Attributschreibweise für die
    Liste selbst aufgerufen: :samp:`{LISTE}.{METHODE}({ARGUMENTE})`.

Listenoperationen
-----------------

Listen sortieren
~~~~~~~~~~~~~~~~

Listen können mit Hilfe der eingebauten Python-Sortiermethode
:meth:`python3:list.sort` sortiert werden:

.. code-block:: pycon

   >>> x = [5, 3, -3, 3.1, 0, 1]
   >>> x.sort()
   >>> x
   [-3, 0, 1, 3, 3.1, 5]

Mit dieser Methode wird eine Sortierung an Ort und Stelle durchgeführt,
:abbr:`d.h. (das heißt)` die zu sortierende Liste wird geändert. Soll die ursprüngliche Liste unverändert bleiben, habt ihr zwei Möglichkeiten:

#. ihr könnt die Built-in-Funktion :func:`python3:sorted` verwenden, die später
   noch ausführlicher beschrieben wird.
#. ihr könnt eine Kopie der Liste erstellen und die Kopie sortieren:

   .. code-block:: pycon

      >>> x = [5, 3, -3, 3.1, 0, 1]
      >>> y = x[:]
      >>> y.sort()
      >>> y
      [-3, 0, 1, 3, 3.1, 5]
      >>> x
      [5, 3, -3, 3.1, 0, 1]

Auch Zeichenketten und Listen von Listen können sortiert werden:

.. code-block:: pycon

   >>> hipy_list = ["Say", "hi", "to", "all", "Pythonistas", "!"]
   >>> hipy_list.sort()
   >>> hipy_list
   ['!', 'Pythonistas', 'Say', 'all', 'hi', 'to']
   >>> ll = [[5.1, 5.2], [4.0, 5.0], [4.0, 3.0], [3.3, 3.2, 3.1]]
   >>> ll.sort()
   >>> ll
   [[3.3, 3.2, 3.1], [4.0, 3.0], [4.0, 5.0], [5.1, 5.2]]

Beim Vergleich komplexer Objekte werden die Teillisten zuerst nach dem ersten
Element und dann nach dem zweiten Element aufsteigend sortiert.

:meth:`python3:list.sort` kann auch in umgekehrter Reihenfolge sortieren mit
``reverse=True``. Zudem kann auch eine eigene ``key``-Funktion verwendet werden,
um zu bestimmen, wie die Elemente einer Liste sortiert werden sollen.

Die Standard-``key``-Methode, die von :meth:`python3:list.sort` verwendet wird,
erfordert jedoch, dass alle Elemente in der Liste von vergleichbarem Typ sind.
In einer Liste, die sowohl Zahlen als auch Zeichenketten enthält, wird daher
eine :term:`Exception` ausgelöst:

.. code-block:: pycon

   >>> x
   [-5, -4, -3, -2, -1, 0, 1, 'zweitens', [3.1, 3.2, 3.3], (5.1, 5.2)]
   >>> x.sort()
   Traceback (most recent call last):
     File "<stdin>", line 1, in <module>
   TypeError: '<' not supported between instances of 'str' and 'int'

Benutzerdefinierte Sortierung
:::::::::::::::::::::::::::::

.. note::
   Für eine benutzerdefinierte Sortierung müsst ihr :doc:`../../functions/index`
   definieren können. Und auch die Verarbeitung von :doc:`../strings/index` wird
   später noch ausführlicher behandelt.

Üblicherweise sortiert Python Wörter lexikografisch – Großbuchstaben vor
Kleinbuchstaben. Wir möchten jedoch stattdessen eine Liste von Wörtern
nach der Anzahl der Zeichen in jedem Wort aufsteigend sortieren:

.. code-block:: pycon

   >>> def ascending_number_chars(string):
   ...     return len(string)
   ...
   >>> hipy_list = ["Say", "hi", "to", "all", "Pythonistas", "!"]
   >>> new_list = hipy_list[:]
   >>> hipy_list.sort()
   >>> hipy_list
   ['!', 'Pythonistas', 'Say', 'all', 'hi', 'to']
   >>> new_list.sort(key=ascending_number_chars)
   >>> new_list
   ['!', 'hi', 'to', 'Say', 'all', 'Pythonistas']

Die Funktion ``sorted``
:::::::::::::::::::::::

Listen haben eine eingebaute Methode, um sich selbst zu sortieren
:meth:`python3:list.sort`. Andere *Iterables* in Python, wie :abbr:`z.B. (zum
Beispiel)` die Schlüssel von :doc:`../dicts`, haben jedoch keine Sortiermethode.
Python bietet hierfür jedoch die eingebaute Funktion :func:`python3:sorted` an,
die eine sortierte Liste aus einer beliebigen  *Iterables* zurückgibt.
:func:`python3:sorted` verwendet die gleichen :doc:`../../functions/params`
``key`` und ``reverse`` wie die Methode :meth:`python3:list.sort`:

.. code-block:: pycon

   >>> x
   [5, 3, -3, 3.1, 0, 1]
   >>> y = sorted(x)
   >>> y
   [-3, 0, 1, 3, 3.1, 5]
   >>> z = sorted(x, reverse=True)
   >>> z
   [5, 3.1, 3, 1, 0, -3]

.. _list-in:

Listenzugehörigkeit
~~~~~~~~~~~~~~~~~~~

Mit den :ref:`in und not in <python3:in>`-Operatoren, die einen booleschen Wert
zurückgeben, lässt sich leicht prüfen, ob ein Wert in einer Liste enthalten ist.

Listenverkettung
~~~~~~~~~~~~~~~~

Der ``+``-Operator kann verwendet werden um eine Liste aus zwei bestehenden
Listen zu erstellen, wobei die Ausgangslisten unverändert bleiben:

.. code-block:: pycon

   >>> x = [3, -3, 0, 1]
   >>> y = [3.1]
   >>> z = x + y
   >>> z
   [3, -3, 0, 1, 3.1]

Listeninitialisierung
~~~~~~~~~~~~~~~~~~~~~

Ihr könnt den ``*``-Operator verwenden, um eine Liste bestimmter Größe und
bestimmter Werte zu erzeugen. Dies ist eine gängige Methode, um mit Listen zu
arbeiten, deren Größe im Voraus bekannt ist und die dann auch keinen
Memory-Reallocation-Overhead verursacht. Daher solltet ihr dies in solchen
Fällen ``append`` vorziehen, um die Liste zu Beginn des Programms zu vergrößern:

.. code-block:: pycon

   >>> x = [None] * 4
   >>> x
   [None, None, None, None]

Der Operator für ``list``-Multiplikationen ``*`` wiederholt das Kopieren der
Elemente einer Liste die angegebene Zahl und fügt alle Kopien zu einer neuen
Liste zusammen. Dabei wird üblicherweise eine Liste mit einer einzelnen Instanz
von :doc:`/types/none` für die Listenmultiplikation verwendet, aber die Liste
kann alles sein:

.. code-block:: pycon

   >>> initial_list = [[1, 2, 3, 4]]
   >>> arr = initial_list * 4
   >>> arr
   [[1, 2, 3, 4], [1, 2, 3, 4], [1, 2, 3, 4], [1, 2, 3, 4]]

Minimum oder Maximum einer Liste
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Ihr könnt :func:`max` und :func:`min` verwenden, um das größte und kleinste
Element einer Liste zu finden. Wahrscheinlich werdet ihr :func:`max` und
:func:`min` vor allem bei :doc:`numerischen </types/numbers/index>` Listen
verwenden, aber ihr könnt sie auch bei Listen mit beliebigen Elementen
einsetzen; wenn der Vergleich dieser Typen jedoch keinen Sinn ergibt, führt dies
zu einem Fehler:

.. code-block:: pycon

   >>> x = [5, 3, -3, 3.1, 0, 1]
   >>> max(x)
   5
   >>> hipy_list = ["Say", "hi", "to", "all", "Pythonistas", "!"]
   >>> max(hipy_list)
   'to'
   >>> max(x + hipy_list)
   Traceback (most recent call last):
     File "<stdin>", line 1, in <module>
   TypeError: '>' not supported between instances of 'str' and 'int'

Beim Vergleich komplexer Objekte werden die Teillisten zuerst nach dem ersten
Element und dann nach dem zweiten Element :abbr:`u.s.w. /und so weiter)`
analysiert.

.. code-block:: pycon

   >>> ll = [[1.0, 1.1], [1.0, 1.1, 1.2], [0.9, 1.3]]
   >>> max(ll)
   [1.0, 1.1, 1.2]

Suche in einer Liste
~~~~~~~~~~~~~~~~~~~~

Wenn ihr wissen wollt, **wo** in einer Liste ein Wert zu finden ist, könnt ihr
Sie die ``index``-Methode verwenden. Sie durchsucht eine Liste nach einem
Listenelement mit einem bestimmten Wert, und gibt die Position dieses
Listenelements zurück:

.. code-block:: pycon
   :linenos:

   >>> x = [5, 3, 3.0, -3, 3.1, 0, 1]
   >>> x.index(3)
   1
   >>> x.index(3.0)
   1
   >>> x.index(5.0)
   0
   >>> x.index(6)
   Traceback (most recent call last):
     File "<stdin>", line 1, in <module>
   ValueError: 6 is not in list

Zeile 8–11
    Der Versuch, die Position eines Elements zu finden, das nicht in der Liste
    vorhanden ist, führt zu einem Fehler. Dieser kann durch Testen der Liste mit
    den :ref:`in oder not-in <list-in>`-Listenoperatoren vor der Verwendung von
    ``index`` vermieden werden.

Übereinstimmungen in Listen
~~~~~~~~~~~~~~~~~~~~~~~~~~~

``count`` durchsucht ebenfalls eine Liste nach einem bestimmten Wert, gibt aber
die Anzahl der Vorkommen in der Liste zurück und nicht die Position:

.. code-block:: pycon

   >>> x = [5, 3, 3.0, -3, 3.1, 0, 1]
   >>> x.count(3)
   2
   >>> x.count(5)
   1
   >>> x.count(6)
   0

Verschachtelte Listen und ``deepcopy``
--------------------------------------

Listen können verschachtelt werden, :abbr:`z.B. (zum Beispiel)` für die
Darstellung zweidimensionaler Matrizen. Auf die Elemente dieser Matrizen kann
mit Hilfe von zweidimensionalen Indizes verwiesen werden:

.. code-block:: pycon

   >>> ll = [[5.1, 5.2], [4.0, 5.0], [4.0, 3.0], [3.3, 3.2]]
   >>> ll[0]
   [5.1, 5.2]
   >>> ll[0][1]
   5.2

Dieser Mechanismus lässt sich wie erwartet auf mehr Dimensionen übertragen:

.. code-block:: pycon

   >>> sub = [0]
   >>> sup = [sub, 1]
   >>> sup
   [[0], 1]
   >>> sub[0] = 1
   >>> sup
   [[1], 1]
   >>> sup[0][0] = 2
   >>> sub
   [2]
   >>> sup
   [[2], 1]

Wenn aber ``sub`` auf eine andere Liste gesetzt wird, ist die Verbindung
zwischen ``sub`` und ``sup`` unterbrochen:

.. code-block:: pycon

   >>> sub = [3]
   >>> sup
   [[2], 1]

Ihr könnt eine Kopie einer Liste erhalten, indem ihr ein vollständiges *Slice*
(also ``x[:]`` erzeugt) oder  + oder * verwendet (:abbr:`z.B. (zum Beispiel)`
``x + []`` oder ``x * 1``). Alle drei erzeugen eine so genannte flache Kopie der
Liste, was wahrscheinlich in den meisten Fällen das Gewünschte ist. Wenn eure
Liste jedoch andere Listen enthält, die in ihr verschachtelt sind, möchtet ihr
vielleicht eine tiefe Kopie erstellen. Dies könnt ihr mit der Funktion
:func:`copy.deepcopy` des :mod:`python3:copy`-Moduls tun:

.. code-block:: pycon

   >>> shallow = sup[:]
   >>> shallow
   [[2], 1]

Die ``shallow``-Kopie kopiert nicht die Elemente der Liste sondern verweist nur
auf die ursprünglichen Elemente. Die Änderung eines dieser Elemente wirkt sich
sowohl auf ``shallow`` wie auch auf ``sup`` aus:

.. code-block:: pycon

   >>> shallow[1] = 2
   >>> shallow
   [[2], 2]
   >>> sup
   [[2], 1]
   >>> shallow[0][0] = 0
   >>> sup
   [[0], 1]

``deepcopy`` ist jedoch unabhängig von der Originalliste, und keine Änderung an
ihr hat Auswirkungen auf die Originalliste:

.. code-block:: pycon

   >>> import copy
   >>> deep = copy.deepcopy(sup)
   >>> deep
   [[0], 1]
   >>> deep[0][0] = 1
   >>> deep
   [[1], 1]
   >>> sup
   [[0], 1]

Checks
------

* Was gibt :func:`len` für jeden der folgenden Fälle zurück:

  * ``[3]``
  * ``[]``
  * ``[[1, [2, 3], 4], "5 6"]``

* Wie würdet ihr mit :func:`len` und Slices die zweite Hälfte einer Liste
  ermitteln, wenn ihr nicht wisst, wie groß sie ist?

* Wie könntet ihr die letzten zwei Einträge einer Liste an den Anfang
  verschieben, ohne die Reihenfolge der beiden zu ändern?

* Welcher der folgenden Fälle löst eine Exception aus?

  * ``min(["1", "2", "3"])``
  * ``max([1, 2, "3"])``
  * ``[1,2,3].count("1")``

* Wenn ihr eine Liste ``l`` habt, wie könnt ihr daraus einen bestimmten Wert
  ``i`` entfernen?

* Wie könnt ihr alle Dubletten aus einer Liste entfernen ohne die Reihenfolge
  der Elemente in der Liste zu ändern?

* Wenn ihr eine verschachtelte Liste ``ll`` habt, wie könnt ihr eine Kopie
  ``nll`` dieser Liste erhalten, in der ihr die Elemente ändern könnt, ohne den
  Inhalt von ``ll`` zu verändern?

.. _check-list:

* Stellt sicher, dass das Objekt ``my_collection`` eine Liste ist, bevor ihr
  versucht, daran Daten anzuhängen.
