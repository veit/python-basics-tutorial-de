Daten erstellen
===============

#. Einfügen eines Datensatzes in die Datenbank:

   .. literalinclude:: create_data.py
      :language: python
      :lines: 7-9
      :lineno-start: 7

#. Daten in der Datenbank speichern:

   .. literalinclude:: create_data.py
      :language: python
      :lines: 12
      :lineno-start: 12

#. Fügt mehrere Datensätze mit der sichereren  ``?``-Methode ein, wobei die
   Anzahl der  ``?`` der Anzahl der Spalten entsprechen sollte:

   .. literalinclude:: create_data.py
      :language: python
      :lines: 15-
      :lineno-start: 15
