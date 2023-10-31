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
von pytest und ``pdb`` anzusehen.

Debuggen mit pytest-Optionen
----------------------------

pytest enthält eine ganze Reihe von Kommandozeilen-Optionen, die für die
Fehlersuche nützlich sind. Wir werden einige davon verwenden, um unsere
Testfehler zu beheben.
Optionen für die Auswahl, welche Tests in welcher Reihenfolge ausgeführt werden
sollen und wann sie gestoppt werden sollen.

In all diesen Beschreibungen bezieht sich der Begriff *Fehler* auf eine
fehlgeschlagene *Assertion*  oder eine andere nicht abgefangene *Exception*, die
in unserem Quell- oder Testcode, einschließlich der Fixtures, gefunden wurde.

#. Erneute Ausführung fehlgeschlagener Tests

   Beginnen wir mit der Fehlersuche, indem wir sicherstellen, dass die Tests
   fehlschlagen, wenn wir sie erneut ausführen. Hierfür verwenden wir ``--lf``,
   um nur die fehlgeschlagenen Tests erneut auszuführen, und ``--tb=no``, um den
   Traceback auszublenden. So wissen wir , dass wir den Fehler reproduzieren
   können.

   #. Nun können wir mit dem Debuggen des ersten Fehlers beginnen und führen
      hierzu den ersten fehlgeschlagenen Testaus, halten nach dem Fehler an und
      sehen uns den Traceback an: ``pytest --lf -x``.

   #.  Um sicher zu gehen, dass wir das Problem verstehen, können wir den
       gleichen Test mit ``-l``/``--showlocals`` noch einmal durchführen. Wir
       brauchen den vollständigen Traceback nicht noch einmal, also können wir
       ihn mit ``--tb=short`` kürzen: ``pytest --lf -x -l --tb=short``.

       ``-l``/``--showlocals`` sind oft sehr hilfreich und manchmal gut genug,
       um einen Testfehler vollständig zu erkennen.

