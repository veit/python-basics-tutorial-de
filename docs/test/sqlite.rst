Beispiel: SQLite-Datenbank testen
=================================

#. Zum Testen, ob die Datenbank ``library.db`` mit :download:`create_db.py
   <../save-data/create_db.py>` angelegt wurde, importieren wir neben
   :doc:`sqlite3 <python3:library/sqlite3>` und :doc:`unittest
   <python3:library/unittest>` auch noch :download:`create_db.py
   <../save-data/create_db.py>` und :doc:`os <python3:library/os>`:

      .. literalinclude:: ../save-data/test_sqlite.py
         :language: python
         :lines: 1-5
         :lineno-start: 1

#. Anschließend definieren wir zunächst eine Testklasse ``TestCreateDB``:

      .. literalinclude:: ../save-data/test_sqlite.py
         :language: python
         :lines: 8
         :lineno-start: 8

#. In ihr definieren wir dann die Testmethode ``test_db_exists``, in der wir mit
   ``assert`` die Annahme treffen, dass die Datei in :doc:`os.path
   <python3:library/os.path>` existiert:

      .. literalinclude:: ../save-data/test_sqlite.py
         :language: python
         :lines: 9-10
         :lineno-start: 9

#. Nun überprüfen wir auch noch, ob die Tabelle ``books`` angelegt wurde.
   Hierfür versuchen wir, die Tabelle erneut anzulegen und erwarten mit
   ``assertRaises``, dass ``sqlite`` mit einem ``OperationalError`` beendet
   wird:

   .. literalinclude:: ../save-data/test_sqlite.py
      :language: python
      :lines: 12-14
      :lineno-start: 12

#. Weitere Tests wollen wir nicht an einer Datenbank im Dateisystem
   durchführen sondern in einer SQLite-Datenbank im Arbeitsspeicher:

   .. literalinclude:: ../save-data/test_sqlite.py
      :language: python
      :lines: 17-20
      :lineno-start: 17

.. seealso::
   Weitere Beispiele zum Testen eurer SQLite-Datenbankfunktionen findet ihr in
   der SQLite Testsuite `test_sqlite3
   <https://github.com/python/cpython/tree/main/Lib/test/test_sqlite3>`_.
