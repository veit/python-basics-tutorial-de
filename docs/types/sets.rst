Sets
====

Sets in Python sind eine ungeordnete Sammlung von Objekten, die in Situationen
verwendet werden, in denen die Zugehörigkeit und Einzigartigkeit zur Menge die
wichtigsten Informationen des Objekts sind. Der ``in``-Operator läuft bei Sets
schneller als bei :doc:`lists`:

``set``
-------

.. code-block:: pycon
   :linenos:

    >>> x = set([4, 2, 3, 2, 1])
    >>> x
    {1, 2, 3, 4}
    >>> 1 in x
    True
    >>> 5 in x
    False
    >>> x.add(0)
    >>> x
    {0, 1, 2, 3, 4}
    >>> x.remove(4)
    >>> x
    {0, 1, 2, 3}
    >>> y = set([3, 4, 5])
    >>> x | y
    {0, 1, 2, 3, 4, 5}
    >>> x & y
    {3}
    >>> x ^ y
    {0, 1, 2, 4, 5}
    >>> x.update(y)
    >>> x
    {0, 1, 2, 3, 4, 5}

Zeile 1
    Ihr könnt ein Set erstellen, indem ihr ``set`` auf eine Sequenz anwendet,
    :abbr:`z.B. (zum Beispiel)` auf eine :doc:`Liste <lists>`.
Zeile 3
    Wenn eine Sequenz zu einem Set gemacht wird, werden Duplikate entfernt.
Zeilen 4–7
    Das Schlüsselwort ``in`` wird verwendet, um die Zugehörigkeit eines Objekts
    zu einer Menge zu prüfen.
Zeilen 8–13
    Mit ``add`` und ``remove`` könnt ihr die Elemente in ``set`` ändern.
Zeile 15
    ``|`` wird verwendet, um die Vereinigung oder Kombination von zwei Mengen zu
    erhalten.
Zeile 17
    ``&`` wird verwendet, um die Schnittmenge zu erhalten.
Zeile 19
    ``^`` wird verwendet, um die symmetrische Differenz zu finden, :abbr:`d.h.
    (das heißt)` Elemente, die in der einen oder der anderen Menge enthalten
    sind, aber nicht in beiden.

``frozenset``
-------------

Neben ``set`` gibt es noch ``frozenset``, einen unveränderlichen Datentyp. Damit
können sie auch Mitglieder anderer Mengen sein:

.. code-block:: pycon
   :linenos:

   >>> x = set([4, 2, 3, 2, 1])
   >>> z = frozenset(x)
   >>> z
   frozenset({1, 2, 3, 4})
   >>> z.add(5)
   Traceback (most recent call last):
     File "<stdin>", line 1, in <module>
   AttributeError: 'frozenset' object has no attribute 'add'
   >>> x.add(z)
   >>> x
   {1, 2, 3, 4, frozenset({1, 2, 3, 4})}

Zusammenfassung
---------------

Der Geschwindigkeitsvorteil hat jedoch auch ihren Preis: Sets halten die
Elemente nicht in der richtigen Reihenfolge, während :doc:`lists` und
:doc:`tuples` dies tun. Wenn die Reihenfolge für euch wichtig ist, solltet ihr
eine Datenstruktur verwenden, die sich die Reihenfolge merkt.

+---------------+---------------+---------------+---------------+---------------+
| Datentyp      | veränderlich  | geordnet      | indiziert     | Duplikate     |
+===============+===============+===============+===============+===============+
| Sets          | ✅            | ❌            | ❌            | ❌            |
+---------------+---------------+---------------+---------------+---------------+
| Frozensets    | ❌            | ❌            | ❌            | ❌            |
+---------------+---------------+---------------+---------------+---------------+
