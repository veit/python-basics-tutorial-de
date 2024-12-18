Operatoren und Funktionen
=========================

Die Operatoren und Funktionen, die mit Zeichenketten arbeiten, geben neue, vom
Original abgeleitete Zeichenketten zurück. Die Operatoren (``in``, ``+`` und
``*``) und eingebauten Funktionen (``len``, ``max`` und ``min``) arbeiten mit
Zeichenketten genauso wie mit :doc:`../sequences-sets/lists` and
:doc:`../sequences-sets/tuples`.

.. code-block:: pycon

   >>> welcome = "Hello pythonistas!\n"
   >>> 2 * welcome
   'Hello pythonistas!\nHello pythonistas!\n'
   >>> welcome + welcome
   'Hello pythonistas!\nHello pythonistas!\n'
   >>> "python" in welcome
   True
   >>> max(welcome)
   'y'
   >>> min(welcome)
   '\n'

Indizierung und Slicing
-----------------------

Auch die Index- und Slice-Notation funktioniert auf die gleiche Weise, um
einzelne Elemente zu erhalten:

.. code-block:: pycon

   >>> welcome[0:5]
   'Hello'
   >>> welcome[6:-1]
   'pythonistas!'

Die Index- und Slice-Notation kann jedoch nicht verwendet werden, um Elemente
hinzuzufügen, zu entfernen oder zu ersetzen, da Zeichenketten unveränderlich
sind:

.. code-block:: pycon

   >>> welcome[6:-1] = "everybody!"
   Traceback (most recent call last):
     File "<stdin>", line 1, in <module>
   TypeError: 'str' object does not support item assignment

Konvertierungen
---------------

Konvertieren von Zeichenketten in Zahlen
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Ihr könnt die Funktionen :class:`python3:int` und :class:`python3:float`
verwenden, um Zeichenketten in Ganzzahl- bzw. Fließkommazahlen zu konvertieren.
Wenn eine Zeichenkette übergeben wird, die nicht als Zahl des angegebenen Typs
interpretiert werden kann, lösen diese Funktionen eine
:class:`python3:ValueError`-Ausnahme aus. Ausnahmen werden in
:doc:`../../control-flows/exceptions` ausführlicher erklärt. Darüber hinaus
könnt ihr :class:`python3:int` einen optionalen zweiten
:doc:`../../functions/params` übergeben, der die numerische Basis angibt, die
bei der Interpretation der Zeichenfolge verwendet werden soll:

.. code-block:: pycon
   :linenos:

   >>> float("12.34")
   12.34
   >>> float("12e3")
   12000.0
   >>> int("1000")
   1000
   >>> int("1000", base=10)
   1000
   >>> int("1000", 8)
   512
   >>> int("1000", 2)
   8
   >>> int("1234", 2)
   Traceback (most recent call last):
     File "<stdin>", line 1, in <module>
   ValueError: invalid literal for int() with base 2: '1234'

Zeilen 5–8
    Wird kein zweiter :doc:`../../functions/params` angegeben, rechnet
    :class:`python3:int` mit einer Basis von ``10``.
Zeilen 9, 10
    ``1000`` wird als `Oktalzahl <https://de.wikipedia.org/wiki/Oktalsystem>`_
    interpretiert.
Zeilen 11, 12
    ``1000`` wird als `Dualzahl <https://de.wikipedia.org/wiki/Dualsystem>`_
    interpretiert.
Zeilen 13–16
    ``1234`` kann nicht als Ganzzahl auf der Basis ``2`` angegeben werden. Daher
    wird eine :class:`python3:ValueError`-Ausnahme ausgelöst.

Ändern von Zeichenketten mit Listenmanipulationen
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Da :ref:`str <python3:textseq>`-Objekte unveränderlich sind, gibt es keine
Möglichkeit, sie direkt zu verändern wie :doc:`../sequences-sets/lists`. Ihr
könnt sie jedoch in Listen umwandeln:

.. code-block:: pycon

   >>> palindromes = "lol level gag"
   >>> palindromes_list = list(palindromes)
   >>> palindromes_list.reverse()
   >>> "".join(palindromes_list)
   'gag level lol'

Objekte in Zeichenketten konvertieren
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

In Python kann fast alles in eine Zeichenkette mit der eingebauten Funktion
:ref:`str <python3:textseq>` umgewandelt werden:

.. code-block:: pycon

   >>> data_types = [(7, "Data types", 19), (7.1, "Numbers", 19), (7.2, "Lists", 23)]
   >>> (
   ...     "The title of chapter "
   ...     + str(data_types[0][0])
   ...     + " is «"
   ...     + data_types[0][1]
   ...     + "»."
   ... )
   'The title of chapter 7 is «Data types».'

Das Beispiel verwendet :ref:`str <python3:textseq>`, um eine Ganzzahl aus der
Liste ``data_types`` in eine Zeichenkette umzuwandeln, die dann wieder
aneinanderhängt werden, um die endgültige Zeichenkette zu bilden.

.. note::
   Während :ref:`str <python3:textseq>` meist verwendet wird, um für Menschen
   lesbare Texte zu erzeugen, wird :func:`python3:repr` eher für
   Debugging-Ausgaben oder Statusberichte verwendet, :abbr:`z.B. (zum
   Beispiel)`, um Informationen über die eingebaute Python-Funktion
   :func:`python3:len` zu erhalten:

   .. code-block:: pycon

      >>> repr(len)
      '<built-in function len>'

Checks
------

* Könnt ihr :abbr:`z.B. (zum Beispiel)` eine Zeichenkette mit einer ganzen Zahl
  addieren oder multiplizieren, oder mit einer Gleitkommazahl oder einer
  komplexen Zahl?

* Welche der folgenden Zeichenketten können nicht in Zahlen umgewandelt werden
  und warum?

  * ``int("1e2")``
  * ``int(1e+2)``
  * ``int("1+2")``
  * ``int("+2")``
