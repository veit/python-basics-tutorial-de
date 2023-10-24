Debugging von Testfehlern
=========================

Wenn Tests fehlschlagen, müssen wir herausfinden, warum. Vielleicht liegt es am
Test, vielleicht aber auch an der Anwendung. Der Prozess, um herauszufinden, wo
das Problem liegt und was man dagegen tun kann, ist ähnlich.

pytest bietet viele Werkzeuge, die uns helfen können, ein Problem schneller zu
lösen, ohne dass wir zu einem Debugger greifen müssen. Python enthält einen
eingebauten Quellcode-Debugger namens ``pdb``, sowie mehrere Optionen, die das
Debuggen mit ``pdb`` schnell und einfach machen.

Im Folgenden werden wir einige fehlerhafte Codes mit Hilfe von pytest-Optionen
und ``pdb`` debuggen und uns dabei die Debugging-Optionen und die Integration
von pytest und ``pdb`` anzusehen. Hierzu benötigen wir einen fehlgeschlagenen
Test. Dazu fügen wir in unserem Items-Projekt eine Funktion und einige Tests
hinzu:

Nehmen wir an, wir verwenden Cards schon eine Weile und haben nun einige Aufgaben abgeschlossen:

.. code-block:: console

   $ items list

   ID   state         owner   summary
   ────────────────────────────────────────────────────────
   1    done          veit    Update pytest section
   2    in progress   veit    Update cibuildwheel section
   3    todo          veit    Update mock tests

Wenn wir alle erledigten Aufgaben auflisten wollen, können wir dies bereits mit
``items list`` machen, weil sie einige Filterfunktionen hat:

.. code-block:: console

   $ items list --help

    List the items in the db.

   ╭─ Options ──────────────────────────────────────────────────────────────────────────────────────────────────────────────────╮
   │ --owner  -o      TEXT  [default: None]                                                                                     │
   │ --state  -s      TEXT  [default: None]                                                                                     │
   │ --help                 Show this message and exit.                                                                         │
   ╰────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯


Optionen
--------

+-----------------------+-----------------------------------------------+
| Optionen              | Beschreibung                                  |
+=======================+===============================================+
| :samp:`-s`            | stummschalten                                 |
+-----------------------+-----------------------------------------------+
| :samp:`-v`            | verbos, :samp:`-vv` ist noch ausführlicher    |
+-----------------------+-----------------------------------------------+
| :samp:`--tb`          | Traceback-Stil:                               |
|                       | :samp:`auto|long|short|line|native|no|`       |
|                       |                                               |
|                       | Üblicherweise nutze ich :samp:`--tb=short`    |
|                       | als Standardeinstellung in der                |
|                       | Konfigurationsdatei und die anderen für die   |
|                       | Fehlersuche.                                  |
+-----------------------+-----------------------------------------------+
| :samp:`-x`            | hält beim ersten Fehler an                    |
+-----------------------+-----------------------------------------------+
| :samp:`--lf`          | führt den zuerst fehlgeschlagenen Test aus    |
+-----------------------+-----------------------------------------------+
| :samp:`--ff`          | startet mit dem zuerst fehlgeschlagenen Test  |
|                       | und führt dann alle aus.                      |
+-----------------------+-----------------------------------------------+
| :samp:`--nf`          | führt zuerst neue Testdateien aus, dann den   |
|                       | Rest sortiert nach Änderungsdatum.            |
+-----------------------+-----------------------------------------------+
| :samp:`--sw`          | führt den letzten fehlgeschlagenen Test aus,  |
|                       | stoppt dann beim nächsten Fehler und startet  |
|                       | beim nächsten Mal wieder beim letzten         |
|                       | fehlgeschlagenen Test. Ähnlich wie die        |
|                       | Kombination von :samp:`--lf -x`, aber         |
|                       | effizienter.                                  |
+-----------------------+-----------------------------------------------+
| :samp:`--sw-skip`     | wie oben, aber ein fehlgeschlagener Test      |
|                       | wird übersprungen.                            |
+-----------------------+-----------------------------------------------+
| :samp:`--pdb`         | startet den Python-Debugger im Fehlerfall.    |
|                       |                                               |
|                       | Sehr nützlich zum Debuggen mit :doc:`../tox`. |
+-----------------------+-----------------------------------------------+
