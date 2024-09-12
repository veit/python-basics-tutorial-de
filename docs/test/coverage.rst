Coverage
========

Wir haben eine erste Liste von Testfällen erstellt. Die Tests im
:file:`tests/api`-Verzeichnis testen die Items über die API. Aber woher wissen
wir, ob diese Tests unseren Code umfassend testen? An dieser Stelle kommt die
Codeabdeckung (engl.: Coverage) ins Spiel.

Tools, die die Codeabdeckung messen, beobachten euren Code, während eine
Testsuite ausgeführt wird, und halten fest, welche Zeilen durchlaufen werden und
welche nicht. Dieses Maß – die :abbr:`sog. (sogenannte)` line coverage – wird
berechnet, indem die Gesamtzahl der ausgeführten Zeilen durch die Gesamtanzahl
der Codezeilen geteilt wird. Code-Coverage-Tools können euch auch sagen, ob alle
Pfade in Control-Statements durchlaufen werden, eine Messung, die als
Branch-Coverage bezeichnet wird.

Die Codeabdeckung kann euch jedoch nicht sagen, ob eure Testsuite gut ist; sie
kann euch nur darüber informieren, wie viel des Anwendungscodes von eurer
Testsuite durchlaufen wird.

`Coverage.py <https://coverage.readthedocs.io/en/latest/>`_ ist das bevorzugte
Python-Tool, das die Codeabdeckung misst. Und `pytest-cov
<https://pytest-cov.readthedocs.io/en/latest/>`_ ist ein beliebtes
:doc:`Pytest-Plugin <pytest/plugins>`, das oft in Verbindung mit Coverage.py
verwendet wird.

Coverage.py mit pytest-cov verwenden
------------------------------------

Sowohl Coverage.py als auch pytest-cov sind Third-Party-Packages, die vor der
Verwendung installiert werden müssen:

Ihr könnt einen Report für die Testabdeckung erstellen mit Coverage.py.

.. tab:: Linux/macOS

   .. code-block:: console

      $ bin/python -m pip install coverage pytest-cov

.. tab:: Windows

   .. code-block:: ps1con

      C:> Scripts\python -m pip install coverage pytest-cov

.. note::
   Wollt ihr die Testabdeckung für Python 2 and Python<3.6 ermitteln, müsst ihr
   Coverage<6.0 verwenden.

Um Tests mit Coverage.py auszuführen, müsst ihr die Option ``--cov`` hinzufügen
und entweder einen Pfad zu dem Code angeben, den ihr messen wollt, oder das
installierte Paket, das ihr testet. In unserem Fall ist das Projekt Items ein
installiertes Paket, so dass wir es mit ``--cov=items`` testen werden.

Auf die normale pytest-Ausgabe folgt der Abdeckungsbericht, wie hier gezeigt:

.. code-block:: pytest

    $ cd /PATH/TO/items
    $ python3 -m venv .
    $ . bin/activate
    $ python -m pip install ".[dev]"
    $ pytest --cov=items
    ============================= test session starts ==============================
    ...
    rootdir: /Users/veit/cusy/prj/items
    configfile: pyproject.toml
    testpaths: tests
    plugins: cov-4.1.0, Faker-19.11.0
    collected 35 items

    tests/api/test_add.py ....                                               [ 11%]
    tests/api/test_config.py .                                               [ 14%]
    tests/api/test_count.py ...                                              [ 22%]
    tests/api/test_delete.py ...                                             [ 31%]
    tests/api/test_finish.py ....                                            [ 42%]
    tests/api/test_list.py .........                                         [ 68%]
    tests/api/test_start.py ....                                             [ 80%]
    tests/api/test_update.py ....                                            [ 91%]
    tests/api/test_version.py .                                              [ 94%]
    tests/cli/test_add.py ..                                                 [100%]

    ---------- coverage: platform darwin, python 3.11.5-final-0 ----------
    Name                    Stmts   Miss  Cover
    -------------------------------------------
    src/items/__init__.py       3      0   100%
    src/items/api.py           70      1    99%
    src/items/cli.py           38      9    76%
    src/items/db.py            23      0   100%
    -------------------------------------------
    TOTAL                     134     10    93%


    ============================== 35 passed in 0.11s ==============================

