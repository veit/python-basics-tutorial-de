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

tox1 ist ein Kommandozeilen-Tool, mit dem ihr eure komplette Testsuite in
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
   envlist = py311
   isolated_build = True

   [testenv]
   deps =
     pytest>=6.0
     faker
   commands = pytest

Im ``[tox]``-Abschnitt haben wir ``envlist = py311`` definiert. Dies ist eine
Abkürzung, die tox anweist, unsere Tests mit Python Version 3.11 durchzuführen.
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

    $ tox
    .pkg: _optional_hooks> python /PATH/TO/items/lib/python3.11/site-packages/pyproject_api/_backend.py True hatchling.build
    .pkg: get_requires_for_build_sdist> python PATH/TO/items/lib/python3.11/site-packages/pyproject_api/_backend.py True hatchling.build
    .pkg: build_sdist> python PATH/TO/items/lib/python3.11/site-packages/pyproject_api/_backend.py True hatchling.build
    py311: install_package> python -I -m pip install --force-reinstall --no-deps PATH/TO/items/.tox/.tmp/package/14/items-0.1.0.tar.gz
    py311: commands[0]> pytest
    ============================= test session starts ==============================
    ...
    configfile: pyproject.toml
    testpaths: tests
    plugins: Faker-19.11.0
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

    ============================== 49 passed in 0.08s ==============================
    .pkg: _exit> python /PATCH/TO/items/lib/python3.11/site-packages/pyproject_api/_backend.py True hatchling.build
      py311: OK (1.48=setup[1.21]+cmd[0.27] seconds)
      congratulations :) (1.51 seconds)

Mehrere Python-Versionen testen
-------------------------------

Hierfür erweitern wir ``envlist`` in der :file:`tox.ini`-Datei um weitere
Python-Versionen hinzuzufügen:

.. code-block:: ini
   :emphasize-lines: 2, 4

   [tox]
   envlist = py38, py39, py310, py311
   isolated_build = True
   skip_missing_interpreters = True

Damit werden wir jetzt Python-Versionen von 3.8 bis 3.11 testen. Zusätzlich
haben wir auch die Einstellung ``skip_missing_interpreters = True`` hinzugefügt,
damit tox nicht fehlschlägt, wenn auf eurem System eine der aufgeführten
Python-Versionen fehlt. Ist der Wert auf ``True`` gesetzt, führt tox die Tests
mit jeder verfügbaren Python-Version durch, überspringt aber Versionen, die es
nicht findet, ohne fehlzuschlagen. Die Ausgabe ist sehr ähnlich, wobei ich in
der folgenden Darstellung lediglich die Unterschiede hervorhebe:

.. code-block:: pytest
   :emphasize-lines: 2, 4, 10, 12, 18-

    $ tox
    py38: skipped because could not find python interpreter with spec(s): py38
    py38: SKIP ⚠ in 2.13 seconds
    py39: install_package> python -I -m pip install --force-reinstall --no-deps /PATCH/TO/items/.tox/.tmp/package/15/items-0.1.0.tar.gz
    py39: commands[0]> pytest
    ============================= test session starts ==============================
    ...
    ============================== 49 passed in 0.16s ==============================
    py39: OK ✔ in 8.08 seconds
    py310: skipped because could not find python interpreter with spec(s): py310
    py310: SKIP ⚠ in 0 seconds
    py311: install_package> python -I -m pip install --force-reinstall --no-deps /PATH/TO/items/.tox/.tmp/package/16/items-0.1.0.tar.gz
    py311: commands[0]> pytest
    ============================= test session starts ==============================
    ...
    ============================== 49 passed in 0.09s ==============================
    .pkg: _exit> python /PYTH/TO/items/lib/python3.11/site-packages/pyproject_api/_backend.py True hatchling.build
      py38: SKIP (2.13 seconds)
      py39: OK (8.08=setup[6.92]+cmd[1.16] seconds)
      py310: SKIP (0.00 seconds)
      py311: OK (1.24=setup[0.95]+cmd[0.29] seconds)
      congratulations :) (11.48 seconds)

Tox-Umgebungen parallel ausführen
---------------------------------

Im vorherigen Beispiel wurden die verschiedenen Umgebungen nacheinander
ausgeführt. Es ist auch möglich, sie mit der Option ``-p`` parallel laufen zu
lassen:

.. code-block:: pytest

    $ tox -p
    py38: SKIP ⚠ in 0.02 seconds
    py310: SKIP ⚠ in 0.29 seconds
    py311: OK ✔ in 1.53 seconds
      py38: SKIP (0.02 seconds)
      py39: OK (2.21=setup[1.88]+cmd[0.33] seconds)
      py310: SKIP (0.29 seconds)
      py311: OK (1.53=setup[1.24]+cmd[0.29] seconds)
      congratulations :) (2.24 seconds)

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
   :emphasize-lines: 10-

   [tox]
   envlist = py38, py39, py310, py311
   isolated_build = True
   skip_missing_interpreters = True

   [testenv]
   deps =
     pytest
     faker
     pytest-cov
   commands = pytest --cov=items