#. Fehlersuche mit pdb

   :abbr:`pdb (Python Debugger)` ist Teil der Python Standardbibliothek, so dass
   wir nichts installieren müssen, um es zu benutzen. Ihr könnt pdb von pytest
   aus auf verschiedene Weise starten:

   - Fügt einen ``breakpoint()``-Aufruf entweder zum Test- oder zum
     Anwendungscode hinzu. Wenn ein pytest Lauf auf einen
     ``breakpoint()``-Funktionsaufruf trifft, wird er dort anhalten und pdb
     starten.
   - Verwendet die ``--pdb``-Option. Mit ``--pdb`` hält pytest an der Stelle des
     Fehlers an.
   - Verwendet die Kombination der ``--lf``  und ``--trace``-Optionen. Mit
     ``--trace`` hält pytest am Anfang eines jeden Tests.

     Nachfolgend sind die üblichen Befehle aufgeführt, die von pdb erkannt
     werden:

     +-------------------------------+-----------------------------------------------+
     | Optionen                      | Beschreibung                                  |
     +===============================+===============================================+
     | Meta-Befehle                                                                  |
     +-------------------------------+-----------------------------------------------+
     | :samp:`h(elp)`                | gibt eine Liste von Befehlen aus.             |
     +-------------------------------+-----------------------------------------------+
     | :samp:`h(elp) {COMMAND}`      | gibt die Hilfe zu einem Befehl aus.           |
     +-------------------------------+-----------------------------------------------+
     | :samp:`q(uit)`                | beendet pdb.                                  |
     +-------------------------------+-----------------------------------------------+
     | Sehen, wo ihr seid                                                            |
     +-------------------------------+-----------------------------------------------+
     | :samp:`l(ist)`                | listet elf Zeilen um die aktuelle Zeile auf;  |
     |                               | beim erneuten Aufruf werden die nächsten elf  |
     |                               | Zeilen aufgelistet.                           |
     +-------------------------------+-----------------------------------------------+
     | :samp:`l(ist) .`              | Das Gleiche wie oben, aber mit einem Punkt.   |
     |                               | Listet elf Zeilen um die aktuelle Zeile auf.  |
     |                               | Praktisch, wenn ihr :samp:`l(list)` ein paar  |
     |                               | Mal benutzt habt und eure aktuelle Position   |
     |                               | verloren habt.                                |
     +-------------------------------+-----------------------------------------------+
     | :samp:`l(ist) first|last`     | listet eine bestimmte Gruppe von Zeilen auf.  |
     +-------------------------------+-----------------------------------------------+
     | :samp:`ll`                    | listet den gesamten Quellcode für die         |
     |                               | aktuelle Funktion auf.                        |
     +-------------------------------+-----------------------------------------------+
     | :samp:`w(here)`               | gibt den Stack-Trace aus.                     |
     +-------------------------------+-----------------------------------------------+
     | Werte ansehen                                                                 |
     +-------------------------------+-----------------------------------------------+
     | :samp:`p(rint) {EXPR}`        | wertet :samp:`{EXPR}` aus und gibt Wert aus.  |
     +-------------------------------+-----------------------------------------------+
     | :samp:`pp {EXPR}`             | entspricht :samp:`p(rint) {EXPR}`, verwendet  |
     |                               | aber ``pretty-print`` aus dem                 |
     |                               | :doc:`pprint <python3:library/pprint>`-Modul. |
     +-------------------------------+-----------------------------------------------+
     | :samp:`a(rgs)`                | gibt die Argumentliste der aktuellen Funktion |
     |                               | aus.                                          |
     +-------------------------------+-----------------------------------------------+
     | Ausführungsbefehle                                                            |
     +-------------------------------+-----------------------------------------------+
     | :samp:`s(tep)`                | führt die aktuelle Zeile aus und springt zur  |
     |                               | nächsten Zeile in Ihrem Quellcode, auch wenn  |
     |                               | sie sich innerhalb einer Funktion befindet.   |
     +-------------------------------+-----------------------------------------------+
     | :samp:`n(ext)`                | führt die aktuelle Zeile aus und springt zur  |
     |                               | nächsten Zeile in der aktuellen Funktion.     |
     +-------------------------------+-----------------------------------------------+
     | :samp:`c(ontinue)`            | wird bis zum nächsten Haltepunkt fortgesetzt. |
     |                               | Bei Verwendung mit ``--trace`` bis zum        |
     |                               | Beginn des nächsten Tests fortgesetzt.        |
     +-------------------------------+-----------------------------------------------+
     | :samp:`unt(il) {LINENO}`      | wird bis zur angegebenen Zeilennummer         |
     |                               | fortgesetzt.                                  |
     +-------------------------------+-----------------------------------------------+

     .. seealso::
        Die vollständige Liste findet ihr in `Debugger Commands
        <https://docs.python.org/3/library/pdb.html#debugger-commands>`_ der
        pdb-Dokumentation.

Kombinieren von pdb und tox
---------------------------

Um pdb mit tox kombinieren zu können, müssen wir sicherstellen, dass wir
Argumente durch tox an pytest übergeben können. Dies geschieht mit der
``{posargs}``-Funktion von tox, die in :ref:`posargs` beschrieben wurde. Wir
haben diese Funktion bereits in unserer :file:`tox.ini` für Items eingerichtet:

.. code-block:: ini
   :emphasize-lines: 11

   [tox]
   envlist = py38, py39, py310, py311
   isolated_build = True
   skip_missing_interpreters = True

   [testenv]
   deps =
     pytest
     faker
     pytest-cov
   commands = pytest --cov=items --cov-fail-under=99  {posargs}

   [gh-actions]
   python =
     3.8: py38
     3.9: py39
     3.10: py310
     3.11: py311

Wir möchten die Python 3.11-Umgebung ausführen und den Debugger bei einem
fehlgeschlagenen Test starten mit ``tox -e py311 -- --pdb --no-cov``. Das
bringt uns in den pdb, genau an der *Assertion*, die fehlgeschlagen ist.

