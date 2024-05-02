tox
===

`tox <https://tox.readthedocs.io/>`_ ist ein Automatisierungstool, das ähnlich
wie ein :term:`CI`-Tool funktioniert, aber sowohl lokal als auch in Verbindung
mit anderen CI-Tools auf einem Server ausgeführt werden kann.

Im Folgenden richten wir uns tox für unsere Items-Anwendung so ein, dass es uns
beim lokalen Testen hilft. Anschließend werden wir das Testen mithilfe von
GitHub Actions einrichten.

Einführung in tox
-----------------

tox ist ein Kommandozeilen-Tool, mit dem ihr eure komplette Testsuite in
verschiedenen Umgebungen ausführen könnt. Wir werden tox verwenden, um das
Items-Projekt in mehreren Python-Versionen zu testen. tox ist jedoch nicht nur
auf Python-Versionen beschränkt. Ihr könnt es zum Testen mit verschiedenen
Abhängigkeits-Konfigurationen und verschiedenen Konfigurationen für verschiedene
Betriebssysteme verwenden. tox verwendet dabei Projektinformationen aus der
:file:`setup.py`- oder :file:`pyproject.toml`-Datei für das zu testende Paket,
um eine installierbare :doc:`Distribution eures Pakets <../libs/distribution>`
zu erstellen. Es sucht in der :file:`tox.ini`-Datei nach einer Liste von
Umgebungen, und führt dann jeweils folgende Schritte aus:

#. erstellt eine :term:`virtuelle Umgebung <Virtuelle Umgebung>`,
#. installiert einige Abhängigkeiten mit :term:`pip`,
#. baut euer Paket,
#. installiert euer Paket mit pip,
#. führt weitere Tests aus.

Nachdem alle Umgebungen getestet wurden, gibt tox eine Zusammenfassung der
Ergebnisse aus.

.. note::
   Obwohl tox von vielen Projekten verwendet wird, gibt es Alternativen, die
   ähnliche Funktionen erfüllen. Zwei Alternativen zu tox sind `nox
   <https://nox.thea.codes/en/stable/>`_ und `invoke
   <https://www.pyinvoke.org>`_.

tox einrichten
--------------

Bis jetzt hatten wir den items-Code in einem :file:`src/`-Verzeichnis und die
Tests in :file:`tests/api/` und :file:`tests/cli/`. Jetzt werden wir eine
:file:`tox.ini`-Datei hinzufügen, sodass die Struktur so aussieht:

.. code-block:: console
   :emphasize-lines: 16

    items
     ├── …
     ├── pyproject.toml
     ├── src
     │   └── items
     │       └── ...
     ├── tests
     │   ├── api
     │   │   ├── __init__.py
     │   │   ├── conftest.py
     │   │   └── test_….py
     │   └── cli
     │       ├── __init__.py
     │       ├── conftest.py
     │       └── test_….py
     └── tox.ini

Dies ist ein typisches Layout für viele Projekte. Werfen wir einen Blick auf
eine einfache :file:`tox.ini`-Datei im Items-Projekt:

.. code-block:: ini

   [tox]
   envlist = py312

   [testenv]
   deps =
     pytest>=6.0
     faker
   commands = pytest

Im ``[tox]``-Abschnitt haben wir ``envlist = py312`` definiert. Dies ist eine
Abkürzung, die tox anweist, unsere Tests mit Python Version 3.12 durchzuführen.
Wir werden in Kürze weitere Python-Versionen hinzufügen, aber die Verwendung
einer Version hilft, den Ablauf von tox zu verstehen.

Beachtet auch die Zeile ``isolated_build = True``: Dies ist für alle mit
:file:`pyproject.toml` konfigurierten Pakete erforderlich. Für alle mit
:file:`setup.py`-konfigurierten Projekte, die die :term:`setuptools`-Bibliothek
verwenden, kann diese Zeile jedoch weggelassen werden.

