Dictionaries
============

Pythons eingebauter Dictionary-Datentyp bietet assoziative Array-Funktionalität,
die mit Hilfe von Hash-Tabellen implementiert wird. Die eingebaute Funktion
``len`` gibt die Anzahl der Schlüssel-Wert-Paare in einem Wörterbuch zurück. Die
``del``-Anweisung kann zum Löschen eines Schlüssel-Wert-Paares verwendet werden.
Wie bei :doc:`lists` sind mehrere Dictionary-Methoden (:py:meth:`clear
<dict.clear>`, :py:meth:`copy <dict.copy>`, :py:meth:`get <dict.get>`,
:py:meth:`items <dict.items>`, :py:meth:`keys <dict.keys>`, :py:meth:`update
<dict.update>` und :py:meth:`values <dict.values>`) verfügbar.

.. code-block:: pycon

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
:doc:`strings` und Tupel.

.. warning::
   Auch wenn ihr in einem Dictionary verschiedene Schlüsseltypen verwenden
   könnt, solltet ihr das vermeiden, da dadurch nicht nur die Lesbarkeit sondern
   auch die Sortierung erschwert wird.

Werte können alle Arten von Objekten sein,
einschließlich veränderlicher Typen wie :doc:`lists` und :doc:`dicts`. Wenn ihr
versucht, auf den Wert eines Schlüssels zuzugreifen, der nicht im Dictionary
enthalten ist, wird eine ``KeyError``-Exception ausgelöst. Um diesen Fehler zu
vermeiden, gibt die Dictionary-Methode ``get`` optional einen
benutzerdefinierten Wert zurück, wenn ein Schlüssel nicht in einem Wörterbuch
enthalten ist.

``setdefault``
--------------

:py:meth:`setdefault <dict.setdefault>` kann verwendet werden, um Zähler für
die Schlüssel eines Dicts bereitzustellen, :abbr:`z.B. (zum Beispiel)`:

.. code-block:: pycon

   >>> titles = ["Data types", "Lists", "Sets", "Lists"]
   >>> for title in titles:
   ...     titles_count.setdefault(title, 0)
   ...     titles_count[title] += 1
   ...
   >>> titles_count
   {'Data types': 1, 'Lists': 2, 'Sets': 1}

.. note::
   Solche Zähloperationen verbreiteten sich schnell, sodass später die Klasse
   :py:class:`collections.Counter` zur Python-Standardbibliothek hinzugefügt
   wurde. Diese Klasse kann die oben genannten Operationen viel einfacher
   durchführen:

   .. code-block:: pycon

      >>> collections.Counter(titles)
      Counter({'Lists': 2, 'Data types': 1, 'Sets': 1})

Dictionaries zusammenführen
---------------------------

Ihr könnt zwei Dictionaries zu einem einzigen Dictionary zusammenfügen mit der
:py:meth:`dict.update`-Methode:

.. code-block:: pycon

   >>> titles = {7.0: "Data Types", 7.1: "Lists", 7.2: "Tuples"}
   >>> new_titles = {7.0: "Data types", 7.3: "Sets"}
   >>> titles.update(new_titles)
   >>> titles
   {7.0: 'Data types', 7.1: 'Lists', 7.2: 'Tuples', 7.3: 'Sets'}

.. note::
   Die Reihenfolge der Operanden ist wichtig, da ``7.0`` dupliziert wird und der
   Wert des letzten Schlüssel den vorhergehenden überschreibt.

Erweiterungen
-------------

`python-benedict <https://github.com/fabiocaccamo/python-benedict>`_
    ``dict``-Unterklasse mit Keylist/Keypath/Keyattr-Unterstützung sowie
    I/O-Shortcuts.
:doc:`pandas <Python4DataScience:workspace/pandas/python-data-structures>`
    kann Dicts in Series und DataFrames überführen.

Checks
------

* Angenommen, ihr habt die beiden Dictionaries ``x = {"a": 1, "b": 2, "c": 3,
  "d": 4}`` und ``y = {"a": 5, "e": 6, "f": 7}``. Was wäre der Inhalt von ``x``,
  nachdem die folgenden Codeschnipsel ausgeführt wurden?

  .. code-block:: pycon

     >>> del x["b"]
     >>> z = x.setdefault("e", 8)
     >>> x.update(y)

# Welcher der folgenden Ausdrücke kann ein Schlüssel eines Dictionary sein:
  ``1``; ``"Veit"``; ``("Veit", [1])``; ``[("Veit", [1])]``; ``["Veit"]``;
  ``("Veit", "Tim", "Monique")``

* Ihr könnt ein :doc:`Dictionary </types/dicts>` verwenden, und das wie ein
  Sheet einer Tabellenkalkulation verwenden, indem ihr :doc:`/types/tuples` als
  Schlüssel Zeilen- und Spaltenwerte verwendet. Schreibt Beispielcode, um Werte
  hinzuzufügen und wieder abzufragen.