Bei der Verwendung von Coverage mit tox kann es manchmal sinnvoll sein, eine
:file:`.coveragerc`-Datei einzurichten um Coverage mitzuteilen, welche
Quelltextpfade als identisch betrachtet werden sollen:

.. code-block:: ini

    [paths]
    source =
       src
       .tox/*/site-packages

Der Items-Quellcode befindet sich zunächst in :file:`src/items/`, bevor von tox
die virtuellen Umgebungen erstellt und Items in der Umgebung installiert wird.
Dann befindet es sich :abbr:`z.B. (zum Beispiel)` in
:file:`.tox/py311/lib/python3.11/site-packages/items`.

.. code-block:: pytest
   :emphasize-lines: 1

    $ tox -e py311
    ...
    py311: commands[0]> pytest --cov=items
    ...
    ---------- coverage: platform darwin, python 3.11.5-final-0 ----------
    Name                                                        Stmts   Miss  Cover
    -------------------------------------------------------------------------------
    .tox/py311/lib/python3.11/site-packages/items/__init__.py       3      0   100%
    .tox/py311/lib/python3.11/site-packages/items/api.py           68      1    99%
    .tox/py311/lib/python3.11/site-packages/items/cli.py           86      0   100%
    .tox/py311/lib/python3.11/site-packages/items/db.py            23      0   100%
    -------------------------------------------------------------------------------
    TOTAL                                                         180      1    99%


    ============================== 49 passed in 0.17s ==============================
    ...
      py311: OK (1.85=setup[1.34]+cmd[0.51] seconds)
      congratulations :) (1.89 seconds)

.. note::
   Wir haben hier die Option ``-e py311`` verwendet, um eine bestimmte Umgebung
   auszuwählen.

Mindestabdeckungsgrad festlegen
-------------------------------

Bei der Ausführung der Coverage durch tox ist auch sinnvoll, einen
Mindestabdeckungsgrad festzulegen, um eventuelle Ausrutscher bei der Coverage zu
erkennen. Dies wird mit der Option ``--cov-fail-under`` erreicht:

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
   commands = pytest --cov=items --cov-fail-under=100

Dadurch wird der Ausgabe eine zusätzliche Zeile hinzugefügt:

.. code-block:: pytest
   :emphasize-lines: 15

    $ tox -e py311
    ...
    ============================= test session starts ==============================
    ...
    ---------- coverage: platform darwin, python 3.11.5-final-0 ----------
    Name                                                        Stmts   Miss  Cover
    -------------------------------------------------------------------------------
    .tox/py311/lib/python3.11/site-packages/items/__init__.py       3      0   100%
    .tox/py311/lib/python3.11/site-packages/items/api.py           68      1    99%
    .tox/py311/lib/python3.11/site-packages/items/cli.py           86      0   100%
    .tox/py311/lib/python3.11/site-packages/items/db.py            23      0   100%
    -------------------------------------------------------------------------------
    TOTAL                                                         180      1    99%

    FAIL Required test coverage of 100% not reached. Total coverage: 99.44%

    ============================== 49 passed in 0.16s ==============================
    py311: exit 1 (0.43 seconds) /PATH/TO/items> pytest --cov=items --cov-fail-under=100 pid=58109
    .pkg: _exit> python /PATH/TO/items/lib/python3.11/site-packages/pyproject_api/_backend.py True hatchling.build
      py311: FAIL code 1 (1.65=setup[1.22]+cmd[0.43] seconds)
      evaluation failed :( (1.68 seconds)

pytest-Parameter an tox übergeben
---------------------------------

Wir können auch einzelne Tests mit tox aufrufen, indem wir eine weitere Änderung
vornehmen, damit Parameter an pytest übergeben werden können:

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
    commands = pytest --cov=items --cov-fail-under=100  {posargs}

Um Argumente an pytest zu übergeben, fügt sie zwischen den tox-Argumenten und
den pytest-Argumenten ein. In diesem Fall wählen wir ``test_version``-Tests mit
der Schlüsselwort-Option ``-k`` aus. Wir verwenden auch ``--no-cov``, um die
Abdeckung zu deaktivieren:

.. code-block:: pytest
   :emphasize-lines: 1, 3

    $ tox -e py311 -- -k test_version --no-cov
    ...
    py311: commands[0]> pytest --cov=items --cov-fail-under=100 -k test_version --no-cov
    ============================= test session starts ==============================
    ...
    configfile: pyproject.toml
    testpaths: tests
    plugins: cov-4.1.0, Faker-19.11.0
    collected 49 items / 47 deselected / 2 selected

    tests/api/test_version.py .                                              [ 50%]
    tests/cli/test_version.py .                                              [100%]

    ======================= 2 passed, 47 deselected in 0.04s =======================
    .pkg: _exit> python /PATH/TO/items/lib/python3.11/site-packages/pyproject_api/_backend.py True hatchling.build
      py311: OK (1.51=setup[1.25]+cmd[0.26] seconds)
      congratulations :) (1.53 seconds)

tox eignet sich nicht nur hervorragend für die lokale Automatisierung von
Testprozessen, sondern hilft auch bei Server-basierter :term:`CI`. Fahren wir
fort mit der Ausführung von pytest und tox mithilfe von GitHub-Aktionen.

Tox mit GitHub-Aktionen ausführen
---------------------------------

Wenn euer Projekt auf `GitHub <https://github.com/>`_ gehostet ist, könnt ihr
GitHub-Actions verwenden um automatisiert eure Tests in verschiedenen Umgebungen
ausführen zu können. Dabei sind eine ganze Reihe von Umgebungen für die
GitHub-Actions verfügbar: `github.com/actions/virtual-environments
<https://github.com/actions/virtual-environments/#readme>`_.

#. Um eine GitHub-Action in eurem Projekt zu erstellen, klickt auf
   :menuselection:`Actions --> set up a workflow yourself`. Dies erstellt
   üblicherweise eine Datei :file:`.github/workflows/main.yml`.
#. Gebt dieser Datei einen aussagekräftigeren Namen. Wir verwenden hierfür
   üblicherweise :file:`ci.yml`.
#. Die vorausgefüllte YAML-Datei ist für unsere Zwecke wenig hilfreich. Ihr
   könnt den Text ersetzen, :abbr:`z.B. (zum Beispiel)` mit:

   .. code-block:: yaml

      name: CI
      on: [push, pull_request]
      jobs:
        build:
          runs-on: ubuntu-latest
          strategy:
            matrix:
              python: ["3.8", "3.9", "3.10", "3.11"]
          steps:
            - uses: actions/checkout@v2
            - name: Setup Python
              uses: actions/setup-python@v2
              with:
                python-version: ${{ matrix.python }}
            - name: Install tox and any other packages
              run: python -m pip install tox tox-gh-actions
            - name: Run tox for "${{ matrix.python }}"
              run: python -m tox

   ``name``
       kann ein beliebiger Name sein. Er wird in der Benutzeroberfläche von
       GitHub Actions angezeigt.
   ``on: [push, pull_request]``
       weist Actions an, unsere Tests jedes Mal auszuführen, wenn wir entweder
       Code in das Repository pushen oder ein Pull-Request erstellt wird. Bei
       Pull-Requests kann das Ergebnis des Testlaufs in der
       Pull-Request-Schnittstelle eingesehen werden. Alle Ergebnisse der
       GitHub-Actions sind auf der GitHub-Benutzeroberfläche zu sehen.
   ``runs-on: ubuntu-latest``
       gibt an, auf welchem Betriebssystem die Tests ausgeführt werden sollen.
       Hier laufen die Tests nur unter Linux, aber auch andere Betriebssysteme
       sind verfügbar.
   ``matrix: python: ["3.8", "3.9", "3.10", "3.11"]``
       gibt an, welche Python-Version ausgeführt werden soll.
   ``steps``
       ist eine Liste von Schritten. Der Name eines jeden Schrittes kann
       beliebig sein und ist optional.
   ``uses: actions/checkout@v2``
       ist ein GitHub-Actions-Tool, das unser Repository auscheckt, damit der
       Rest des Workflows darauf zugreifen kann.
   ``uses: actions/setup-python@v2``
       ist ein GitHub-Actions-Tool, das Python konfiguriert und in einer
       Build-Umgebung installiert.
   ``with: python-version: ${{ matrix.python }}``
       sagt, dass eine Umgebung für jede der in ``matrix.python`` aufgeführten
       Python-Versionen erstellt werden soll.
   ``run: python -m pip install tox tox-gh-actions``
       installiert tox und vereinfacht mit `tox-gh-actions
       <https://pypi.org/project/tox-gh-actions/>`_ das Ausführen von tox in
       GitHub-Actions indem es als Umgebung für die Tests diejenige
       bereitstellt, die auch tox selbst verwendet. Hierfür müssen wir jedoch
       noch unsere :file:`tox.ini`-Datei anpassen, :abbr:`z.B. (zum Beispiel)`:

       .. code-block:: ini

          [gh-actions]
          python =
              3.8: py38
              3.9: py39
              3.10: py310
              3.11: py311

       Dies ordnet GitHub-Actions tox-Umgebungen zu.

       .. note::
          * Es müssen nicht alle Varianten eurer Umgebung angegeben werden. Dies
            unterscheidet ``tox-gh-actions`` von ``tox -e py``.
          * Stellt sicher, dass die Versionen im ``[gh-actions]``-Abschnitt mit
            den verfügbaren Python-Versionen und :abbr:`ggf. (gegebenenfalls)`
            mit denen in den :ref:`GitHub-Actions für Git pre-commit Hooks
            <gh-action-pre-commit-example>` übereinstimmen.
          * Da alle Tests für eine spezifische Python-Version nacheinander in
            einem Container ausgeführt werden, gehen hierbei die Vorteile der
            parallelen Ausführung verloren.

   ``run: python -m tox``
       führt tox aus.

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
   * `Extending tox <https://tox.readthedocs.io/en/latest/plugins.html>`_
   * `tox development team <https://github.com/orgs/tox-dev/repositories>`_
