Daten abfragen
==============

#. Alle Datensätze eines Autors auswählen:

   .. literalinclude:: query_data.py
      :language: python
      :lines: 7-11
      :lineno-start: 7

   Für die ``print``-Ausgabe verwenden wir durch ein vorangestelltes ``f`` 
   ein formatiertes Stringliteral oder `f-string
   <https://docs.python.org/3/reference/lexical_analysis.html#formatted-string-literals>`_.

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
