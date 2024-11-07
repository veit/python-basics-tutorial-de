Daten erstellen
===============

#. Einfügen eines Datensatzes in die Datenbank:

   .. literalinclude:: create_data.py
      :language: python
      :lines: 7-10
      :lineno-start: 7

#. Daten in der Datenbank speichern:

   .. literalinclude:: create_data.py
      :language: python
      :lines: 14
      :lineno-start: 14

#. Fügt mehrere Datensätze mit der sichereren  ``?``-Methode ein, wobei die
   Anzahl der  ``?`` der Anzahl der Spalten entsprechen sollte:

   .. literalinclude:: create_data.py
      :language: python
      :lines: 17-
      :lineno-start: 17
