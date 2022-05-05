Sets
====

Ein Set in Python ist eine ungeordnete Sammlung von Objekten, die in Situationen
verwendet wird, in denen die Zugehörigkeit und Einzigartigkeit zur Menge die
wichtigsten Informationen des Objekts sind. Sets verhalten sich wie Kollektionen
von :doc:`Dictionary <dicts>`-Schlüsseln ohne zugehörige Werte:

.. code-block:: python
   :linenos:

    >>> x = set([1, 2, 3, 2, 4])
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