Nachdem wir den Fehler gefunden und behoben haben, können wir die Tox-Umgebung
mit diesem einen Testfehler erneut ausführen: ``tox -e py311 -- --lf --tb=no
--no-cov``.

Überblick über die gebräuchlichsten pytest-Debugger-Optionen
------------------------------------------------------------

+-------------------------------+-----------------------------------------------+
| Optionen                      | Beschreibung                                  |
+===============================+===============================================+
| Optionen für die Auswahl, welche Tests in welcher Reihenfolge                 |
| ausgeführt werden sollen und wann sie gestoppt werden sollen:                 |
+-------------------------------+-----------------------------------------------+
| :samp:`--lf`,                 | führt den zuerst fehlgeschlagenen Test aus    |
| :samp:`--last-failedlf`       |                                               |
+-------------------------------+-----------------------------------------------+
| :samp:`--ff`,                 | startet mit dem zuerst fehlgeschlagenen Test  |
| :samp:`--failed-first`        | und führt dann alle aus.                      |
+-------------------------------+-----------------------------------------------+
| :samp:`-x`,                   | hält beim ersten Fehler an                    |
| :samp:`--exitfirst`           | und führt dann alle aus.                      |
+-------------------------------+-----------------------------------------------+
| :samp:`-maxfail={NUM}`        | stoppt die Tests nach :samp:`{NUM}` Fehlern.  |
+-------------------------------+-----------------------------------------------+
| :samp:`--nf`,                 | führt zuerst neue Testdateien aus, dann den   |
| :samp:`--new-first`           | Rest sortiert nach Änderungsdatum.            |
+-------------------------------+-----------------------------------------------+
| :samp:`--sw`,                 | führt den letzten fehlgeschlagenen Test aus,  |
| :samp:`--stepwise`            | stoppt dann beim nächsten Fehler und startet  |
|                               | beim nächsten Mal wieder beim letzten         |
|                               | fehlgeschlagenen Test. Ähnlich wie die        |
|                               | Kombination von :samp:`--lf -x`, aber         |
|                               | effizienter.                                  |
+-------------------------------+-----------------------------------------------+
| :samp:`--sw-skip`,            | wie oben, aber ein fehlgeschlagener Test      |
| :samp:`--stepwise-skip`       | wird übersprungen.                            |
+-------------------------------+-----------------------------------------------+
| Optionen zur Kontrolle der pytest-Ausgabe:                                    |
+-------------------------------+-----------------------------------------------+
| :samp:`-v`,                   | verbos, :samp:`-vv` ist noch ausführlicher    |
| :samp:`--verbose:`            |                                               |
+-------------------------------+-----------------------------------------------+
| :samp:`--tb`                  | Traceback-Stil:                               |
|                               | :samp:`[auto|long|short|line|native|no]`      |
|                               |                                               |
|                               | Üblicherweise nutze ich :samp:`--tb=short`    |
|                               | als Standardeinstellung in der                |
|                               | Konfigurationsdatei und die anderen für die   |
|                               | Fehlersuche.                                  |
+-------------------------------+-----------------------------------------------+
| :samp:`-l`,                   | zeigt lokale Variablen neben dem Stacktrace   |
| :samp:`--showlocals`          | an.                                           |
+-------------------------------+-----------------------------------------------+
| Optionen zum Starten eines Kommandozeilen-Debuggers:                          |
+-------------------------------+-----------------------------------------------+
| :samp:`--pdb`                 | startet den Python-Debugger im Fehlerfall.    |
|                               | Sehr nützlich zum Debuggen mit :doc:`../tox`. |
+-------------------------------+-----------------------------------------------+
| :samp:`--trace`               | startet den pdb-Quellcode-Debugger sofort     |
|                               | bei der Ausführung jedes Tests.               |
+-------------------------------+-----------------------------------------------+
| :samp:`--pdbcls`              | verwendet Alternativen zu pdb, :abbr:`z.B.    |
|                               | (zum Beispiel)` den IPython-Debugger mit      |
|                               | ``--pdb-cls =                                 |
|                               | IPython.terminal.debugger:TerminalPdb``       |
+-------------------------------+-----------------------------------------------+
