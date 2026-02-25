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
um eine installierbare :doc:`Distribution eures Pakets
<../packs/distribution>` zu erstellen. Es sucht im ``[tool.tox]``-Abschnitt der
:file:`pyproject.toml`-Datei nach einer Liste von Umgebungen, und führt dann
jeweils folgende Schritte aus:

#. erstellt eine :term:`virtuelle Umgebung <Virtuelle Umgebung>`
#. installiert einige Abhängigkeiten mit :term:`pip`
#. baut euer Paket
#. installiert euer Paket mit pip
#. führt weitere Tests aus

Nachdem alle Umgebungen getestet wurden, gibt tox eine Zusammenfassung der
Ergebnisse aus.

Um diesen Prozess mit :term:`uv` zu beschleunigen, verwenden wir tox nicht
direkt, sondern `tox-uv <https://github.com/tox-dev/tox-uv>`_.

.. note::
   Obwohl tox von vielen Projekten verwendet wird, gibt es Alternativen, die
   ähnliche Funktionen erfüllen. Zwei Alternativen zu tox sind `nox
   <https://nox.thea.codes/en/stable/>`_ und `invoke
   <https://www.pyinvoke.org>`_.

tox einrichten
--------------

Früher wurde tox üblicherweise in der :file:`tox.ini`-Datei konfiguriert. Seit
tox 4.44.0 ist deren Funktionalität jedoch eingefroren und zukünftige
Konfigurationsparameter werden wohl nur noch in einer
:doc:`Python4DataScience:data-processing/serialisation-formats/toml/index`-Datei
bereitgestellt werden, :abbr:`z. B. (zum Beispiel)` in der
:ref:`pyproject-toml`-Datei. Werfen wir einen Blick auf eine einfache
Konfiguration in der :file:`pyproject.toml`-Datei:

.. code-block:: toml

   [tool.tox]
   env_list = ["py313"]

   [tool.tox.env_run_base]
   dependency_groups = [ "tests" ]
   commands = [[ "pytest"]]

Im ``[tool.tox]``-Abschnitt haben wir ``env_list = ["py313"]`` definiert. Dies
ist eine Abkürzung, die tox anweist, unsere Tests mit Python Version 3.13
durchzuführen. Wir werden in Kürze weitere Python-Versionen hinzufügen, aber die
Verwendung einer Version hilft uns zunächst, den Ablauf von tox besser zu
verstehen.

Im ``[tool.tox.env_run_base]``-Abschnitt wird in ``dependency_groups`` ``tests``
angegeben. Somit weiß tox, dass die entsprechenden Bibliotheken in dieser
Umgebung installiert werden sollen. Mit ``commands`` wird tox schließlich
angewiesen, ``pytest`` auszuführen.

tox ausführen
-------------

Bevor ihr tox ausführen könnt, müsst ihr sicherstellen, dass ihr tox-uv
installiert habt:

.. tab:: Linux/macOS

   .. code-block:: console

      $ uv add --group dev tox-uv

.. tab:: Windows

   .. code-block:: ps1con

      C:> uv add --group dev tox-uv

Um tox auszuführen, startet einfach tox:

