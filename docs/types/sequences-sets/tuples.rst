Tupel
=====

Tupel ähneln :doc:`lists`, können jedoch nicht geändert sondern nur erstellt
werden. Tupel haben die wichtige Aufgabe, effizient :abbr:`z.B. (zum Beispiel)`
Schlüssel für :doc:`../dicts` zu erstellen.

Tupel werden ähnlich wie die Listen erstellt: einer Variablen wird eine Folge
von Werten zugewiesen, die jedoch nicht in eckige sondern in runde Klammern
eingeschlossen werden:

.. code-block:: pycon

   >>> x = (1, "2.", 3.0, ["4a", "4b"], (5.1, 5.2))

Diese Zeile erzeugt ein Tupel mit fünf Elementen. Nachdem ein Tupel erstellt
wurde, kann es ähnlich wie die eine Liste verwendet werden:

.. code-block:: pycon

   >>> x[1]
   '2.'
   >>> x[1:]
   ('2.', 3.0, ['4a', '4b'], (5.1, 5.2))
   >>> len(x)
   5
   >>> max(x[:3:2])
   3.0
   >>> min(x[:3:2])
   1
   >>> 1 in x
   True
   >>> 5.1 not in x
   True

Die Operatoren (:ref:`in, not in <python3:in>`, ``+`` und ``*``) und die
eingebauten Funktionen (``len``, ``max`` und ``min``) arbeiten mit Tupeln auf
die gleiche Weise wie mit :doc:`lists`, da keine dieser Funktionen das Original
verändert. Es gibt jedoch nur zwei Tupel-Methoden: ``count`` und ``index``.

Mit den ``+``- und ``*``-Operatoren könnt ihr Tupel aus bestehenden Tupeln
erstellen:

.. code-block:: pycon

   >>> x + x
   (1, '2.', 3.0, ['4a', '4b'], (5.1, 5.2), 1, '2.', 3.0, ['4a', '4b'], (5.1, 5.2))
   >>> 2 * x
   (1, '2.', 3.0, ['4a', '4b'], (5.1, 5.2), 1, '2.', 3.0, ['4a', '4b'], (5.1, 5.2))

Eine Kopie eines Tupels kann auf die gleiche Weise wie bei Listen erstellt
werden:

.. code-block:: pycon

   >>> x[:]
   (1, '2.', 3.0, ['4a', '4b'], (5.1, 5.2))
   >>> x * 1
   (1, '2.', 3.0, ['4a', '4b'], (5.1, 5.2))
   >>> x + ()
   (1, '2.', 3.0, ['4a', '4b'], (5.1, 5.2))

Der Versuch, ein Tupel zu ändern, führt jedoch zu einer
Fehlermeldung:

.. code-block:: pycon

   >>> x[1] = "zweitens"
   Traceback (most recent call last):
     File "<stdin>", line 1, in <module>
   TypeError: 'tuple' object does not support item assignment

Ein-Element-Tupel
-----------------

Einen kleinen syntaktischen Unterschied gibt es jedoch zu Listen: während
``[1]`` eine Liste mit einem Element erstellt, ist ``(1)`` eine Ganzzahl und
kein Tupel. Der Hintergrund hierfür ist, dass runde Klammern auch dazu verwendet
werden, Elemente in Ausdrücken zu gruppieren, um eine bestimmte
Auswertungsreihenfolge zu erzwingen. Daher enthält jedes Tupel mit einem oder
mehr Elementen ein oder mehr Kommas:

.. blacken-docs:off

.. code-block:: pycon

    >>> y = ()
    >>> type(y)
    <class 'tuple'>
    >>> z = (1 + 3.0)
    >>> type(z)
    <class 'float'>
    >>> z = (1 + 3.0,)
    >>> type(z)
    <class 'tuple'>

.. blacken-docs:on

Packen und Entpacken von Tupeln
-------------------------------

Tupel können auf der linken Seite eines Zuweisungsoperators erscheinen. In
diesem Fall erhalten die Variablen im Tupel die entsprechenden Werte aus dem
Tupel auf der rechten Seite des Zuweisungsoperators. Hier ist ein einfaches
Beispiel:

.. code-block:: pycon

   >>> (v, w, x, y, z) = (1, "2.", 3.0, ["4a", "4b"], (5.1, 5.2))
   >>> v
   1
   >>> w
   '2.'

Dieses Beispiel kann noch weiter vereinfacht werden, da Python Tupel in einem
Zuweisungskontext auch ohne die runden Klammern erkennt:

.. code-block:: pycon

   >>> v, w, x, y, z = 1, "2.", 3.0, ["4a", "4b"], (5.1, 5.2)
   >>> y
   ['4a', '4b']
   >>> z
   (5.1, 5.2)

Mit ``*`` wird das Entpacken noch erweitert um eine beliebige Anzahl von
Elementen aufzunehmen, die nicht zu den sonstigen Elementen passen:

.. code-block:: pycon

   >>> x = (1, "2.", 3.0, ["4a", "4b"], (5.1, 5.2))
   >>> a, b, *c = x
   >>> a, b, c
   (1, '2.', [3.0, ['4a', '4b'], (5.1, 5.2)])
   >>> a, *b, c = x
   >>> a, b, c
   (1, ['2.', 3.0, ['4a', '4b']], (5.1, 5.2))
   >>> a, *b, c, d, e, f = x
   >>> a, b, c, d, e, f
   (1, [], '2.', 3.0, ['4a', '4b'], (5.1, 5.2))

.. note::
   Das mit ``*`` versehene Element erhält alle überzähligen Elemente als Liste
   und, wenn keine überzähligen Elemente vorhanden sind, eine leere Liste.

Konvertieren zwischen Listen und Tupeln
---------------------------------------

Eine Liste kann mit Hilfe der eingebauten Funktion ``tuple`` in ein Tupel
umgewandelt werden:

.. code-block:: pycon

    >>> x = [1, 2, 3, 5]
    >>> tuple(x)
    (1, 2, 3, 5)

Umgekehrt kann ein Tupel mit Hilfe der eingebauten Funktion list in eine Liste
umgewandelt werden:

.. code-block:: pycon

    >>> x = (1, 2, 3, 4)
    >>> list(x)
    [1, 2, 3, 4]

Die Vorteile von Tupeln gegenüber :doc:`lists` sind:

* Tupel sind schneller als Listen.

  Wenn ihr eine konstante Menge von Werten definieren und diese nur durchlaufen
  wollt, solltet ihr ein Tupel anstelle einer Liste verwenden.

* Tupel können nicht verändert werden und sind daher *schreibgeschützt*.

* Tupel können als Schlüssel in :doc:`../dicts` und Werte in :doc:`sets`
  verwendet werden.

Checks
------

* Erläutert, warum die folgenden Operationen nicht auf das Tuple ``t``
  angewendet werden können:

  * ``t.append(1)``
  * ``t[2] = 2``
  * ``del t[3]``

* Wie könnt ihr die Elemente eines Tuple sortieren?
