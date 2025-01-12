Beispiele
=========

Ihr könnt einfach eine Datei :file:`test_one.py` anlegen mit folgendem Inhalt:

.. literalinclude:: test_one.py
   :language: python
   :lineno-start: 1

Die Funktion ``test_sorted()`` wird von pytest als Testfunktion erkannt, weil
sie mit :samp:`test_` beginnt und in einer Datei steht, die mit :samp:`test_`
beginnt. Wenn der Test ausgeführt wird, bestimmt die ``assert``-Anweisung, ob
der Test erfolgreich war oder nicht. ``assert`` ist ein in Python eingebautes
Schlüsselwort und löst eine ``AssertionError``-Ausnahme aus, wenn der Ausdruck
nach ``assert`` falsch ist. Jede nicht abgefangene Ausnahme, die innerhalb eines
Tests ausgelöst wird, führt dazu, dass der Test fehlschlägt.

pytest ausführen
----------------

.. code-block:: pytest

   $ cd docs/test/pytest
   $ pytest test_one.py
   ============================= test session starts ==============================
   …
   collected 1 item

   test_one.py .                                                            [100%]

   ============================== 1 passed in 0.00s ===============================

Der Punkt hinter :file:`test_one.py` bedeutet, dass ein Test durchgeführt und
bestanden wurde. ``[100%]`` ist eine Prozentanzeige, die angibt, wie viele Tests
der Testsitzung bisher durchgeführt wurden. Da es nur einen Test gibt,
entspricht ein Test 100% der Tests. Wenn ihr mehr Informationen benötigt, könnt
ihr ``-v`` oder ``--verbose`` verwenden:

.. code-block:: pytest

   $ pytest -v test_one.py
   ============================= test session starts ==============================
   …
   collected 1 item

   test_one.py::test_sorted PASSED                                          [100%]

   ============================== 1 passed in 0.00s ===============================

:file:`test_two.py` schlägt hingegen fehl:

.. code-block:: pytest

   $ pytest test_two.py
   collected 1 item

   test_two.py F                                                            [100%]

   =================================== FAILURES ===================================
   _________________________________ test_failing _________________________________

       def test_failing():
   >       assert sorted([4, 2, 1, 3]) == [0, 1, 2, 3]
   E       assert [1, 2, 3, 4] == [0, 1, 2, 3]
   E         At index 0 diff: 1 != 0
   E         Use -v to get more diff

   test_two.py:2: AssertionError
   =========================== short test summary info ============================
   FAILED test_two.py::test_failing - assert [1, 2, 3, 4] == [0, 1, 2, 3]
   ============================== 1 failed in 0.03s ===============================

Der fehlgeschlagene Test, ``test_in``, erhält einen eigenen Abschnitt, um uns zu
zeigen, warum er fehlgeschlagen ist. Und ``pytest`` sagt uns genau, was der
erste Fehler ist. Dieser zusätzliche Abschnitt wird Traceback genannt. Das sind
schon eine Menge Informationen, aber es gibt eine Zeile, die besagt, dass wir
mit ``-v`` den kompletten Diff erhalten. Lasst uns das tun:

.. code-block:: pytest

   $ pytest -v test_two.py
   ============================= test session starts ==============================
   …
   collected 1 item

   test_two.py::test_failing FAILED                                         [100%]

   =================================== FAILURES ===================================
   _________________________________ test_failing _________________________________

       def test_failing():
   >       assert sorted([4, 2, 1, 3]) == [0, 1, 2, 3]
   E       assert [1, 2, 3, 4] == [0, 1, 2, 3]
   E         At index 0 diff: 1 != 0
   E         Full diff:
   E         - [0, 1, 2, 3]
   E         ?  ---
   E         + [1, 2, 3, 4]
   E         ?         +++

   test_two.py:2: AssertionError
   =========================== short test summary info ============================
   FAILED test_two.py::test_failing - assert [1, 2, 3, 4] == [0, 1, 2, 3]
   ============================== 1 failed in 0.03s ===============================

``pytest`` fügt ``+``- und ``-``-Zeichen hinzu, um uns genau die Unterschiede zu
zeigen.

Bisher haben wir ``pytest`` mit dem Befehl :samp:`pytest {FILE}.py` ausgeführt.
Lasst uns ``pytest`` nun auf ein paar weitere Arten laufen. Wenn ihr keine
Dateien oder Verzeichnisse angebt, sucht ``pytest`` nach Tests im aktuellen
Arbeitsverzeichnis und in Unterverzeichnissen; genauer wird nach ``.py`` Dateien
gesucht, die mit :file:`test_` beginnen oder mit :file:`_test` enden. Wenn ihr
pytest im Verzeichnis :file:`docs/test/pytest` ohne Optionen startet, werden
zwei Dateien mit Tests ausgeführt:

.. code-block:: pytest

   $ pytest --tb=no
   ============================= test session starts ==============================
   …

   test_one.py .                                                            [ 50%]
   test_two.py F                                                            [100%]

   =========================== short test summary info ============================
   FAILED test_two.py::test_failing - assert [1, 2, 3, 4] == [0, 1, 2, 3]
   ========================= 1 failed, 1 passed in 0.00s ==========================

Ich habe auch die Option ``--tb=no`` verwendet, um die Rückverfolgung (engl.:
Traceback) abzuschalten, da wir die vollständige Ausgabe im Moment nicht
wirklich brauchen.

Wir können auch eine Testfunktion innerhalb einer Testdatei angeben, die
ausgeführt werden soll, indem wir :samp:`::test_{name}` zum Dateinamen
hinzufügen:

.. code-block:: pytest

   $ pytest -v test_one.py::test_sorted
   ============================= test session starts ==============================
   …
   collected 1 item

   test_one.py::test_sorted PASSED                                          [100%]

   ============================== 1 passed in 0.00s ===============================

Testergebnisse
--------------

Zu den möglichen Ergebnissen einer Testfunktion gehören:

``PASSED (.)``
    Der Test wurde erfolgreich durchgeführt.
``FAILED (F)``
    Der Test wurde nicht erfolgreich durchgeführt.
``SKIPPED (s)``
    Der Test wurde übersprungen.
``XFAIL (x)``
    Der Test sollte nicht bestehen, wurde aber durchgeführt und ist
    fehlgeschlagen.
``XPASS (X)``
    Der Test wurde mit ``xfail`` markiert, aber er lief und bestand.
``ERROR (E)``
    Eine Ausnahme ist während der Ausführung einer :doc:`fixtures` aufgetreten,
    nicht aber während der Ausführung einer Testfunktion.