.. code-block:: pytest

   $ uv run tox
   .pkg: _optional_hooks> python /Users/veit/cusy/prj/items/.venv/lib/python3.13/site-packages/pyproject_api/_backend.py True hatchling.build
   .pkg: get_requires_for_build_sdist> python /Users/veit/cusy/prj/items/.venv/lib/python3.13/site-packages/pyproject_api/_backend.py True hatchling.build
   .pkg: build_sdist> python /Users/veit/cusy/prj/items/.venv/lib/python3.13/site-packages/pyproject_api/_backend.py True hatchling.build
   py313: install_package> .venv/bin/uv pip install --reinstall --no-deps items@/Users/veit/cusy/prj/items/.tox/.tmp/package/18/items-0.1.0.tar.gz
   py313: commands[0]> python --version --version
   Python 3.13.0 (main, Oct  7 2024, 23:47:22) [Clang 18.1.8 ]
   py313: commands[1]> coverage run -m pytest
   ============================= test session starts ==============================
   platform darwin -- Python 3.13.0, pytest-9.0.2, pluggy-1.6.0
   cachedir: .tox/py313/.pytest_cache
   rootdir: /Users/veit/cusy/prj/items
   configfile: pyproject.toml
   testpaths: tests
   plugins: Faker-40.1.0, cov-7.0.0
   collected 83 items
   tests/api/test_add.py ......                                             [  7%]
   tests/api/test_config.py .                                               [  8%]
   tests/api/test_count.py ...                                              [ 12%]
   tests/api/test_delete.py ...                                             [ 15%]
   tests/api/test_delete_all.py ..                                          [ 18%]
   tests/api/test_exceptions.py ..                                          [ 20%]
   tests/api/test_finish.py ....                                            [ 25%]
   tests/api/test_item.py ...                                               [ 28%]
   tests/api/test_item_id.py .                                              [ 30%]
   tests/api/test_list.py .........                                         [ 40%]
   tests/api/test_list_edge_cases.py ........                               [ 50%]
   tests/api/test_start.py ....                                             [ 55%]
   tests/api/test_update.py .....                                           [ 61%]
   tests/api/test_version.py .                                              [ 62%]
   tests/cli/test_add.py ..                                                 [ 65%]
   tests/cli/test_config.py ..                                              [ 67%]
   tests/cli/test_count.py .                                                [ 68%]
   tests/cli/test_delete.py .                                               [ 69%]
   tests/cli/test_errors.py .......                                         [ 78%]
   tests/cli/test_finish.py .                                               [ 79%]
   tests/cli/test_help.py .........                                         [ 90%]
   tests/cli/test_list.py .....                                             [ 96%]
   tests/cli/test_start.py .                                                [ 97%]
   tests/cli/test_update.py .                                               [ 98%]
   tests/cli/test_version.py .                                              [100%]

   ============================== 83 passed in 0.35s ==============================
   .pkg: _exit> python /Users/veit/cusy/prj/items/.venv/lib/python3.13/site-packages/pyproject_api/_backend.py True hatchling.build
     py313: OK (1.19=setup[0.45]+cmd[0.01,0.72] seconds)
     congratulations :) (1.23 seconds)

Mehrere Python-Versionen testen
-------------------------------

Hierfür erweitern wir ``envlist`` in der :file:`pyproject.toml`-Datei um weitere
Python-Versionen hinzuzufügen:

.. code-block:: toml

   [tool.tox]
   env_list = [
     "py3{10-14}",
     "py{13-14}t",
   ]
   skip_missing_interpreters = true

Damit werden wir jetzt Python-Versionen von 3.10 bis 3.14 testen. Zusätzlich
haben wir auch die Einstellung ``skip_missing_interpreters = true`` hinzugefügt,
damit tox nicht fehlschlägt, wenn auf eurem System eine der aufgeführten
Python-Versionen fehlt. Ist der Wert auf ``true`` gesetzt, führt tox die Tests
mit jeder verfügbaren Python-Version durch, überspringt aber Versionen, die es
nicht findet, ohne fehlzuschlagen. Die Ausgabe ist sehr ähnlich, wobei ich in
der folgenden Darstellung lediglich die Unterschiede hervorhebe:

