Das ``psycopg``-Modul
=====================

#. Installiert das psycopg-Modul:

   .. code-block:: console

      $ uv add psycopg
      Resolved 3 packages in 4ms
      Built psycopg-env @ file:///Users/veit/sandbox/psycopg_env
      Prepared 1 package in 7ms
      Uninstalled 1 package in 0.96ms
      Installed 2 packages in 5ms
       + psycopg==3.3.4
       ~ psycopg-env==0.1.0 (from file:///Users/veit/cusy/trn/python-basics-tutorial)

#. Importiert das psycopg-Modul:

   .. literalinclude:: psycopg.py
      :language: python
      :lines: 1
      :linenos:

#. Erstellt eine Datenbank:

   .. literalinclude:: psycopg.py
      :language: python
      :lines: 3-4
      :lineno-start: 3

#. Abfragen der Datenbank:

   .. literalinclude:: psycopg.py
      :language: python
      :lines: 7-8
      :lineno-start: 7

#. Zeiger und Verbindung schließen:

   .. literalinclude:: psycopg.py
      :language: python
      :lines: 11-12
      :lineno-start: 11