Die vorherige Ausgabe wurde von den Berichtsfunktionen von coverage erzeugt, obwohl wir coverage nicht direkt aufgerufen haben.
``pytest --cov=items`` wies das ``pytest-cov``-Plugin an

* ``coverage`` mit ``--source`` auf ``items`` zu setzen, während pytest mit den
  Tests ausgeführt wird
* ``coverage report`` auszuführen für den Line-Coverage-Report

Ohne pytest-cov würden die Befehle wie folgt aussehen:

.. code-block:: console

    $ coverage run --source=items -m pytest
    $ coverage report

Die Dateien :file:`__init__.py` und :file:`db.py` haben eine Abdeckung von 100%,
was bedeutet, dass unsere Testsuite auf jede Zeile in diesen Dateien trifft. Das
sagt uns jedoch nicht, dass sie ausreichend getestet ist oder dass die Tests
mögliche Fehler erkennen; aber es sagt uns zumindest, dass jede Zeile während der
Testsuite ausgeführt wurde.

Die Datei :file:`cli.py` hat eine Abdeckung von 76%. Dies mag überraschend hoch
erscheinen, da wir die CLI noch gar nicht getestet haben. Dies hängt jedoch
damit zusammen, dass :file:`cli.py` von :file:`__init__.py` importiert wird, so
dass alle Funktionsdefinitionen ausgeführt werden, aber keiner der
Funktionsinhalte.

Wirklich interessiert uns jedoch die :file:`api.py`-Datei mit 99% Testabdeckung.
Wir können herausfinden, was übersehen wurde, indem wir die Tests erneut ausführen und die Option ``--cov-report=term-missing`` hinzufügen:

.. code-block:: pytest

    pytest --cov=items --cov-report=term-missing
    ============================= test session starts ==============================
    ...
    rootdir: /Users/veit/cusy/prj/items
    configfile: pyproject.toml
    testpaths: tests
    plugins: cov-4.1.0, Faker-19.11.0
    collected 35 items

    tests/api/test_add.py ....                                               [ 11%]
    tests/api/test_config.py .                                               [ 14%]
    tests/api/test_count.py ...                                              [ 22%]
    tests/api/test_delete.py ...                                             [ 31%]
    tests/api/test_finish.py ....                                            [ 42%]
    tests/api/test_list.py .........                                         [ 68%]
    tests/api/test_start.py ....                                             [ 80%]
    tests/api/test_update.py ....                                            [ 91%]
    tests/api/test_version.py .                                              [ 94%]
    tests/cli/test_add.py ..                                                 [100%]

    ---------- coverage: platform darwin, python 3.11.5-final-0 ----------
    Name                    Stmts   Miss  Cover   Missing
    -----------------------------------------------------
    src/items/__init__.py       3      0   100%
    src/items/api.py           68      1    99%   52
    src/items/cli.py           38      9    76%   18-19, 25, 39-43, 51
    src/items/db.py            23      0   100%
    -----------------------------------------------------
    TOTAL                     132     10    92%


    ============================== 35 passed in 0.11s ==============================

Da wir nun die Zeilennummern der nicht getesteten Zeilen haben, können wir die
Dateien in einem Editor öffnen und die fehlenden Zeilen betrachten. Einfacher
ist es jedoch, sich den HTML-Bericht anzusehen.

.. seealso::
   * `pytest-cov’s documentation <https://pytest-cov.readthedocs.io/>`_

HTML-Berichte generieren
~~~~~~~~~~~~~~~~~~~~~~~~

Mit Coverage.py können wir HTML-Berichte erstellen, um die Coverage-Daten
detaillierter betrachten zu können. Der Bericht wird entweder mit der Option
``--cov-report=html`` oder durch die Ausführung von ``coverage html`` nach einem
vorherigen Coverage-Run erstellt:

