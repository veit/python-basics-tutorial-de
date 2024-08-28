Tupel
=====

Tupel ähneln Listen, sind aber unveränderlich, :abbr:`d.h. (das heißt)` sie
können nach ihrer Erstellung nicht mehr geändert werden. Die Operatoren (``in``,
``+`` und ``*``) und eingebauten Funktionen (``len``, ``max`` und ``min``)
arbeiten mit ihnen auf die gleiche Weise wie mit :doc:`lists`, da keine dieser
Funktionen das Original verändert. Die Index- und die Slice-Notation
funktionieren auf die gleiche Weise, um Elemente oder Slices zu erhalten, können
aber nicht zum Hinzufügen, Entfernen oder Ersetzen von Elementen verwendet
werden. Außerdem gibt es nur zwei Tupelmethoden: ``count`` und ``index``. Ein
wichtiger Zweck von Tupeln ist die Verwendung als Schlüssel für :doc:`dicts`.
Sie sind auch effizienter zu verwenden, wenn man keine Änderungsmöglichkeit
benötigt.

.. code-block:: python
    :linenos:

    ()
    (1,)
    (1, 2, 3, 5)
    (1, "2.", 3.0, ["4a", "4b"], (5.1, 5.2))

Zeile 2
    Ein Tupel mit einem Element benötigt ein Komma.
Zeile 4
    Ein Tupel kann, wie eine :doc:`Liste <lists>`, eine Mischung anderer Typen
    als Elemente enthalten, darunter beliebige :doc:`numbers`, :doc:`strings`,
    :doc:`tuples`, :doc:`lists`, :doc:`dicts`, :doc:`files` und Funktionen.

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

* Tupel können als Schlüssel in :doc:`dicts` und Werte in :doc:`sets` verwendet
  werden.

Zusammenfassung
---------------

+---------------+---------------+---------------+---------------+---------------+
| Datentyp      | veränderlich  | geordnet      | indiziert     | Duplikate     |
+===============+===============+===============+===============+===============+
| Tuple         | ❌            | ✅            | ✅            | ✅            |
+---------------+---------------+---------------+---------------+---------------+

Checks
------

* Erläutert, warum die folgenden Operationen nicht auf das Tuple ``t``
  angewendet werden können:

  * ``t.append(1)``
  * ``t[2] = 2``
  * ``del t[3]``

* Wie könnt ihr die Elemente eines Tuple sortieren?
