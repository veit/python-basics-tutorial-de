Normalisieren der Daten
=======================

Unter `Normalisierung
<https://de.wikipedia.org/wiki/Normalisierung_(Datenbank)>`_ wird die Aufteilung
von Attributen oder Tabellenspalten in mehrere Relationen oder Tabellen
verstanden, sodass keine Redundanzen mehr enthalten sind.

Beispiel
--------

Im folgenden Beispiel normalisieren wir die Sprache, in der die Bücher
veräffentlicht wurden.

#. Hierfür erstellen wir zunächst eine neue Tabelle ``languages`` mit den
   Spalten ``id`` und ``language_code`` anlegen:

   .. literalinclude:: normalise.py
      :language: python
      :lines: 6-9
      :lineno-start: 6

#. Anschließend legen wir die Werte ``de`` und ``en`` in dieser Tabelle an:

   .. literalinclude:: normalise.py
      :language: python
      :lines: 11-17
      :lineno-start: 11

#. Da SQLite ``MODIFY COLUMN`` nicht unterstützt, legen wir nun eine temporäre
   Tabelle ``temp`` an mit allen Spalten aus ``books`` und einer Spalte
   ``language_code``, die die Spalte ``id`` aus der Tabelle ``languages`` als
   Fremdschlüssel verwendet:

   .. literalinclude:: normalise.py
      :language: python
      :lines: 19-29
      :lineno-start: 19

#. Nun übernehmen wir die Werte aus der ``books``-Tabelle in die
   ``temp``-Tabelle:

   .. literalinclude:: normalise.py
      :language: python
      :lines: 31-33
      :lineno-start: 31

#. Die Angabe der Sprache in ``books`` als ``id`` der Datensätze aus der
   ``languages``-Tabelle in ``temp`` übernehmen.

   .. literalinclude:: normalise.py
      :language: python
      :lines: 35-43
      :lineno-start: 35

#. Nun können wir die Spalte ``languages`` in der Tabelle ``temp`` löschen:

   .. literalinclude:: normalise.py
      :language: python
      :lines: 48-49
      :lineno-start: 48

   .. note::
      Erst ab Python-Versionen ab 3.8, die nach dem 27. April 2021
      veröffentlicht wurden, kann  ``DROP COLUMN`` verwendet werden.

      Bei älteren Python-Versionen müsste eine weitere Tabelle angelegt werden,
      die nicht mehr die Spalte ``languages`` enthält und anschließend die
      Datensätze aus ``templ`` in diese Tabelle eingefügt werden.

#. Auch die ``books``-Tabelle kann nun gelöscht werden:

   .. literalinclude:: normalise.py
      :language: python
      :lines: 51-52
      :lineno-start: 51

#. Und schließlich kann die ``temp``-Tabelle umbenannt werden in ``books``:

   .. literalinclude:: normalise.py
      :language: python
      :lines: 54-55
      :lineno-start: 54
