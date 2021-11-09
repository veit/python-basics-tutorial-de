Das ``pickle``-Modul
====================

Wenn ihr Python-Objekte serialisieren wollt, könnt ihr einfach das
Python-:doc:`pickle <python3:library/pickle>`-Modul verwenden.

.. note::
   Beachtet dabei jedoch bitte, dass Pickle nicht sicher ist und
   Pickle-Versionen auch nicht immer rückwärtskompatibel sind.

Hier ein Beispiel für ein Python-Dict, das mehrere Datentypen enthält:

#. Importieren des ``pickle``-Moduls

   .. literalinclude:: pickle-example.py
      :language: python
      :lines: 1
      :lineno-start: 1

#. Serialisieren des Python-Objekts mit ``pickle``:

   .. literalinclude:: pickle-example.py
      :language: python
      :lines: 4-8
      :lineno-start: 4

#. Laden der gepickelten Daten:

   .. literalinclude:: pickle-example.py
      :language: python
      :lines: 14-15
      :lineno-start: 14

#. Ausgeben der gepickelten Daten:

   .. literalinclude:: pickle-example.py
      :language: python
      :lines: 18
      :lineno-start: 18

   .. code-block:: python

      {'a': [1, 2.0, (3+4j)], 'b': ('character string', b'byte string'), 'c': {False, True, None}}

