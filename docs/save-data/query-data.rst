Daten abfragen
==============

#. Alle Datensätze eines Autors auswählen:

   .. literalinclude:: query_data.py
      :language: python
      :lines: 7-11
      :lineno-start: 7

   Für die ``print``-Ausgabe verwenden wir durch ein vorangestelltes ``f`` 
   ein formatiertes Stringliteral oder :term:`python3:f-string`.

#. Alle Daten auswählen und nach Autor sortieren:

   .. literalinclude:: query_data.py
      :language: python
      :lines: 13-16
      :lineno-start: 13

#. Alle Titel auswählen, die Python enthalten:

   .. literalinclude:: query_data.py
      :language: python
      :lines: 18-24
      :lineno-start: 18

#. Schließlich können die Daten abgefragt werden mit:

   .. literalinclude:: query_data.py
      :language: python
      :lines: 28-30
      :lineno-start: 28
