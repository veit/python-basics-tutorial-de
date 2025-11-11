Zeichenketten
=============

Die Verarbeitung von Zeichenketten ist eine der Stärken von Python. Es gibt
viele Optionen zur Begrenzung von Zeichenketten:

.. blacken-docs:off

.. code-block:: python

   "Eine Zeichenfolge in doppelten Anführungszeichen kann 'einfache Anführungszeichen' enthalten."
   'Eine Zeichenfolge in einfachen Anführungszeichen kann "doppelte Anführungszeichen" enthalten.'
   """\tEine Zeichenkette, die mit einem Tabulator beginnt und mit einem Zeilenumbruchzeichen endet.\n"""
   """Dies ist eine Zeichenkette in dreifach doppelten Anführungszeichen, die
   einzige Zeichenkette, die echte Zeilenumbrüche enthält."""

.. blacken-docs:on

Zeichenketten können durch einfache (``' '``), doppelte (``" "``), dreifache
einfache (``''' '''``) oder dreifache doppelte (``""" """``) Anführungszeichen
getrennt werden.

Eine normale Zeichenkette kann nicht auf mehrere Zeilen aufgeteilt werden. Der
folgende Code wird also nicht funktionieren:

.. code-block::

   "Dies ist ein fehlerhafter Versuch, einen Zeilenumbruch in
   eine Zeichenkette einzufügen, ohne \n zu verwenden."

Sie können auch Tabulator- (``\t``) und *Newline*-Zeichen (``\n``) enthalten.
Allgemein können Backslashes ``\`` als Escape-Zeichen verwendet werden. So kann
:abbr:`z.B. (zum Beispiel)` ``\\`` für einen einzelnen Backslash und ``\'`` für
ein einfaches Anführungszeichen verwendet werden, wodurch es die
Zeichenfolge nicht beendet:

.. blacken-docs:off

.. code-block:: python

   "You don't need a backslash here."
   'However, this wouldn\'t work without a backslash.'

.. blacken-docs:on

Python bietet jedoch auch Zeichenketten in dreifachen Anführungszeichen
(``"""``), die dies ermöglichen und einfache und doppelte Anführungszeichen ohne
Backslashes ``\`` als Escape-Zeichen enthalten können.

.. toctree::
   :titlesonly:
   :hidden:

   encodings
   operators-functions
   built-in-modules/index
   print
   input

Checks
------

* Könnt ihr :abbr:`z.B. (zum Beispiel)` eine Zeichenkette mit einer ganzen Zahl
  addieren oder multiplizieren, oder mit einer Gleitkommazahl oder einer
  komplexen Zahl?

* Wie könnt ihr eine Überschrift wie ``variables and expressions`` so abändern,
  dass sie keine Leerzeichen mehr enthält und besser als Dateinamen verwendet
  werden kann?

* Wenn ihr überprüfen wollt, ob eine Zeile mit ``.. note::`` beginnt, welche
  Methode würdet ihr verwenden? Gibt es auch noch andere Möglichkeiten?

* Angenommen, ihr habt eine Zeichenkette mit Ausrufezeichen, Anführungszeichen
  und Zeilenumbruch. Wie können diese aus der Zeichenkette entfernt werden?

* Wie könnt ihr **alle** Leerräume und Satzzeichen aus einer Zeichenfolge in
  einen Bindestrich (``-``) ändern?
