Daten aus csv erstellen
=======================

#. Die Module sqlite und csv importieren

   .. literalinclude:: create_data_from_csv.py
      :language: python
      :lines: 1-2
      :lineno-start: 1

#. Zeigen auf die Bibliotheksdatenbank

   .. literalinclude:: create_data_from_csv.py
      :language: python
      :lines: 4-5
      :lineno-start: 4

#. Lest die csv-Datei und fügt die Datensätze in die Datenbank ein:

   .. literalinclude:: create_data_from_csv.py
      :language: python
      :lines: 8-9,11
      :lineno-start: 8

#. Speichert die Daten in der Datenbank:

   .. literalinclude:: create_data_from_csv.py
      :language: python
      :lines: 14
      :lineno-start: 14
