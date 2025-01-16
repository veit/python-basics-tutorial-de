Sets
====

Sets in Python sind eine ungeordnete Sammlung von Objekten, die in Situationen
verwendet werden, in denen die Zugehörigkeit und Einzigartigkeit zur Menge die
wichtigsten Informationen des Objekts sind. Der ``in``-Operator läuft bei Sets
schneller als bei :doc:`lists`:

.. _set:

``set``
-------

Sets erstellen
~~~~~~~~~~~~~~

Ihr könnt Sets erstellen, indem ihr :class:`set` auf eine Sequenz anwendet,
:abbr:`z.B. (zum Beispiel)` auf eine :doc:`Liste <lists>`.

.. code-block:: pycon

   >>> sequences = set(["list", "tuple", "tuple"])
   >>> sequences
   {'tuple', 'list'}

Wenn eine Sequenz zu einem Set gemacht wird, werden Duplikate entfernt,
allerdings geht dann auch die Reihenfolge verloren.

Auch können einzelne Elemente nicht mit Slicing ausgewählt werden:

.. code-block:: pycon

   >>> sequences[0]
   Traceback (most recent call last):
     File "<python-input-27>", line 1, in <module>
       sequences[0]
       ~~~~~~~~~^^^
   TypeError: 'set' object is not subscriptable

Werte überprüfen
~~~~~~~~~~~~~~~~

Das Schlüsselwort ``in`` wird verwendet, um die Zugehörigkeit eines Objekts zu
einer Menge zu prüfen.

.. code-block:: pycon

   >>> "list" in sequences
   True
   >>> "set" in sequences
   False

Werte hinzufügen und löschen
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Mit ``add`` und ``remove`` könnt ihr Werte hinzufügen und löschen.

.. code-block:: pycon

   >>> quantities = sequences.add("set")
   >>> quantities
   {'list', 'tuple', 'set'}
   >>> quantities.remove("set")
   >>> quantities
   {'list', 'tuple'}

Die Elemente sind ungeordnet, :abbr:`d.h. (das heißt)` die Werte innerhalb
einer Sequens können sich verschieben, wenn neue Elemente hinzugefügt werden.

Mengenbildung
~~~~~~~~~~~~~

Vereinigungsmenge
   .. code-block:: pycon

      x = {4, 2, 3, 2, 1}
      y = {3, 4, 5}
      >>> x.union(y)
      {0, 1, 2, 3, 4, 5}

Schnittmenge
   .. code-block:: pycon

      >>> x.intersection(y)
      {3}

Differenz- oder Restmenge
   .. code-block:: pycon

      >>> x.difference(y)
      {0, 1, 2}

.. _frozenset:

``frozenset``
-------------

Neben ``set`` gibt es noch ``frozenset``, einen unveränderlichen Datentyp. Damit
können sie auch Mitglieder anderer Mengen sein:

.. code-block:: pycon
   :linenos:

   >>> sequences = frozenset(["list", "tuple", "set", "tuple"])
   >>> sequences
   frozenset({'list', 'tuple', 'set'})
   >>> dicts = {"dict"}
   >>> sequences.add(dicts)
   Traceback (most recent call last):
     File "<python-input-18>", line 1, in <module>
       sequences.add(dicts)
       ^^^^^^^^^^^^^
   AttributeError: 'frozenset' object has no attribute 'add'
   >>> dicts.add(sequences)
   >>> dicts
   {frozenset({'list', 'tuple', 'set'}), 'dict'}

Performance
-----------

Sets sind sehr schnell bei der Überprüfung, ob Elemente in einer Menge enthalten
sind. Auch zum Auffinden von gemeinsamen und eindeutigen Werten zweier Mengen
ist die mengenarithmetik von Sets gut geeignet. Hierfür kann es sinnvoll sein,
:doc:`lists` oder :doc:`tuples` in Sets umzuwandeln.

Reihenfolge
-----------

Der Geschwindigkeitsvorteil hat jedoch auch ihren Preis: Sets halten die
Elemente nicht in der richtigen Reihenfolge, während :doc:`lists` und
:doc:`tuples` dies tun. Wenn die Reihenfolge für euch wichtig ist, solltet ihr
nur für bestimmte Operationen die Elemente in ein Set umwandeln, :abbr:`z.B.
(zum Beispiel)` um zu überprüfen, ob die Elemente einer Liste eindeutig sind mit

.. code-block:: pycon

   >>> sequences = ["list", "tuple", "set", "tuple"]
   >>> len(sequences) == len(set(sequences))
   False

Checks
------

* Wieviele Elemente hat ein Set, wenn es aus der folgenden Liste
  ``[4, 2, 3, 2, 1]`` gebildet wird?
