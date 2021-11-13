Das ``psycopg``-Modul
=====================

#. Installiert das psycopg-Modul:

   .. tabs::

      .. tab:: Linux/MacOS

         .. code-block:: console

            $ python3 -m pip install psycopg
            Collecting psycopg
              Downloading psycopg-3.0.1-py3-none-any.whl (140 kB)
                 |████████████████████████████████| 140 kB 3.4 MB/s            
            Installing collected packages: psycopg
            Successfully installed psycopg-3.0.1

      .. tab:: Windows

         .. code-block:: ps1con

            C:> python -m pip install psycopg
            Collecting psycopg
              Downloading psycopg-3.0.1-py3-none-any.whl (140 kB)
                 |████████████████████████████████| 140 kB 3.4 MB/s            
            Installing collected packages: psycopg
            Successfully installed psycopg-3.0.1

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

#. Abragen der Datenbank:

   .. literalinclude:: psycopg.py
      :language: python
      :lines: 7-8
      :lineno-start: 7

#. Zeiger und Verbindung schließen:

   .. literalinclude:: psycopg.py
      :language: python
      :lines: 11-12
      :lineno-start: 11
