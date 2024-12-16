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
gespeichert ist: ``a``, ``b`` und ``c``. Ihr könnt diesen Zustand wie folgt in
einer Datei namens ``data.pickle`` speichern:

#. Importieren des ``pickle``-Moduls

   .. code-block:: pycon

      >>> import pickle

#. Definieren verschiedener Daten

   .. code-block:: pycon

      >>> a = [1, 2.0, 3 + 4j]
      >>> b = ("character string", b"byte string")
      >>> c = {None, True, False}

#. Schreiben der Daten:

   .. code-block:: pycon

      >>> with open("data.pickle", "wb") as f:
      ...     pickle.dump(a, f)
      ...     pickle.dump(b, f)
      ...     pickle.dump(c, f)
      ...

   Es spielt keine Rolle, was in den Variablen gespeichert wurde. Der Inhalt
   kann so einfach sein wie Zahlen oder so komplex wie eine Liste von
   Wörterbüchern, die Instanzen von benutzerdefinierten Klassen enthalten.
   :py:func:`pickle.dump` speichert alles.

   Das Pickle-Modul kann fast alles auf diese Weise speichern. Es kann mit
   :doc:`/types/numbers/index`, :doc:`/types/lists`, :doc:`/types/tuples`,
   :doc:`/types/dicts`, :doc:`/types/strings/index` und so ziemlich allem
   umgehen, was aus diesen Objekttypen besteht, also auch mit allen
   Klasseninstanzen. Es geht auch mit gemeinsam genutzten Objekten, zyklischen
   Referenzen und anderen komplexen Speicherstrukturen korrekt um, indem es
   gemeinsam genutzte Objekte nur einmal speichert und sie als gemeinsam
   genutzte Objekte wiederherstellt, nicht als identische Kopien.

#. Laden der gepickelten Daten:

   Diese Daten können bei einem späteren Programmlauf wieder eingelesen werden
   mit :py:func:`pickle.load`:

   .. code-block:: pycon

      >>> with open("data.pickle", "rb") as f:
      ...     first = pickle.load(f)
      ...     second = pickle.load(f)
      ...     third = pickle.load(f)
      ...

#. Ausgeben der gepickelten Daten:

   .. code-block:: pycon

      >>> print(first, second, third)
      [1, 2.0, (3+4j)] ('character string', b'byte string') {False, None, True}

In den meisten Fällen werdet ihr jedoch nicht eure gesamten Daten in der
gespeicherten Reihenfolge wiederherstellen wollen. Ein einfacher und effektiver
Weg, nur die Daten von Interesse wiederherzustellen, besteht darin, eine
Speicherfunktion zu schreiben, die alle zu speichernden Daten in einem
Wörterbuch speichert und dann Pickle zum Speichern des Wörterbuchs verwendet.
Anschließend könnt ihr eine ergänzende Wiederherstellungsfunktion verwenden, um
das Wörterbuch wieder einzulesen und die Werte im Wörterbuch den entsprechenden
Programmvariablen zuzuweisen. Wenn ihr diesen Ansatz mit dem vorherigen Beispiel
verwendet, erhaltet ihr folgenden Code:

   .. code-block:: pycon

      >>> def save():
      ...     # Serialise Python objects
      ...     data = {"a": a, "b": b, "c": c}
      ...     # File with pickles
      ...     with open("data.pickle", "wb") as f:
      ...         pickle.dump(data, f)
      ...

Anschließend könnt ihr gezielt die Daten aus ``c`` ausgeben mit

.. code-block:: pycon

   >>> with open("data.pickle", "rb") as f:
   ...     saved_data = pickle.load(f)
   ...     print(saved_data["c"])
   ...
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
