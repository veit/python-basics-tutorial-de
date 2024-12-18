``re``
======

Die Python-Standard-Bibliothek :doc:`re <python3:library/re>` enthält ebenfalls
Funktionen für die Arbeit mit Zeichenketten. Dabei bietet ``re`` ausgefeiltere
Möglichkeiten zur Musterextraktion und -ersetzung als der
:ref:`str <python3:textseq>`-Typ.

.. code-block:: pycon

   >>> import re
   >>> re.sub("\n", "", welcome)
   'Hello pythonistas!'

Hier wird der reguläre Ausdruck zunächst kompiliert und dann seine
:py:meth:`re.Pattern.sub`-Methode für den übergebenen Text aufgerufen. Ihr könnt
den Ausdruck selbst mit :py:func:`re.compile` kompilieren und so ein
wiederverwendbares ``regex``-Objekt bilden, das auf unterschiedliche
Zeichenketten angewendet die CPU-Zyklen verringert:

.. code-block:: pycon

   >>> regex = re.compile("\n")
   >>> regex.sub("", welcome)
   'Hello pythonistas!'

Wenn ihr stattdessen eine Liste aller Muster erhalten möchtet, die dem
``regex``-Objekt entsprechen, könnt ihr die
:py:meth:`re.Pattern.findall`-Methode verwenden:

.. code-block:: pycon

   >>> regex.findall(welcome)
   ['\n']

.. note::
   Um das umständliche Escaping mit ``\`` in einem regulären Ausdruck zu
   vermeiden, könnt ihr rohe String-Literale wie ``r'C:\PATH\TO\FILE'``
   anstelle des  entsprechenden ``'C:\\PATH\\TO\\FILE'`` verwenden.

:py:meth:`re.Pattern.match` und :py:meth:`re.Pattern.search` sind eng mit
:py:meth:`re.Pattern.findall` verwandt. Während ``findall`` alle
Übereinstimmungen in einer Zeichenkette zurückgibt, gibt ``search`` nur die
erste Übereinstimmung und ``match`` nur Übereinstimmungen am Anfang der
Zeichenkette zurück. Als weniger triviales Beispiel betrachten wir einen
Textblock und einen regulären Ausdruck, der die meisten E-Mail-Adressen
identifizieren kann:

.. code-block:: pycon

   >>> addresses = """Veit <veit@cusy.io>
   ... Veit Schiele <veit.schiele@cusy.io>
   ... cusy GmbH <info@cusy.io>
   ... """
   >>> pattern = r"[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,4}"
   >>> regex = re.compile(pattern, flags=re.IGNORECASE)
   >>> regex.findall(addresses)
   ['veit@cusy.io', 'veit.schiele@cusy.io', 'info@cusy.io']
   >>> regex.search(addresses)
   <re.Match object; span=(6, 18), match='veit@cusy.io'>
   >>> print(regex.match(addresses))
   None

``regex.match`` gibt ``None`` zurück, da das Muster nur dann passt, wenn es am
Anfang der Zeichenkette steht.

Angenommen, ihr möchtet E-Mail-Adressen finden und gleichzeitig jede Adresse in
ihre drei Komponenten aufteilen:

#. Personen-Name
#. Domänen-Name
#. Domänen-Suffix

Dazu setzt ihr zunächst runde Klammern ``()`` um die zu segmentierenden Teile
des Musters:

.. code-block:: pycon

   >>> pattern = r"([A-Z0-9._%+-]+)@([A-Z0-9.-]+)\.([A-Z]{2,4})"
   >>> regex = re.compile(pattern, flags=re.IGNORECASE)
   >>> match = regex.match("veit@cusy.io")
   >>> match.groups()
   ('veit', 'cusy', 'io')

:py:meth:`re.Match.groups` gibt ein :doc:`../../sequences-sets/tuples` zurück,
das alle Untergruppen der Übereinstimmung enthält.

