Dictionaries
============

Dictionaries bestehen aus Schlüssel-Wert-Paaren. Schlüssel müssen vom
unveränderlichen Typ sein, einschließlich :doc:`numbers/index`,
:doc:`strings/index` und :doc:`sequences-sets/tuples`.

.. warning::
   Auch wenn ihr in einem Dictionary verschiedene Schlüsseltypen verwenden
   könnt, solltet ihr das vermeiden, da dadurch nicht nur die Lesbarkeit sondern
   auch die Sortierung erschwert wird.

Werte können alle Arten von Objekten sein, einschließlich veränderlicher Typen
wie :doc:`sequences-sets/lists` und :doc:`dicts`.

.. code-block:: pycon

   >>> dict = {
   ...     "2022-01-31": -0.751442,
   ...     "2022-02-01": 0.816935,
   ...     "2022-02-02": -0.272546,
   ... }
   >>> dict["2022-02-03"] = -0.268295

Wenn ihr versucht, auf den Wert eines Schlüssels zuzugreifen, der nicht im
Dictionary enthalten ist, wird eine ``KeyError``-:doc:`/control-flow/exceptions`
ausgelöst. Um diesen Fehler zu vermeiden, gibt die Dictionary-Methode ``get``
optional einen benutzerdefinierten Wert zurück, wenn ein Schlüssel nicht in
einem Wörterbuch enthalten ist.

.. code-block:: pycon

   >>> dict["2022-02-03"]
   -0.268295
   >>> dict["2022-02-04"]
   Traceback (most recent call last):
     File "<python-input-15>", line 1, in <module>
       dict["2022-02-04"]
       ~~~~^^^^^^^^^^^^^^
   KeyError: '2022-02-04'
   >>> dict.get("2022-02-03", "Messwert nicht vorhanden")
   -0.268295
   >>> dict.get("2022-02-04", "Messwert nicht vorhanden")
   'Messwert nicht vorhanden'

Weitere Dict-Methoden
---------------------

Die in Dicts eingebaute Funktion ``len`` gibt die Anzahl der
Schlüssel-Wert-Paare zurück. Die ``del``-Anweisung kann zum Löschen eines
Schlüssel-Wert-Paares verwendet werden. Wie bei :doc:`sequences-sets/lists` sind
mehrere Dictionary-Methoden (:py:meth:`clear <dict.clear>`, :py:meth:`copy
<dict.copy>`, :py:meth:`get <dict.get>`, :py:meth:`items <dict.items>`,
:py:meth:`keys <dict.keys>`, :py:meth:`update <dict.update>` und
:py:meth:`values <dict.values>`) verfügbar.

Die Methoden :py:meth:`keys <dict.keys>`, :py:meth:`values <dict.values>` und
:py:meth:`items <dict.items>` geben keine Listen zurück, sondern
Dictionary-View-Objekte, die sich wie Sequenzen verhalten, aber dynamisch
aktualisiert werden, wenn sich das Dictionary ändert. Aus diesem Grund müsst ihr
die Funktion ``list`` verwenden, damit sie in diesen Beispielen zu einer Liste
werden:

.. code-block:: pycon

   >>> list(dict.keys())
   ['2022-01-31', '2022-02-01', '2022-02-02', '2022-02-03']

Ab Python 3.6 behalten Dictionaries die Reihenfolge bei, in der die Schlüssel
erstellt wurden, und sie werden mit :py:meth:`keys <dict.keys>` auch in dieser
Reihenfolge zurückgegeben.

Dicts zusammenführen
~~~~~~~~~~~~~~~~~~~~

Mit der :py:meth:`dict.update`-Methode könnt ihr zwei Dictionaries zu einem
einzigen Dictionary zusammenfügen:

.. code-block:: pycon

   >>> titles = {7.0: "Data Types", 7.1: "Lists", 7.2: "Tuples"}
   >>> new_titles = {7.0: "Data types", 7.3: "Sets"}
   >>> titles.update(new_titles)
   >>> titles
   {7.0: 'Data types', 7.1: 'Lists', 7.2: 'Tuples', 7.3: 'Sets'}

.. note::
   Die Reihenfolge der Operanden ist wichtig, da ``7.0`` dupliziert wird und der
   Wert des letzten Schlüssel den vorhergehenden überschreibt.

``setdefault``
~~~~~~~~~~~~~~

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
  Sheet einer Tabellenkalkulation verwenden, indem ihr
  :doc:`/types/sequences-sets/tuples` als Schlüssel Zeilen- und Spaltenwerte
  verwendet. Schreibt Beispielcode, um Werte hinzuzufügen und wieder abzufragen.

* Wie könnt ihr alle Dubletten aus einer Liste entfernen ohne die Reihenfolge
  der Elemente in der Liste zu ändern?
