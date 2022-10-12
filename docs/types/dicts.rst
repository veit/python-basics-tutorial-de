Dictionaries
============

Pythons eingebauter Dictionary-Datentyp bietet assoziative Array-Funktionalität,
die mit Hilfe von Hash-Tabellen implementiert wird. Die eingebaute Funktion
``len`` gibt die Anzahl der Schlüssel-Wert-Paare in einem Wörterbuch zurück. Die
``del``-Anweisung kann zum Löschen eines Schlüssel-Wert-Paares verwendet werden.
Wie bei :doc:`lists` sind mehrere Dictionary-Methoden (``clear``, ``copy``,
``get``, ``items``, ``keys``, ``update`` und ``values``) verfügbar.

.. code-block:: python

    >>> x = {1: "eins", 2: "zwei"}
    >>> x[3] = "drei"
    >>> x["viertes"] = "vier"
    >>> list(x.keys())
    [1, 2, 3, 'viertes']
    >>> x[1]
    'eins'
    >>> x.get(1, "nicht vorhanden")
    'eins'
    >>> x.get(5, "nicht vorhanden")
    'nicht vorhanden'

Schlüssel müssen vom unveränderlichen Typ sein, einschließlich :doc:`numbers`,
:doc:`strings/index` und Tupel. Werte können alle Arten von Objekten sein,
einschließlich veränderlicher Typen wie :doc:`lists` und :doc:`dicts`. Wenn ihr
versucht, auf den Wert eines Schlüssels zuzugreifen, der nicht im Dictionary
enthalten ist, wird eine ``KeyError``-Exception ausgelöst. Um diesen Fehler zu
vermeiden, gibt die Dictionary-Methode ``get`` optional einen
benutzerdefinierten Wert zurück, wenn ein Schlüssel nicht in einem Wörterbuch
enthalten ist.
