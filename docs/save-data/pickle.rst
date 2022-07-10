Das ``pickle``-Modul
====================

Python kann jede beliebige Datenstruktur in eine Datei schreiben, diese
Datenstruktur wieder aus der Datei lesen und sie mit nur wenigen Befehlen neu
erstellen. Diese Fähigkeit kann sehr nützlich sein, weil sie euch viele Seiten
Code ersparen kann, die nichts anderes tut, als den Zustand eines Programms in
eine Datei zu schreiben und diesen Zustand wieder einzulesen.

Python bietet diese Möglichkeit über das :doc:`pickle
<python3:library/pickle>`-Modul. Pickle ist mächtig, aber einfach zu benutzen.
Nehmen wir an, dass der gesamte Zustand eines Programms in drei Variablen
gespeichert ist: ``a``, ``b`` und ``c``.Ihr könnt diesen Zustand wie folgt in
einer Datei namens ``data.pickle`` speichern:

#. Importieren des ``pickle``-Moduls

   .. literalinclude:: pickle_example.py
      :language: python
      :lines: 1
      :lineno-start: 1

#. Definieren verschiedener Daten

   .. literalinclude:: pickle_example.py
      :language: python
      :lines: 3-5
      :lineno-start: 3

#. Schreiben der Daten:

   .. literalinclude:: pickle_example.py
      :language: python
      :lines: 8-11
      :lineno-start: 8

   Es spielt keine Rolle, was in den Variablen gespeichert wurde. Der Inhalt
   kann so einfach sein wie Zahlen oder so komplex wie eine Liste von
   Wörterbüchern, die Instanzen von benutzerdefinierten Klassen enthalten.
   :py:func:`pickle.dump` speichert alles.

   Das Pickle-Modul kann fast alles auf diese Weise speichern. Es kann mit
   :doc:`/types/numbers`, :doc:`/types/lists`, :doc:`/types/tuples`,
   :doc:`/types/dicts`, :doc:`/types/strings` und so ziemlich allem umgehen, was
   aus diesen Objekttypen besteht, also auch mit allen Klasseninstanzen. Es geht
   auch mit gemeinsam genutzten Objekten, zyklischen Referenzen und anderen
   komplexen Speicherstrukturen korrekt um, indem es gemeinsam genutzte Objekte
   nur einmal speichert und sie als gemeinsam genutzte Objekte wiederherstellt,
   nicht als identische Kopien.

#. Laden der gepickelten Daten:

   Diese Daten können bei einem späteren Programmlauf wieder eingelesen werden
   mit :py:func:`pickle.load`:

   .. literalinclude:: pickle_example.py
      :language: python
      :lines: 14-17
      :lineno-start: 14

#. Ausgeben der gepickelten Daten:

   .. literalinclude:: pickle_example.py
      :language: python
      :lines: 20
      :lineno-start: 20

   .. code-block:: python

      [1, 2.0, (3+4j)] ('character string', b'byte string') {False, None, True}

In den meisten Fällen werdet ihr jedoch nicht eure gesamten Daten in der
gespeicherten Reiehnfolge wiederherstellen wollen. Ein einfacher und effektiver
Weg, nur die Daten von Interesse wiederherzustellen, besteht darin, eine
Speicherfunktion zu schreiben, die alle zu speichernden Daten in einem
Wörterbuch speichert und dann Pickle zum Speichern des Wörterbuchs verwendet.
Anschließend könnt ihr eine ergänzende Wiederherstellungsfunktion verwenden, um
das Wörterbuch wieder einzulesen und die Werte im Wörterbuch den entsprechenden
Programmvariablen zuzuweisen. Wenn ihr diesen Ansatz mit dem vorherigen Beispiel
verwendet, erhaltet ihr folgenden Code:

.. literalinclude:: pickle_example2.py
   :language: python
   :lines: 1-12
   :lineno-start: 1

Anschließend könnt ihr gezielt die Daten aus ``c`` ausgeben mit

.. literalinclude:: pickle_example2.py
   :language: python
   :lines: 15-19
   :lineno-start: 15

.. code-block:: python

   {False, None, True}

Neben :py:func:`pickle.dump` und :py:func:`pickle.load` gibt es auch noch die
Funktionen :py:func:`pickle.dumps` und :py:func:`pickle.loads`. Das
angehängte ``s`` verweist darauf, dass diese Funktionen Strings verarbeiten.

.. warning::
   Obwohl die Verwendung eines gepickelten Objekts im vorherigen Szenario
   durchaus sinnvoll sein kann, solltet ihr euch auch der Nachteile von Pickles
   bewusst sein:

   * Pickling ist weder besonders schnell noch platzsparend als Mittel zur
     Serialisierung. Selbst die Verwendung von :doc:`json
     <python3:library/json>` zur Speicherung serialisierter Objekte ist
     schneller und führt zu kleineren Dateien auf der Festplatte.
   * Pickling ist nicht sicher, und das Laden eines Pickles mit bösartigem
     Inhalt kann zur Ausführung von beliebigem Code auf eurem Rechner führen.
     Daher solltet ihr das Pickling vermeiden, wenn die Möglichkeit besteht,
     dass die Pickle-Datei für jemanden zugänglich ist, der sie verändern
     könnte.
   * Pickle-Versionen sind nicht immer rückwärtskompatibel.

.. seealso::
   * :doc:`Python-Module-Dokumentation <python3:library/pickle>`
   * `Using Pickle <https://wiki.python.org/moin/UsingPickle>`_
