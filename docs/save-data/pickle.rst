Das ``pickle``-Modul
====================

Wenn ihr Python-Objekte serialisieren wollt, könnt ihr einfach das
Python-:doc:`pickle <python3:library/pickle>`-Modul verwenden, das mit Python
mitgeliefert wird.

.. note::
   Beachtet dabei jedoch bitte, dass Pickle nicht sicher ist, ihr also keine
   Daten verarbeiten solltet, die aus nicht vertrauenswürdigen Quellen kommen.

   Zudem sind Pickle-Versionen nicht immer rückwärtskompatibel.

Hier ein Beispiel für ein Python-Dict, das mehrere Datentypen enthält:

#. Importieren des ``pickle``-Moduls

   .. literalinclude:: pickle_example.py
      :language: python
      :lines: 1
      :lineno-start: 1

#. Serialisieren des Python-Objekts mit ``pickle``:

   .. literalinclude:: pickle_example.py
      :language: python
      :lines: 4-8
      :lineno-start: 4

#. Schreiben der serialisierten Daten:

   .. literalinclude:: pickle_example.py
      :language: python
      :lines: 10-11
      :lineno-start: 10

#. Laden der gepickelten Daten:

   .. literalinclude:: pickle_example.py
      :language: python
      :lines: 14-15
      :lineno-start: 14

#. Ausgeben der gepickelten Daten:

   .. literalinclude:: pickle_example.py
      :language: python
      :lines: 18
      :lineno-start: 18

   .. code-block:: python

      {'a': [1, 2.0, (3+4j)], 'b': ('character string', b'byte string'), 'c': {False, True, None}}

Neben :py:func:`pickle.dump` und :py:func:`pickle.load` gibt es auch noch die
Funktionen :py:func:`pickle.dumps` und :py:func:`pickle.loads`. Das angehängte
``s`` verweist darauf, dass diese Funktionen Strings verarbeiten.

.. seealso::
   * :doc:`Python-Module-Dokumentation <python3:library/pickle>`
   * `Using Pickle <https://wiki.python.org/moin/UsingPickle>`_