Im ``[testenv]``-Abschnitt werden unter ``deps`` ``pytest`` und ``faker`` als
Abhängigkeiten aufgeführt. Somit weiß tox, dass wir diese beiden Werkzeuge zum
Testen benötigen. Wenn ihr möchtet, könnt ihr auch angeben, welche Version
verwendet werden soll, :abbr:`z.B. (zum Beispiel)` ``pytest>=6.0``.
Mit ``commands`` wird schließlich tox angewiesen, ``pytest`` in jeder Umgebung
auszuführen.

tox ausführen
-------------

Bevor ihr tox ausführen könnt, müsst ihr sicherstellen, dass ihr es installiert:

.. tab:: Linux/macOS

   .. code-block:: console

      $ python3 -m venv .
      $ . bin/acitvate
      $ python -m pip install tox

.. tab:: Windows

   .. code-block:: ps1con

      C:> python -m venv .
      C:> Scripts\activate
      C:> python -m pip install tox

Um tox auszuführen, startet einfach tox:

.. code-block:: pytest

    $ python -m tox
    .pkg: _optional_hooks> python /Users/veit/cusy/prj/items_env/lib/python3.12/site-packages/pyproject_api/_backend.py True hatchling.build
    .pkg: get_requires_for_build_sdist> python /Users/veit/cusy/prj/items_env/lib/python3.12/site-packages/pyproject_api/_backend.py True hatchling.build
    .pkg: build_sdist> python /Users/veit/cusy/prj/items_env/lib/python3.12/site-packages/pyproject_api/_backend.py True hatchling.build
    py312: install_package> python -I -m pip install --force-reinstall --no-deps /Users/veit/cusy/prj/items/.tox/.tmp/package/37/items-0.1.0.tar.gz
    py312: commands[0]> pytest
    ============================= test session starts ==============================
    ...
    configfile: pyproject.toml
    testpaths: tests
    plugins: Faker-25.0.0
    collected 49 items

    tests/api/test_add.py ....                                               [  8%]
    tests/api/test_config.py .                                               [ 10%]
    tests/api/test_count.py ...                                              [ 16%]
    tests/api/test_delete.py ...                                             [ 22%]
    tests/api/test_finish.py ....                                            [ 30%]
    tests/api/test_list.py .........                                         [ 48%]
    tests/api/test_start.py ....                                             [ 57%]
    tests/api/test_update.py ....                                            [ 65%]
    tests/api/test_version.py .                                              [ 67%]
    tests/cli/test_add.py ..                                                 [ 71%]
    tests/cli/test_config.py ..                                              [ 75%]
    tests/cli/test_count.py .                                                [ 77%]
    tests/cli/test_delete.py .                                               [ 79%]
    tests/cli/test_errors.py ....                                            [ 87%]
    tests/cli/test_finish.py .                                               [ 89%]
    tests/cli/test_list.py ..                                                [ 93%]
    tests/cli/test_start.py .                                                [ 95%]
    tests/cli/test_update.py .                                               [ 97%]
    tests/cli/test_version.py .                                              [100%]

    ============================== 49 passed in 0.09s ==============================
    .pkg: _exit> python /Users/veit/cusy/prj/items_env/lib/python3.12/site-packages/pyproject_api/_backend.py True hatchling.build
      py312: OK (1.40=setup[1.07]+cmd[0.33] seconds)
      congratulations :) (1.44 seconds)

Mehrere Python-Versionen testen
-------------------------------

Hierfür erweitern wir ``envlist`` in der :file:`tox.ini`-Datei um weitere
Python-Versionen hinzuzufügen:

.. code-block:: ini
   :emphasize-lines: 2, 4

   [tox]
   envlist = py3{8,9,10,11,12}
   isolated_build = True
   skip_missing_interpreters = True

Damit werden wir jetzt Python-Versionen von 3.8 bis 3.12 testen. Zusätzlich
haben wir auch die Einstellung ``skip_missing_interpreters = True`` hinzugefügt,
damit tox nicht fehlschlägt, wenn auf eurem System eine der aufgeführten
Python-Versionen fehlt. Ist der Wert auf ``True`` gesetzt, führt tox die Tests
mit jeder verfügbaren Python-Version durch, überspringt aber Versionen, die es
nicht findet, ohne fehlzuschlagen. Die Ausgabe ist sehr ähnlich, wobei ich in
der folgenden Darstellung lediglich die Unterschiede hervorhebe:

