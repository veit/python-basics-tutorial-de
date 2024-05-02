Sets
====

Ein Set in Python ist eine ungeordnete Sammlung von Objekten, die in Situationen
verwendet wird, in denen die Zugehörigkeit und Einzigartigkeit zur Menge die
wichtigsten Informationen des Objekts sind. Der ``in``-Operator läuft bei Sets
schneller als bei :doc:`lists`:

.. code-block:: pycon
   :linenos:

    >>> x = set([4, 2, 3, 2, 1])
    >>> x
    {1, 2, 3, 4}
    >>> 1 in x
    True
    >>> 5 in x
    False

Zeile 1
    Ihr könnt ein Set erstellen, indem ihr ``set`` auf eine Sequenz wie eine
    :doc:`Liste <lists>` anwendet.
Zeile 3
    Wenn eine Sequenz zu einem Set gemacht wird, werden Duplikate entfernt.
Zeilen 4 und 6
    Das Schlüsselwort wird verwendet, um die Zugehörigkeit eines Objekts zu
    einer Menge zu prüfen.

Sets verhalten sich wie Kollektionen von :doc:`Dictionary <dicts>`-Schlüsseln
ohne zugehörige Werte.

Der Geschwindigkeitsvorteil hat jedoch auch ihren Preis: Sets halten die
Elemente nicht in der richtigen Reihenfolge, während :doc:`lists` und
:doc:`tuples` dies tun. Wenn die Reihenfolge für euch wichtig ist, solltet ihr
eine Datenstruktur verwenden, die sich die Reihenfolge merkt.

Zusammenfassung
---------------

+---------------+---------------+---------------+---------------+---------------+
| Datentyp      | veränderlich  | geordnet      | indiziert     | Duplikate     |
+===============+===============+===============+===============+===============+
| Set           | ✅            | ❌            | ❌            | ❌            |
+---------------+---------------+---------------+---------------+---------------+
