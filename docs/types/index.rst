Datentypen
==========

Python hat mehrere eingebaute Datentypen, von Skalaren wie Zahlen und boolschen
Werten bis hin zu komplexeren Strukturen wie Sequenzen, Sets, Dictionaries und
Strings.

+-----------------------+-----------------------------------------------+
| Datentyp              | Beispiele                                     |
+=======================+===============================================+
| Numerische Typen      | :class:`int`, :class:`float`, :class:`complex`|
+-----------------------+-----------------------------------------------+
| Boolscher Typ         | :class:`bool`                                 |
+-----------------------+-----------------------------------------------+
| Sequenzen             | :class:`list`, :class:`tuple`                 |
+-----------------------+-----------------------------------------------+
| Sets                  | :class:`set`, :class:`frozenset`              |
+-----------------------+-----------------------------------------------+
| Mappings              | :class:`dict`                                 |
+-----------------------+-----------------------------------------------+
| Strings               | :class:`str`                                  |
+-----------------------+-----------------------------------------------+
| Dateien               | :func:`open <open>`                           |
+-----------------------+-----------------------------------------------+

Diese Datentypen können mit Hilfe von Sprachoperatoren, eingebauten Funktionen,
Bibliotheksfunktionen oder den eigenen Methoden eines Datentyps manipuliert
werden.

Ihr könnt auch eure eigenen Klassen definieren und eigene Klasseninstanzen
erstellen. Für diese Klasseninstanzen könnt ihr Methoden definieren sowie mit
den Sprachoperatoren und eingebauten Funktionen, für die ihr die entsprechenden
speziellen Methodenattribute definiert habt, bearbeitet werden.

.. note::
   In der Python-Dokumentation und in diesem Buch wird der Begriff *Objekt* für
   Instanzen beliebiger Python-Datentypen verwendet, nicht nur für das, was
   viele andere Sprachen als Klasseninstanzen bezeichnen würden. Das liegt
   daran, dass alle Python-Objekte Instanzen der einen oder anderen Klasse sind.

.. seealso::
   * :doc:`python3:library/stdtypes`

.. toctree::
   :titlesonly:
   :hidden:

   numbers
   lists
   tuples
   sets
   dicts
   strings/index
   files
   none