.. code-block:: pytest
   :emphasize-lines: 2, 4, 13, 14, 16, 22, 23, 30-

    $ python -m tox
    py38: skipped because could not find python interpreter with spec(s): py38
    py38: SKIP ⚠ in 0.01 seconds
    py39: install_deps> python -I -m pip install faker 'pytest>=6.0'
    .pkg: _optional_hooks> python /Users/veit/cusy/prj/items_env/lib/python3.12/site-packages/pyproject_api/_backend.py True hatchling.build
    .pkg: get_requires_for_build_sdist> python /Users/veit/cusy/prj/items_env/lib/python3.12/site-packages/pyproject_api/_backend.py True hatchling.build
    .pkg: build_sdist> python /Users/veit/cusy/prj/items_env/lib/python3.12/site-packages/pyproject_api/_backend.py True hatchling.build
    py39: install_package> python -I -m pip install --force-reinstall --no-deps /Users/veit/cusy/prj/items/.tox/.tmp/package/34/items-0.1.0.tar.gz
    py39: commands[0]> pytest
    ============================= test session starts ==============================
    ...
    ============================== 49 passed in 0.15s ==============================
    py39: OK ✔ in 5.78 seconds
    py310: skipped because could not find python interpreter with spec(s): py310
    py310: SKIP ⚠ in 0.01 seconds
    py311: install_deps> python -I -m pip install faker 'pytest>=6.0'
    py311: install_package> python -I -m pip install --force-reinstall --no-deps /Users/veit/cusy/prj/items/.tox/.tmp/package/35/items-0.1.0.tar.gz
    py311: commands[0]> pytest
    ============================= test session starts ==============================
    ...
    ============================== 49 passed in 0.11s ==============================
    py311: OK ✔ in 3.53 seconds
    py312: install_deps> python -I -m pip install faker 'pytest>=6.0'
    py312: install_package> python -I -m pip install --force-reinstall --no-deps /Users/veit/cusy/prj/items/.tox/.tmp/package/36/items-0.1.0.tar.gz
    py312: commands[0]> pytest
    ============================= test session starts ==============================
    ...
    ============================== 49 passed in 0.11s ==============================
    .pkg: _exit> python /Users/veit/cusy/prj/items_env/lib/python3.12/site-packages/pyproject_api/_backend.py True hatchling.build
      py38: SKIP (0.01 seconds)
      py39: OK (5.78=setup[4.60]+cmd[1.18] seconds)
      py310: SKIP (0.01 seconds)
      py311: OK (3.53=setup[2.82]+cmd[0.71] seconds)
      py312: OK (3.23=setup[2.57]+cmd[0.67] seconds)
      congratulations :) (12.60 seconds)

Tox-Umgebungen parallel ausführen
---------------------------------

Im vorherigen Beispiel wurden die verschiedenen Umgebungen nacheinander
ausgeführt. Es ist auch möglich, sie mit der Option ``-p`` parallel laufen zu
lassen:

.. code-block:: pytest

    $ python -m tox -p
    py38: SKIP ⚠ in 0.07 seconds
    py310: SKIP ⚠ in 0.07 seconds
    py312: OK ✔ in 1.7 seconds
    py311: OK ✔ in 1.75 seconds
      py38: SKIP (0.07 seconds)
      py39: OK (2.42=setup[2.01]+cmd[0.41] seconds)
      py310: SKIP (0.07 seconds)
      py311: OK (1.75=setup[1.35]+cmd[0.40] seconds)
      py312: OK (1.70=setup[1.30]+cmd[0.40] seconds)
      congratulations :) (2.47 seconds)

.. note::
   Die Ausgabe ist nicht abgekürzt; dies ist die gesamte Ausgabe, die ihr seht,
   wenn alles funktioniert.

Coverage-Report in tox hinzufügen
---------------------------------