.. code-block:: pytest
    :emphasize-lines: 3-4, 8-12, 16-20, 24-28, 32-

    $ uv run tox
    .pkg: _optional_hooks> python /Users/veit/cusy/prj/items/.venv/lib/python3.13/site-packages/pyproject_api/_backend.py True hatchling.build
    .pkg: get_requires_for_build_sdist> python /Users/veit/cusy/prj/items/.venv/lib/python3.13/site-packages/pyproject_api/_backend.py True hatchling.build
    .pkg: build_sdist> python /Users/veit/cusy/prj/items/.venv/lib/python3.13/site-packages/pyproject_api/_backend.py True hatchling.build
    py310: install_package> .venv/bin/uv pip install --reinstall --no-deps items@/Users/veit/cusy/prj/items/.tox/.tmp/package/19/items-0.1.0.tar.gz
    py310: commands[0]> python --version --version
    Python 3.10.17 (main, Apr  9 2025, 03:47:39) [Clang 20.1.0 ]
    py310: commands[1]> coverage run -m pytest
    ============================= test session starts ==============================
    ...
    ============================== 83 passed in 0.35s ==============================
    py310: OK ✔ in 1.3 seconds
    py311: install_package> .venv/bin/uv pip install --reinstall --no-deps items@/Users/veit/cusy/prj/items/.tox/.tmp/package/20/items-0.1.0.tar.gz
    py311: commands[0]> python --version --version
    Python 3.11.11 (main, Feb  5 2025, 18:58:27) [Clang 19.1.6 ]
    py311: commands[1]> coverage run -m pytest
    ============================= test session starts ==============================
    ...
    ============================== 83 passed in 0.36s ==============================
    py311: OK ✔ in 1.16 seconds
    py312: install_package> .venv/bin/uv pip install --reinstall --no-deps items@/Users/veit/cusy/prj/items/.tox/.tmp/package/21/items-0.1.0.tar.gz
    py312: commands[0]> python --version --version
    Python 3.12.12 (main, Oct 14 2025, 21:38:21) [Clang 20.1.4 ]
    py312: commands[1]> coverage run -m pytest
    ============================= test session starts ==============================
    ...
    ============================== 83 passed in 0.55s ==============================
    py312: OK ✔ in 1.79 seconds
    py313: install_package> .venv/bin/uv pip install --reinstall --no-deps items@/Users/veit/cusy/prj/items/.tox/.tmp/package/22/items-0.1.0.tar.gz
    py313: commands[0]> python --version --version
    Python 3.13.0 (main, Oct  7 2024, 23:47:22) [Clang 18.1.8 ]
    py313: commands[1]> coverage run -m pytest
    ============================= test session starts ==============================
    ...
    ============================== 83 passed in 0.35s ==============================
    py313: OK ✔ in 1.07 seconds
    py314: install_package> .venv/bin/uv pip install --reinstall --no-deps items@/Users/veit/cusy/prj/items/.tox/.tmp/package/23/items-0.1.0.tar.gz
    py314: commands[0]> python --version --version
    Python 3.14.0 (main, Oct 14 2025, 21:10:22) [Clang 20.1.4 ]
    py314: commands[1]> coverage run -m pytest
    ============================= test session starts ==============================
    ...
    ============================== 83 passed in 0.36s ==============================
    py314: OK ✔ in 1.28 seconds
    py313t: install_package> .venv/bin/uv pip install --reinstall --no-deps items@/Users/veit/cusy/prj/items/.tox/.tmp/package/24/items-0.1.0.tar.gz
    py313t: commands[0]> python --version --version
    Python 3.13.0 experimental free-threading build (main, Oct 16 2024, 08:24:33) [Clang 18.1.8 ]
    py313t: commands[1]> coverage run -m pytest
    ============================= test session starts ==============================
    ...
    ============================== 83 passed in 0.49s ==============================
    py313t: OK ✔ in 1.51 seconds
    py314t: install_package> .venv/bin/uv pip install --reinstall --no-deps items@/Users/veit/cusy/prj/items/.tox/.tmp/package/25/items-0.1.0.tar.gz
    py314t: commands[0]> python --version --version
    Python 3.14.0b4 free-threading build (main, Jul  8 2025, 21:06:49) [Clang 20.1.4 ]
    py314t: commands[1]> coverage run -m pytest
    ============================= test session starts ==============================
    ...
    ============================== 83 passed in 0.39s ==============================
    .pkg: _exit> python /Users/veit/cusy/prj/items/.venv/lib/python3.13/site-packages/pyproject_api/_backend.py True hatchling.build
      py310: OK (1.30=setup[0.54]+cmd[0.01,0.75] seconds)
      py311: OK (1.16=setup[0.38]+cmd[0.01,0.76] seconds)
      py312: OK (1.79=setup[0.42]+cmd[0.01,1.36] seconds)
      py313: OK (1.07=setup[0.34]+cmd[0.01,0.71] seconds)
      py314: OK (1.28=setup[0.42]+cmd[0.01,0.85] seconds)
      py313t: OK (1.51=setup[0.44]+cmd[0.01,1.05] seconds)
      py314t: OK (1.34=setup[0.44]+cmd[0.01,0.89] seconds)
      congratulations :) (9.48 seconds)