.. code-block:: console

    $ cd /PATH/TO/items
    $ python3 -m venv .
    $ . bin/activate
    $ python -m pip install ".[dev]"
    $ pytest --cov=items --cov-report=html

Bei beiden Befehlen wird Coverage.py aufgefordert, einen HTML-Bericht im
:file:`htmlcov/`-Verzeichnis zu erstellen. Öffnet :file:`htmlcov/index.html` mit
einem Browser und ihr solltet folgendes sehen:

.. image:: coverage.png
   :alt: Coverage report: 92%

Wenn ihr auf die :file:`src/items/api.py:`-Datei klickt, wird ein Bericht für
diese Datei angezeigt:

.. image:: api.png
   :alt:  Coverage for src/items/api.py: 99%

Der obere Teil des Berichts zeigt den Prozentsatz der abgedeckten Zeilen (99%), die Gesamtzahl der Statements (68) und wie viele Statements ausgeführt (67),
übersehen (1) und ausgeschlossen (0) wurden. Klickt auf
:menuselection:`missing` , um die Zeilen hervorzuheben, die nicht ausgeführt
wurden:

.. image:: missing.png
   :alt: raise MissingSummary

Es sieht so aus, als hätte die Funktion :func:`add_item` eine Exception
``MissingSummary``, die bisher nicht getestet wird.

Code von der Testabdeckung ausschließen
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

In den HTML-Berichten findet ihr eine Spalte mit der Angabe *0 excluded*. Dies
bezieht sich auf eine Funktion von Coverage.py, die es uns ermöglicht, einige
Zeilen von der Prüfung auszuschließen. In Items schließen wir nichts aus. Es ist
jedoch nicht ungewöhnlich, dass einige Codezeilen von der Berechnung der
Testabdeckung ausgeschlossen werden, :abbr:`z.B. (zum Beispiel)` können Module,
die sowohl importiert wie auch direkt ausgeführt werden sollen, einen Block
enthalten, der so oder so ähnlich aussieht:

.. code-block:: python

    if __name__ == "__main__":
        main()

Dieser Befehl weist Python an, :func:`main` auszuführen, wenn wir das Modul
direkt aufrufen mit ``python my_module.py``, aber den Code nicht auszuführen,
wenn das Modul importiert wird. Diese Arten von Code-Blöcken werden häufig mit
einer einfachen Pragma-Anweisung vom Testen ausgeschlossen:

.. code-block:: python

    if __name__ == "__main__":  # pragma: no cover
        main()

Damit wird Coverage.py angewiesen, entweder eine einzelne Zeile oder einen
Code-Block auszuschließen. Wenn, wie in diesem Fall, das Pragma in der
if-Anweisung steht,  müsst ihr es nicht in beide Codezeilen einfügen.

Alternativ kann dies auch für alle Vorkommen konfiguriert werden:

.. tab:: :file:`.coveragerc`

   .. code-block:: ini

      [run]
      branch = True

      [report]
      ; Regexes for lines to exclude from consideration
      exclude_also =

          ; Don’t complain if tests don’t hit defensive assertion code:
          raise AssertionError
          raise NotImplementedError

          ; Don't complain if non-runnable code isn’t run:
          if __name__ == .__main__.:

      ignore_errors = True

      [html]
      directory = coverage_html_report

.. tab:: :file:`pyproject.toml`

   .. code-block:: toml

      [tool.coverage.run]
      branch = true

      [tool.coverage.report]
      # Regexes for lines to exclude from consideration
      exclude_also = [
          # Don’t complain if tests don’t hit defensive assertion code:
          "raise AssertionError",
          "raise NotImplementedError",

          # Don’t complain if non-runnable code isn’t run:
          "if __name__ == .__main__.:",
          ]

      ignore_errors = true

      [tool.coverage.html]
      directory = "coverage_html_report"