Der :file:`tox.ini`-Datei kann einfach die Konfiguration von Coverage Reports
hinzugefügt werden. Dazu müssen wir ``pytest-cov`` zu den ``deps``-Einstellungen
hinzufügen, damit das ``pytest-cov``-Plugin in den tox-Testumgebungen
installiert wird. Das Einbinden von ``pytest-cov`` schließt auch alle seine
Abhängigkeiten ein, wie :abbr:`z.B. (zum Beispiel)` Coverage. Wir erweitern dann
``commands`` zu ``pytest --cov=items``:

.. code-block::
   :emphasize-lines: 12-

    [tox]
    envlist = py3{8,9,10,11,12}
    isolated_build = True
    skip_missing_interpreters = True

    [testenv]
    deps =
     pytest>=6.0
     faker
    commands = pytest

    [testenv:coverage-report]
    description = Report coverage over all test runs.
    deps = coverage[toml]
    skip_install = true
    allowlist_externals = coverage
    commands =
      coverage combine
      coverage report

Bei der Verwendung von Coverage mit ``tox`` kann es manchmal sinnvoll sein, in
der  :file:`pyproject.toml`-Datei einen Abschnitt einzurichten, der Coverage
mitteilt, welche Quelltextpfade als identisch betrachtet werden sollen:

.. code-block:: ini

    [tool.coverage.paths]
    source = ["src", ".tox/py*/**/site-packages"]

Der Items-Quellcode befindet sich zunächst in :file:`src/items/`, bevor von tox
die virtuellen Umgebungen erstellt und Items in der Umgebung installiert wird.
Dann befindet es sich :abbr:`z.B. (zum Beispiel)` in
:file:`.tox/py312/lib/python3.12/site-packages/items`.

.. code-block:: pytest

    $ python -m tox
    ...
    coverage-report: install_deps> python -I -m pip install 'coverage[toml]'
    coverage-report: commands[0]> coverage combine
    Combined data file .coverage.fay.local.74688.XgGaASxx
    Skipping duplicate data .coverage.fay.local.74695.XmMjaOox
    Skipping duplicate data .coverage.fay.local.74702.XUQUSgdx
    coverage-report: commands[1]> coverage report
    Name               Stmts   Miss Branch BrPart  Cover   Missing
    --------------------------------------------------------------
    src/items/api.py      68      1     16      1    98%   52
    --------------------------------------------------------------
    TOTAL                428      1    118      1    99%

    27 files skipped due to complete coverage.
      py38: SKIP (0.02 seconds)
      py39: OK (6.24=setup[5.06]+cmd[1.18] seconds)
      py310: SKIP (0.01 seconds)
      py311: OK (3.97=setup[2.77]+cmd[1.20] seconds)
      py312: OK (3.80=setup[2.67]+cmd[1.13] seconds)
      coverage-report: OK (1.53=setup[0.90]+cmd[0.54,0.09] seconds)
      congratulations :) (15.59 seconds)

Mindestabdeckungsgrad festlegen
-------------------------------

Bei der Ausführung der Coverage durch tox ist auch sinnvoll, einen
Mindestabdeckungsgrad festzulegen, um eventuelle Ausrutscher bei der Coverage zu
erkennen. Dies wird mit der Option ``--cov-fail-under`` erreicht:

.. code-block:: console
   :emphasize-lines: 9

    $ python -m coverage report --fail-under=100
    Name               Stmts   Miss Branch BrPart  Cover   Missing
    --------------------------------------------------------------
    src/items/api.py      68      1     16      1    98%   52
    --------------------------------------------------------------
    TOTAL                428      1    118      1    99%

    27 files skipped due to complete coverage.
    Coverage failure: total of 99 is less than fail-under=100

Dadurch wird der Ausgabe die hervorgehobene Zeile hinzugefügt.

.. _posargs:

pytest-Parameter an tox übergeben
---------------------------------

Wir können auch einzelne Tests mit tox aufrufen, indem wir eine weitere Änderung
vornehmen, damit Parameter an pytest übergeben werden können:

.. code-block:: ini
   :emphasize-lines: 17

    [tox]
    envlist =
        pre-commit
        docs
        py3{8,9,10,11,12}
        coverage-report
    isolated_build = True
    skip_missing_interpreters = True

    [testenv]
    extras =
      tests: tests
    deps =
      tests: coverage[toml]
    allowlist_externals = coverage
    commands =
      coverage run -m pytest {posargs}

