Daten abfragen
==============

#. Alle Datensätze eines Autors auswählen:

   .. literalinclude:: query_data.py
      :language: python
      :lines: 6-10
      :lineno-start: 6

   Für die ``print``-Ausgabe verwenden wir durch ein vorangestelltes ``f`` 
   ein formatiertes Stringliteral oder :term:`python3:f-string`.

#. Alle Daten auswählen und nach Autor sortieren:

   .. literalinclude:: query_data.py
      :language: python
      :lines: 12-15
      :lineno-start: 12

#. Alle Titel auswählen, die Python enthalten:

   .. literalinclude:: query_data.py
      :language: python
      :lines: 17-23
      :lineno-start: 17

#. Schließlich können die Daten abgefragt werden mit:

   .. literalinclude:: query_data.py
      :language: python
      :lines: 26
      :lineno-start: 26