.. tab:: :file:`setup.cfg`, :file:`tox.ini`

   .. code-block:: ini

      [coverage:run]
      branch = True

      [coverage:report]
      ; Regexes for lines to exclude from consideration
      exclude_also =

          ; Don’t complain if tests don’t hit defensive assertion code:
          raise AssertionError
          raise NotImplementedError

          ; Don’t complain if non-runnable code isn’t run:
          if __name__ == .__main__.:

      ignore_errors = True

      [coverage:html]
      directory = coverage_html_report

.. seealso::
   `Configuration reference
   <https://coverage.readthedocs.io/en/latest/config.html>`_

Erweiterungen
-------------

In `Coverage.py plugins
<https://gist.github.com/nedbat/2e9dbf7f33b1e0e857368af5c5d06202>`_ findet ihr
auch eine Reihe von Erweiterungen für Coverage.

.. _coverage-github-actions:

Testabdeckung aller Tests mit GitHub-Actions
--------------------------------------------

Nachdem ihr die Testabdeckung überprüft habt, könnt ihr die Dateien als
GitHub-Action :abbr:`z.B. (zum Beispiel)` in einer :download:`ci.yaml` als
Artefakte hochladen um sie später in weiteren Jobs wiederverwenden zu können:

.. literalinclude:: ci.yaml
   :language: yaml
   :lines: 45-51
   :lineno-start: 45

``include-hidden-files``
    ist mit `actions/upload-artifact v4.4.0
    <https://github.com/actions/upload-artifact/releases/tag/v4.4.0>`_ notwendig
    geworden.
``if-no-files-found: ignore``
    ist sinnvoll, wenn nicht für alle Python-Versionen die Testabdeckung
    gemessen werden soll um schneller zum Ergebnis zu kommen. Daher solltet ihr
    nur für diejenigen Elemente eurer Matrix, die ihr berücksichtigen wollt, die
    Daten hochladen.

Nachdem alle Tests durchlaufen wurden, könnt ihr einen weiteren Job definieren,
der die Ergebnisse zusammenführt:

.. literalinclude:: ci.yaml
   :language: yaml
   :lines: 53-91
   :lineno-start: 53

``needs: tests``
    stellt sicher, dass alle Tests durchgeführt werden. Wenn euer Job, der die
    Tests ausführt, einen anderen Namen hat, müsst ihr ihn hier anpassen.

``name: "Download coverage data"``
    lädt die Daten der Testabdeckung herunter, die zuvor mit ``name: "Upload
    coverage data"`` hochgeladen wurden.
``name: "Combine coverage and fail it it’s under 100 %"``
    kombiniert die Testabdeckung und erstellt einen HTML-Bericht, wenn die
    Bedingung ``--fail-under=100`` erfüllt ist.

Sobald der Workflow abgeschlossen ist, könnt ihr den HTML-Bericht herunterladen
unter :menuselection:`YOUR_REPO --> Actions --> tests --> Combine and check
coverage`.

.. seealso::
   * `How to Ditch Codecov for Python Projects
     <https://hynek.me/articles/ditch-codecov-python/>`_
   * `structlog main.yml
     <https://github.com/hynek/structlog/blob/main/.github/workflows/ci.yml>`_

.. _coverage-badge:

Badge
-----

Ihr könnt GitHub Actions verwenden, um ein Badge mit eurer Code-Coverage zu
erstellen. Dabei wird zusätzlich ein GitHub Gist benötigt um die Parameter für
das Badge, das von `shields.io <https://shields.io>`_ gerendert wird, zu
speichern. Hierfür erweitern wir unsere :download:`ci.yaml` folgendermaßen:

.. literalinclude:: ci.yaml
   :language: yaml
   :lines: 93-
   :lineno-start: 93

Zeile 97
    ``GIST_TOKEN`` ist ein persönliches GitHub-Zugangs-Token.