Um Argumente an pytest zu übergeben, fügt sie zwischen den tox-Argumenten und
den pytest-Argumenten ein. In diesem Fall wählen wir ``test_version``-Tests mit
der Schlüsselwort-Option ``-k`` aus. Wir verwenden auch ``--no-cov``, um die
Abdeckung zu deaktivieren:

.. code-block:: pytest
   :emphasize-lines: 1, 3

    $ tox -e py312 -- -k test_version --no-cov
    ...
    py312: commands[0]> coverage run -m pytest -k test_version --no-cov
    ============================= test session starts ==============================
    ...
    configfile: pyproject.toml
    testpaths: tests
    plugins: cov-5.0.0, Faker-25.0.0
    collected 49 items / 47 deselected / 2 selected

    tests/api/test_version.py .                                              [ 50%]
    tests/cli/test_version.py .                                              [100%]

    ======================= 2 passed, 47 deselected in 0.09s =======================
    .pkg: _exit> python /Users/veit/cusy/prj/items_env/lib/python3.12/site-packages/pyproject_api/_backend.py True hatchling.build
      py312: OK (2.22=setup[1.12]+cmd[1.10] seconds)
      congratulations :) (2.25 seconds)

``tox`` eignet sich nicht nur hervorragend für die lokale Automatisierung von
Testprozessen, sondern hilft auch bei Server-basierter :term:`CI`. Fahren wir
fort mit der Ausführung von ``pytest`` und ``tox`` mithilfe von GitHub-Aktionen.

``tox`` mit GitHub-Aktionen ausführen
-------------------------------------

Wenn euer Projekt auf `GitHub <https://github.com/>`_ gehostet ist, könnt ihr
GitHub-Actions verwenden um automatisiert eure Tests in verschiedenen Umgebungen
ausführen zu können. Dabei sind eine ganze Reihe von Umgebungen für die
GitHub-Actions verfügbar: `github.com/actions/runner-images
<https://github.com/actions/runner-images>`_.

#. Um eine GitHub-Action in eurem Projekt zu erstellen, klickt auf
   :menuselection:`Actions --> set up a workflow yourself`. Dies erstellt
   üblicherweise eine Datei :file:`.github/workflows/main.yml`.
#. Gebt dieser Datei einen aussagekräftigeren Namen. Wir verwenden hierfür
   üblicherweise :file:`ci.yml`.
#. Die vorausgefüllte YAML-Datei ist für unsere Zwecke wenig hilfreich. Ihr
   könnt hier einen ``coverage``-Abschnitt einfügen, :abbr:`z.B. (zum Beispiel)`
   mit:

   .. code-block:: yaml

      jobs:
        coverage:
          name: Ensure 99% test coverage
          runs-on: ubuntu-latest
          needs: tests
          if: always()

          steps:
            - uses: actions/checkout@v4
            - uses: actions/setup-python@v5
              with:
                cache: pip
                python-version: 3.12

            - name: Download coverage data
              uses: actions/download-artifact@v4
              with:
                pattern: coverage-data-*
                merge-multiple: true

            - name: Combine coverage and fail if it’s <99%.
              run: |
                python -m pip install --upgrade coverage[toml]
                python -m coverage combine
                python -m coverage html --skip-covered --skip-empty
                # Report and write to summary.
                python -m coverage report --format=markdown >> $GITHUB_STEP_SUMMARY
                # Report again and fail if under 99%.
                python -m coverage report --fail-under=99

   ``name``
       kann ein beliebiger Name sein. Er wird in der Benutzeroberfläche von
       GitHub Actions angezeigt.
   ``steps``
       ist eine Liste von Schritten. Der Name eines jeden Schrittes kann
       beliebig sein und ist optional.
   ``uses: actions/checkout@v4``
       ist ein GitHub-Actions-Tool, das unser Repository auscheckt, damit der
       Rest des Workflows darauf zugreifen kann.
   ``uses: actions/setup-python@v5``
       ist ein GitHub-Actions-Tool, das Python konfiguriert und in einer
       Build-Umgebung installiert.
   ``with: python-version: ${{ matrix.python }}``
       sagt, dass eine Umgebung für jede der in ``matrix.python`` aufgeführten
       Python-Versionen erstellt werden soll.