.. versionchanged:: tox 4.25.0
   Vor tox 4.25.0 vom 27. März 2025 mussten die verschiedenen Python-Versionen
   einzeln angegeben werden:

   .. code-block:: toml

      [tool.tox]
      envlist = [py3{10,11,12,13,14,13t,14t}]

Tox-Umgebungen parallel ausführen
---------------------------------

Im vorherigen Beispiel wurden die verschiedenen Umgebungen nacheinander
ausgeführt. Es ist auch möglich, sie mit der Option ``-p`` parallel laufen zu
lassen:

.. code-block:: pytest

   $ uv run tox -p
   py311: OK ✔ in 1.7 seconds
   py310: OK ✔ in 1.8 seconds
   py313: OK ✔ in 1.8 seconds
   py314t: OK ✔ in 1.89 seconds
   py314: OK ✔ in 1.91 seconds
   py313t: OK ✔ in 2.24 seconds
     py310: OK (1.80=setup[0.62]+cmd[0.02,1.16] seconds)
     py311: OK (1.70=setup[0.54]+cmd[0.02,1.15] seconds)
     py312: OK (2.28=setup[0.58]+cmd[0.01,1.69] seconds)
     py313: OK (1.80=setup[0.60]+cmd[0.02,1.18] seconds)
     py314: OK (1.91=setup[0.62]+cmd[0.02,1.28] seconds)
     py313t: OK (2.24=setup[0.72]+cmd[0.02,1.51] seconds)
     py314t: OK (1.89=setup[0.61]+cmd[0.02,1.26] seconds)
     congratulations :) (2.33 seconds)

.. note::
   Die Ausgabe ist nicht abgekürzt; dies ist die gesamte Ausgabe, die ihr seht,
   wenn alles funktioniert.

Coverage-Report in tox hinzufügen
---------------------------------

Der :file:`pyproject.toml`-Datei kann einfach die Konfiguration von Coverage
Reports hinzugefügt werden. Dazu müssen wir ``pytest-cov`` in der
``tests``-Abhängigkeitsgruppe hinzufügen, damit das ``pytest-cov``-Plugin auch
in den tox-Testumgebungen installiert wird. Das Einbinden von ``pytest-cov``
schließt auch alle weiteren Abhängigkeiten ein, wie :abbr:`z. B. (zum Beispiel)`
Coverage. Wir fügen dann noch die :samp:`env.coverage-report.{OPTIONS}` hinzu und ändern ``env_run_base.commands``:

.. code-block:: toml
   :emphasize-lines: 6, 16-23, 26-

   [dependency-groups]
   ...
   tests = [
     "faker",
     "pytest>=6",
     "pytest-cov",
   ]

   [tool.tox]
   requires = [ "tox>=4" ]
   env_list = [
     "py3{10-14}",
     "py{13-14}t",
   ]
   skip_missing_interpreters = true
   env.coverage-report.description = "Report coverage over all test runs."
   env.coverage-report.deps = [ "coverage[toml]" ]
   env.coverage-report.depends = [ "py" ]
   env.coverage-report.skip_install = true
   env.coverage-report.commands = [
     [ "coverage combine" ],
     [ "coverage report" ],
   ]
   env_run_base.dependency_groups = [ "tests" ]
   env_run_base.deps = [ "coverage[toml]" ]
   env_run_base.commands = [
     [ "coverage", "run", "-m", "pytest" ],
   ]