Zeile 98
    ``YOUR_GIST_ID`` solltet ihr durch eure eigene Gist-ID ersetzen. Falls ihr
    noch keine Gist-ID habt, könnt ihr diese erstellen mit:

    #. Ruft https://gist.github.com auf und erstellt einen neuen Gist, den ihr
       :abbr:`z.B. (zum Beispiel)` :file:`test.json` nennen könnt. Die ID des
       Gist ist der lange alphanumerische Teil der URL, den ihr hier benötigt.
    #. Anschließend geht ihr zu https://github.com/settings/tokens und erstellt
       ein neues Token mit dem Gist-Bereich.
    #. Geht schließlich zu :menuselection:`YOUR_REPO --> Settings --> Secrets
       --> Actions` und fügt dieses Token hinzu. Ihr könnt ihm einen beliebigen
       Namen geben, :abbr:`z.B. (zum Beispiel)` :samp:`{GIST_SECRET}`.

       Wenn ihr `Dependabot <https://github.com/dependabot>`_ verwendet, um die
       Abhängigkeiten eures Repository automatisch zu aktualisieren, müsst ihr
       das  :samp:`{GIST_SECRET}` auch in :menuselection:`YOUR_REPO --> Settings
       --> Secrets --> Dependabot` hinzufügen.

Zeilen 102-104
    Das Badge wird automatisch eingefärbt:

    * ≤ 50 % in rot
    * ≥ 90 % in grün
    * mit einem Farbverlauf zwischen den beiden


Jetzt kann das Badge mit einer URL wie dieser angezeigt werden:
:samp:`https://img.shields.io/endpoint?url=https://gist.githubusercontent.com/{YOUR_GITHUB_NAME}/{GIST_SECRET}/raw/covbadge.json`.

.. image:: https://img.shields.io/endpoint?url=https://gist.githubusercontent.com/nedbat/a27aaed4944c1f760a969a543fb52767/raw/covbadge2_40.json
.. image:: https://img.shields.io/endpoint?url=https://gist.githubusercontent.com/nedbat/a27aaed4944c1f760a969a543fb52767/raw/covbadge2_45.json
.. image:: https://img.shields.io/endpoint?url=https://gist.githubusercontent.com/nedbat/a27aaed4944c1f760a969a543fb52767/raw/covbadge2_50.json
.. image:: https://img.shields.io/endpoint?url=https://gist.githubusercontent.com/nedbat/a27aaed4944c1f760a969a543fb52767/raw/covbadge2_55.json
.. image:: https://img.shields.io/endpoint?url=https://gist.githubusercontent.com/nedbat/a27aaed4944c1f760a969a543fb52767/raw/covbadge2_60.json
.. image:: https://img.shields.io/endpoint?url=https://gist.githubusercontent.com/nedbat/a27aaed4944c1f760a969a543fb52767/raw/covbadge2_65.json
.. image:: https://img.shields.io/endpoint?url=https://gist.githubusercontent.com/nedbat/a27aaed4944c1f760a969a543fb52767/raw/covbadge2_70.json
.. image:: https://img.shields.io/endpoint?url=https://gist.githubusercontent.com/nedbat/a27aaed4944c1f760a969a543fb52767/raw/covbadge2_75.json
.. image:: https://img.shields.io/endpoint?url=https://gist.githubusercontent.com/nedbat/a27aaed4944c1f760a969a543fb52767/raw/covbadge2_80.json
.. image:: https://img.shields.io/endpoint?url=https://gist.githubusercontent.com/nedbat/a27aaed4944c1f760a969a543fb52767/raw/covbadge2_85.json
.. image:: https://img.shields.io/endpoint?url=https://gist.githubusercontent.com/nedbat/a27aaed4944c1f760a969a543fb52767/raw/covbadge2_90.json
.. image:: https://img.shields.io/endpoint?url=https://gist.githubusercontent.com/nedbat/a27aaed4944c1f760a969a543fb52767/raw/covbadge2_95.json
.. image:: https://img.shields.io/endpoint?url=https://gist.githubusercontent.com/nedbat/a27aaed4944c1f760a969a543fb52767/raw/covbadge2_100.json
