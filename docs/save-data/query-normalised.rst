Abfragen normalisierter Daten
=============================

#. Abfragen aller Bücher sortiert nach ``language_id`` und ``title``:

   .. literalinclude:: query_normalised.py
      :language: python
      :lines: 7-13
      :lineno-start: 7

   .. code-block:: rest

    All books ordered by language id and title:
    (1, 'Veit Schiele', 'Jupyter Tutorial')
    (2, 'Veit Schiele', 'Jupyter Tutorial')
    (2, 'Veit Schiele', 'PyViz Tutorial')
    (2, 'Veit Schiele', 'Python basics')

#. Um nun nicht nur die ID der Sprachen zu erhalten sondern die zugehörigen
   Sprachcodes wird mit ``JOIN`` über die ``id``-Spalte in der
   ``languages``-Tabelle eine Verbindung zu den dort hinterlegten Sprachcodes
   hergestellt:

   .. literalinclude:: query_normalised.py
      :language: python
      :lines: 16-24
      :lineno-start: 16

   .. code-block:: rest

    All books ordered by language code and title:
    ('de', 'Veit Schiele', 'Jupyter Tutorial')
    ('en', 'Veit Schiele', 'Jupyter Tutorial')
    ('en', 'Veit Schiele', 'PyViz Tutorial')
    ('en', 'Veit Schiele', 'Python basics')
