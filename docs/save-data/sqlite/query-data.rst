Daten abfragen
==============

#. Alle Datensätze eines Autors auswählen:

   .. literalinclude:: query_data.py
      :language: python
      :lines: 7-12
      :lineno-start: 7

   Für die ``print``-Ausgabe verwenden wir durch ein vorangestelltes ``f``
   ein formatiertes String-Literal oder :term:`python3:f-string`.

#. Alle Daten auswählen und nach Autor sortieren:

   .. literalinclude:: query_data.py
      :language: python
      :lines: 15-18
      :lineno-start: 15

#. Alle Titel auswählen, die Python enthalten:

   .. literalinclude:: query_data.py
      :language: python
      :lines: 21-27
      :lineno-start: 21

#. Schließlich können die Daten abgefragt werden mit:

   .. literalinclude:: query_data.py
      :language: python
      :lines: 30-
      :lineno-start: 30

   .. code-block:: pycon

      >>> import create_db
      >>> import create_data_from_csv
      >>> import query_data
      All books from Veit Schiele:
      ('Python basics', 'en', 'Veit Schiele', 'BSD-3-Clause', '2021-10-28')
      ('Jupyter Tutorial', 'en', 'Veit Schiele', 'BSD-3-Clause', '2019-06-27')
      ('Jupyter Tutorial', 'de', 'Veit Schiele', 'BSD-3-Clause', '2020-10-26')
      ('PyViz Tutorial', 'en', 'Veit Schiele', 'BSD-3-Clause', '2020-04-13')
      Listing of all books sorted by author:
      ('Python basics', 'en', 'Veit Schiele', 'BSD-3-Clause', '2021-10-28')
      ('Jupyter Tutorial', 'en', 'Veit Schiele', 'BSD-3-Clause', '2019-06-27')
      ('Jupyter Tutorial', 'de', 'Veit Schiele', 'BSD-3-Clause', '2020-10-26')
      ('PyViz Tutorial', 'en', 'Veit Schiele', 'BSD-3-Clause', '2020-04-13')
      All books with Python in the title:
      [('Python basics', 'en', 'Veit Schiele', 'BSD-3-Clause', '2021-10-28')]