#. Anschließend könnt ihr auf :guilabel:`Start commit` klicken. Da wir noch
   weitere Änderungen vornehmen wollen bevor die Tests automatisiert ausgeführt
   werden sollen, wählen wir :guilabel:`Create a new branch for this commit and
   start a pull request` und als Name für den neuen :term:`Branch <branch>`
   ``github-actions``. Schließlich könnt ihr auf :guilabel:`Create pull request`
   klicken.
#. Um nun in den neuen Branch zu wechseln, gehen wir zu :menuselection:`Code -->
   main --> github-actions`.

Die Actions-Syntax ist gut dokumentiert. Ein guter Startpunkt in der
GitHub-Actions-Dokumentation ist die Seite `Building and Testing Python
<https://docs.github.com/en/actions/automating-builds-and-tests/building-and-testing-python>`_.
Die Dokumentation zeigt euch auch, wie ihr pytest direkt ohne tox ausführen
könnt und wie ihr die Matrix auf mehrere Betriebssysteme erweitern könnt. Sobald
ihr eure :file:`*.yml`-Datei eingerichtet und in euer GitHub-Repository
hochgeladen habt, wird sie automatisch ausgeführt. Im Reiter
:menuselection:`Actions` könnt ihr anschließend die Durchläufe sehen:

.. figure:: github-actions.png
   :alt: Screenshot der GitHub-Actions-Übersicht

Die verschiedenen Python-Umgebungen sind auf der linken Seite aufgelistet. Wenn
ihr eine auswählt, werden die Ergebnisse für diese Umgebung angezeigt, wie im
folgenden Screenshot dargestellt:

.. figure:: github-actions-run.png
   :alt: Screenshot eines GitHub-Actions-Run für eine Umgebung

.. seealso::
   * `Building and testing Python
     <https://docs.github.com/en/actions/automating-builds-and-tests/building-and-testing-python>`_
   * `Workflow syntax for GitHub Actions
     <https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions>`_

Badge anzeigen
--------------

Nun könnt ihr in eurer :file:`README.rst`-Datei noch ein Badge eures
:term:`CI`-Status hinzufügen, :abbr:`z.B. (zum Beispiel)` mit:

.. code-block::

   .. image:: https://github.com/YOU/YOUR_PROJECT/workflows/CI/badge.svg?branch=main
      :target: https://github.com/YOU/YOUR_PROJECT/actions?workflow=CI
      :alt: CI Status

Testabdeckung veröffentlichen
-----------------------------

Die Testabdeckung könnt ihr auf GitHub veröffentlichen, :abbr:`s.a. (siehe
auch)` :ref:`Coverage GitHub-Actions <coverage-github-actions>`.

tox erweitern
-------------

tox verwendet `pluggy <https://pluggy.readthedocs.io/en/stable/>`_, um das
Standardverhalten anzupassen. Pluggy findet ein Plugin, indem es nach
einem Einstiegspunkt mit dem Namen ``tox`` sucht, :abbr:`z.B. (zum Beispiel)` in
einer :file:`pyproject.toml`-Datei:

.. code-block:: toml

    [project.entry-points.tox]
    my_plugin = "my_plugin.hooks"

Um das Plugin zu verwenden, muss es daher lediglich in der gleichen Umgebung
installiert werden, in der auch tox läuft, und es wird über den definierten
Einstiegspunkt gefunden.

Ein Plugin wird durch die Implementierung von Erweiterungspunkten in Form von
Hooks erstellt. Der folgende Codeschnipsel würde zum Beispiel ein neues --my
:abbr:`CLI (Command Line Interface)` definieren:

.. code-block:: python

    from tox.config.cli.parser import ToxParser
    from tox.plugin import impl


    @impl
    def tox_add_option(parser: ToxParser) -> None:
        parser.add_argument("--my", action="store_true", help="my option")

.. seealso::
   * `Extending tox <https://tox.wiki/en/latest/plugins.html>`_
   * `tox development team <https://github.com/orgs/tox-dev/repositories>`_