Bei der Verwendung von Coverage mit ``tox`` kann es manchmal sinnvoll sein, in
der  :file:`pyproject.toml`-Datei einen Abschnitt einzurichten, der Coverage
mitteilt, welche Quelltext-Pfade als identisch betrachtet werden sollen:

.. code-block:: toml

   [tool.coverage.paths]
   source = ["src", ".tox/py*/**/site-packages"]

Der Items-Quellcode befindet sich zunächst in :file:`src/items/`, bevor von tox
die virtuellen Umgebungen erstellt und Items in der Umgebung installiert wird.
Dann befindet es sich :abbr:`z.B. (zum Beispiel)` in
:file:`.tox/py313/lib/python3.13/site-packages/items`.

.. code-block:: console
   :emphasize-lines: 1

   $ uv run tox
   ...
   Name    Stmts   Miss Branch BrPart  Cover   Missing
   ---------------------------------------------------
   TOTAL     540      0     32      0   100%

   33 files skipped due to complete coverage.
     py310: OK (1.10=setup[0.44]+cmd[0.01,0.64] seconds)
     py311: OK (0.98=setup[0.31]+cmd[0.01,0.66] seconds)
     py312: OK (1.59=setup[0.34]+cmd[0.01,1.24] seconds)
     py313: OK (1.06=setup[0.34]+cmd[0.01,0.71] seconds)
     py314: OK (1.10=setup[0.35]+cmd[0.01,0.74] seconds)
     py313t: OK (1.36=setup[0.40]+cmd[0.01,0.95] seconds)
     py314t: OK (1.31=setup[0.44]+cmd[0.01,0.86] seconds)
     coverage-report: OK (1.55=setup[0.37]+cmd[1.08,0.10] seconds)
     congratulations :) (10.09 seconds)

Mindestabdeckungsgrad festlegen
-------------------------------

Bei der Ausführung der Coverage durch tox ist auch sinnvoll, einen
Mindestabdeckungsgrad festzulegen, um eventuelle Ausrutscher bei der Coverage zu
erkennen. Dies wird mit der Option ``--cov-fail-under`` erreicht:

.. code-block:: console
   :emphasize-lines: 8

   Name               Stmts   Miss Branch BrPart  Cover   Missing
   --------------------------------------------------------------
   src/items/api.py      68      1     12      1    98%   88
   --------------------------------------------------------------
   TOTAL                428      1     32      1    99%

   26 files skipped due to complete coverage.
   Coverage failure: total of 99 is less than fail-under=100

Dadurch wird der Ausgabe die hervorgehobene Zeile hinzugefügt.

.. _posargs:

pytest-Parameter an tox übergeben
---------------------------------

Wir können auch einzelne Tests mit tox aufrufen, indem wir eine weitere Änderung
vornehmen, damit Parameter an pytest übergeben werden können:

.. code-block:: toml
   :emphasize-lines: 15

   [tool.tox]
   requires = [ "tox>=4" ]
   env_list = [
     "pre-commit",
     "docs",
     "py3{10-14}",
     "py{13-14}t",
     "coverage-report",
   ]
   skip_missing_interpreters = true
   env_run_base.dependency_groups = [ "tests" ]
   env_run_base.deps = [ "coverage[toml]" ]
   env_run_base.commands = [
     [ "python", "--version", "--version" ],
     [ "coverage", "run", "-m", "pytest", "{posargs}" ],
   ]

Um Argumente an pytest zu übergeben, fügt sie zwischen den tox-Argumenten und
den pytest-Argumenten ein. In diesem Fall wählen wir ``test_version``-Tests mit
der Schlüsselwort-Option ``-k`` aus. Wir verwenden auch ``--no-cov``, um die
Abdeckung zu deaktivieren:

