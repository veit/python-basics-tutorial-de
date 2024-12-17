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

Ihr könnt Sets erstellen, indem ihr ``set`` auf eine Sequenz anwendet,
:abbr:`z.B. (zum Beispiel)` auf eine :doc:`Liste <lists>`.

.. code-block:: pycon

   >>> x = set([4, 2, 3, 2, 1])
   >>> y = set([3, 4, 5])
   >>> x
   {1, 2, 3, 4}
   >>> y
   {3, 4, 5}

Wenn eine Sequenz zu einem Set gemacht wird, werden Duplikate entfernt.

Werte überprüfen
~~~~~~~~~~~~~~~~

Das Schlüsselwort ``in`` wird verwendet, um die Zugehörigkeit eines Objekts zu
einer Menge zu prüfen.

.. code-block:: pycon

   >>> 1 in x
   True
   >>> 1 in y
   False

Werte hinzufügen und löschen
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Mit ``add`` und ``remove`` könnt ihr Werte hinzufügen und löschen.

.. code-block:: pycon

   >>> x.add(0)
   >>> x
   {0, 1, 2, 3, 4}
   >>> x.remove(4)
   >>> x
   {0, 1, 2, 3}

Mengenbildung
~~~~~~~~~~~~~

Vereinigungsmenge
   .. code-block:: pycon

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

Reihenfolge
-----------

Der Geschwindigkeitsvorteil hat jedoch auch ihren Preis: Sets halten die
Elemente nicht in der richtigen Reihenfolge, während :doc:`lists` und
:doc:`tuples` dies tun. Wenn die Reihenfolge für euch wichtig ist, solltet ihr
eine Datenstruktur verwenden, die sich die Reihenfolge merkt.

Checks
------

* Wieviele Elemente hat ein Set, wenn es aus der folgenden Liste
  ``[4, 2, 3, 2, 1]`` gebildet wird?