:py:meth:`re.Pattern.findall` gibt eine Liste von Tupeln zurück, wenn das Muster
Gruppen enthält:

.. code-block:: pycon

   >>> regex.findall(addresses)
   [('veit', 'cusy', 'io'), ('veit.schiele', 'cusy', 'io'), ('info', 'cusy', 'io')]

Auch in :py:meth:`re.Pattern.sub` können Gruppen verwendet werden wobei ``\1``
für die erste übereinstimmende Gruppe steht, ``\2`` für die zweite :abbr:`usw.
(und so weiter)`:

.. code-block:: pycon

   >>> regex.findall(addresses)
   [('veit', 'cusy', 'io'), ('veit.schiele', 'cusy', 'io'), ('info', 'cusy', 'io')]
   >>> print(regex.sub(r"Username: \1, Domain: \2, Suffix: \3", addresses))
   Veit <Username: veit, Domain: cusy, Suffix: io>
   Veit Schiele <Username: veit.schiele, Domain: cusy, Suffix: io>
   cusy GmbH <Username: info, Domain: cusy, Suffix: io>

Die folgende Tabelle enthält einen kurzen Überblick über Methoden für reguläre
Ausdrücke:

+-------------------------------+-------------------------------------------------------------------------------+
| Methode                       | Beschreibung                                                                  |
+===============================+===============================================================================+
| :py:func:`re.findall`         | gibt alle sich nicht überschneidenden übereinstimmenden Muster in einer       |
|                               | Zeichenkette als Liste zurück.                                                |
+-------------------------------+-------------------------------------------------------------------------------+
| :py:func:`re.finditer`        | wie ``findall``, gibt aber einen Iterator zurück.                             |
+-------------------------------+-------------------------------------------------------------------------------+
| :py:func:`re.match`           | entspricht dem Muster am Anfang der Zeichenkette und segmentiert optional die |
|                               | Musterkomponenten in Gruppen; wenn das Muster übereinstimmt, wird ein         |
|                               | ``match``-Objekt zurückgegeben, andernfalls keines.                           |
+-------------------------------+-------------------------------------------------------------------------------+
| :py:func:`re.search`          | durchsucht die Zeichenkette nach Übereinstimmungen mit dem Muster; gibt in    |
|                               | diesem Fall ein ``match``-Objekt zurück; im Gegensatz zu ``match`` kann die   |
|                               | Übereinstimmung an einer beliebigen Stelle der Zeichenkette und nicht nur am  |
|                               | Anfang stehen.                                                                |
+-------------------------------+-------------------------------------------------------------------------------+
| :py:func:`re.split`           | zerlegt die Zeichenkette bei jedem Auftreten des Musters in Teile.            |
+-------------------------------+-------------------------------------------------------------------------------+
| :py:func:`re.sub`,            | ersetzt alle (``sub``) oder die ersten ``n`` Vorkommen (``subn``) des Musters |
| :py:func:`re.subn`            | in der Zeichenkette durch einen Ersetzungsausdruck; verwendet die Symbole     |
|                               | ``\1``, ``\2``, …, um auf die Elemente der Übereinstimmungsgruppe zu          |
|                               | verweisen.                                                                    |
+-------------------------------+-------------------------------------------------------------------------------+
| :py:meth:`str.removeprefix`   | In Python 3.9 kann dies verwendet werden, um das Suffix oder den Dateinamen   |
| :py:meth:`str.removesuffix`   | zu extrahieren.                                                               |
+-------------------------------+-------------------------------------------------------------------------------+


.. seealso::
   * :doc:`regex`
   * :doc:`python3:howto/regex`
   * :doc:`python3:library/re`

Checks
------

* Welchen regulären Ausdruck würdet ihr verwenden, um Zeichenfolgen zu finden,
  die die Zahlen zwischen -3 und +3 darstellen?
* Welchen regulären Ausdruck würdet ihr verwenden, um Hexadezimalwerte zu
  finden?