.. code-block:: pytest
   :emphasize-lines: 1, 3

   $ uv run tox -e py313 -- -k test_version --no-cov
   ...
   coverage-report: commands[0]> coverage combine
   Combined data file .coverage.fay.local.19539.XpQXpsGx
   coverage-report: commands[1]> coverage report
   Name               Stmts   Miss Branch BrPart  Cover   Missing
   --------------------------------------------------------------
   src/items/api.py      68      1     12      1    98%   88
   --------------------------------------------------------------
   TOTAL                428      1     32      1    99%

   26 files skipped due to complete coverage.
     py310: OK (2.12=setup[1.49]+cmd[0.63] seconds)
     py311: OK (1.41=setup[0.80]+cmd[0.62] seconds)
     py312: OK (1.43=setup[0.81]+cmd[0.62] seconds)
     py313: OK (1.46=setup[0.83]+cmd[0.62] seconds)
     coverage-report: OK (0.16=setup[0.00]+cmd[0.07,0.09] seconds)
     congratulations :) (10.26 seconds)

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
          name: Ensure 100% test coverage
          runs-on: ubuntu-latest
          needs: tests
          if: always()

          steps:
            - uses: actions/checkout@v6
              with:
                persist-credentials: false
            - uses: actions/setup-python@v6
              with:
                python-version-file: .python-version
            - uses: hynek/setup-cached-uv@v2

            - name: Download coverage data
              uses: actions/download-artifact@v7
              with:
                pattern: coverage-data-*
                merge-multiple: true

            - name: Combine coverage and fail if it’s <100%.
              run: |
                uv tool install coverage

                coverage combine
                coverage html --skip-covered --skip-empty

                # Report and write to summary.
                coverage report --format=markdown >> $GITHUB_STEP_SUMMARY

                # Report again and fail if under 100%.
                coverage report --fail-under=100

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
   ``uses: hynek/setup-cached-uv@v2``
       uses :term:`uv` in GitHub Actions.

       .. seealso::
          * `setup-cached-uv <https://github.com/hynek/setup-cached-uv>`_

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
<https://docs.github.com/en/actions/use-cases-and-examples/building-and-testing/building-and-testing-python>`_.
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
     <https://docs.github.com/en/actions/use-cases-and-examples/building-and-testing/building-and-testing-python>`_
   * `Workflow syntax for GitHub Actions
     <https://docs.github.com/en/actions/writing-workflows/workflow-syntax-for-github-actions>`_

Badge anzeigen
--------------

Nun könnt ihr in eurer :file:`README.rst`-Datei noch ein Badge eures
:term:`CI`-Status hinzufügen, :abbr:`z.B. (zum Beispiel)` mit:

.. code-block:: rest

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

.. _tox_uv:

``tox-uv``
----------

`tox-uv <https://pypi.org/project/tox-uv/>`__ ist ein Tox-Plugin, das
:term:`virtualenv` und :term:`pip` durch :term:`uv` in euren Tox-Umgebungen
ersetzt.

Ihr könnt ``tox`` und ``tox-uv`` installieren mit:

.. code-block:: console

   $ uv tool install tox --with tox-uv

``uv.lock``-Unterstützung
~~~~~~~~~~~~~~~~~~~~~~~~~

Wenn ihr für eine Tox-Umgebung ``uv sync`` mit einer :file:`uv.lock`-Datei
verwenden wollt, müsst ihr für diese Tox-Umgebung den Runner auf
``uv-venv-lock-runner`` ändern, zum Beispiel:

.. code-block:: toml
   :caption: pyproject.toml

   env.app.dependency_groups = [ "tests" ]
   env.app.runner = "uv-venv-lock-runner"
   commands = [[ "pytest"]]

Die ``app``-Umgebung  verwendet den ``uv-venv-lock-runner`` und nutzt ``uv sync --locked``, um die Abhängigkeiten in den Versionen der :file:`uv.lock`-Datei zu
installieren.

.. seealso::
   * `uv.lock support
     <https://github.com/tox-dev/tox-uv?tab=readme-ov-file#uvlock-support>`_
