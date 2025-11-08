Befehlszeilenoptionen
=====================

In :ref:`dynamic-fixture-scope` haben wir bereits gesehen, wie der Fixture-Scope
durch eine Befehlszeilenoption geändert werden kann. Nun wollen wir uns die
Befehlszeilenoptionen genauer anschauen.

Unterschiedliche Werte an eine Testfunktion übergeben
-----------------------------------------------------

Angenommen, ihr möchtet einen Test schreiben, der von einer Befehlszeilenoption
abhängt, könnt ihr dies mit folgendem Muster erreichen:

.. code-block:: python
   :caption: test_example.py

   def test_db(items_db, db_path, cmdopt):
       if cmdopt == "json":
           print("Save as JSON file")
       elif cmdopt == "sqlite":
           print("Save in a SQLite database")
       assert items_db.path() == db_path

Damit dies funktioniert, muss die Befehlszeilenoption hinzugefügt und ``cmdopt``
über eine Fixture-Funktion bereitgestellt werden:

.. code-block:: python
   :caption: conftest.py

   import pytest


   def pytest_addoption(parser):
       parser.addoption(
           "--cmdopt",
           action="store",
           default="json",
           help="Store data as JSON file or in a SQLite database",
       )


   @pytest.fixture
   def cmdopt(request):
       return request.config.getoption("--cmdopt")

Anschließend könnt ihr eure Tests :abbr:`z. B. (zum Beispiel)` aufrufen mit:

.. code-block:: console

   $ pytest --sqlite

Darüberhinaus könnt ihr eine einfache Validierung der Eingabe hinzufügen, indem
ihr die Auswahlmöglichkeiten auflistet:

.. code-block:: python
   :caption: conftest.py
   :emphasize-lines: 7

   def pytest_addoption(parser):
       parser.addoption(
           "--cmdopt",
           action="store",
           default="json",
           help="Store data as JSON file or in a SQLite database",
           choices=("json", "sqlite"),
       )

So bekommen wir Feedback zu einem falschen Argument:

.. code-block:: console

   $ pytest --postgresql
   ERROR: usage: pytest [options] [file_or_dir] [file_or_dir] [...]
   pytest: error: argument --cmdopt: invalid choice: 'postgresql' (choose from json, sqlite)

Wenn ihr detailliertere Fehlermeldungen bereitstellen wollt, könnt ihr den
``type``-Parameter verwenden und ``pytest.UsageError`` auslösen:

.. code-block:: python
   :caption: conftest.py
   :emphasize-lines: -6, 15

   def type_checker(value):
       msg = "cmdopt must specify json or sqlite"
       if not value.startswith("json" or "sqlite"):
           raise pytest.UsageError(msg)

       return value


   def pytest_addoption(parser):
       parser.addoption(
           "--cmdopt",
           action="store",
           default="json",
           help="Store data as JSON file or in a SQLite database",
           type=type_checker,
       )

Häufig sollten jedoch Befehlszeilenoptionen außerhalb des Tests verarbeitet und
komplexere Objekte übergeben werden.

Befehlszeilenoptionen dynamisch hinzufügen
------------------------------------------

Mit :ref:`addopts` könnt ihr für euer Projekt statisch Befehlszeilenoptionen
hinzufügen. Ihr könnt jedoch auch die Befehlszeilenargumente dynamisch ändern,
bevor sie verarbeitet werden:

.. code-block:: python
   :caption: conftest.py

   import sys


   def pytest_load_initial_conftests(args):
       if "xdist" in sys.modules:
           import multiprocessing

           num = max(multiprocessing.cpu_count() / 2, 1)
           args[:] = ["-n", str(num)] + args

Wenn ihr das :ref:`xdist-plugin`-Plugin installiert habt, werden die Testläufe
immer mit einer Anzahl von Unterprozessen durchgeführt, die eurer CPU nahekommt.

Befehlszeilenoption für das Überspringen von Tests
--------------------------------------------------

Im Folgenden fügen wir eine :file:`conftest.py`-Datei mit einer
Befehlszeilenoption ``--runslow`` hinzu, um das Überspringen von mit
``pytest.mark.slow`` gekennzeichneten Tests zu steuern:

.. code-block:: python
   :caption: conftest.py

   import pytest


   def pytest_addoption(parser):
       parser.addoption(
           "--runslow", action="store_true", default=False, help="run slow tests"
       )


   def pytest_collection_modifyitems(config, items):
       if config.getoption("--runslow"):
           # If --runslow is specified on the CLI, slow tests are not skipped.
           return
       skip_slow = pytest.mark.skip(reason="need --runslow option to run")
       for item in items:
           if "slow" in item.keywords:
               item.add_marker(skip_slow)

Falls wir nun einen Test schreiben mit dem ``@pytest.mark.slow``-Dekorator, wird
beim Aufruf von pytest ein übersprungener „langsamer“ Test angezeigt:

.. code-block:: pytest

   $ uv run pytest
   ============================= test session starts ==============================
   ...
   test_example.py s.                                                         [100%]

   =========================== short test summary info ============================
   SKIPPED [1] test_example.py:8: need --runslow option to run
   ========================= 1 passed, 1 skipped in 0.05s =========================

Testbericht-Header erweitern
----------------------------

Zusätzliche Informationen können einfach in einem ``pytest -v``-Lauf
bereitgestellt werden:

.. code-block:: python
   :caption: conftest.py

   import sys


   def pytest_report_header(config):
       gil = sys._is_gil_enabled()
       return f"Is GIL enabled? {gil}"


.. code-block:: pytest
   :emphasize-lines: 5

   $ uv run pytest -v
   ============================= test session starts ==============================
   platform darwin -- Python 3.14.0b4, pytest-8.4.1, pluggy-1.6.0
   cachedir: .pytest_cache
   Is GIL enabled? False
   rootdir: /Users/veit/sandbox/items
   configfile: pyproject.toml
   plugins: anyio-4.9.0, Faker-37.4.0, cov-6.2.1
   ...
   ============================== 2 passed in 0.04s ===============================

Testdauer ermitteln
-------------------

Wenn ihr eine große Testsuite habt, die langsam läuft, möchtet ihr vermutlich
mit ``-vv --durations`` herausfinden, welche Tests am langsamsten sind.

.. code-block:: pytest

   $ uv run pytest -vv --durations=3
   ============================= test session starts ==============================
   ...
   ============================= slowest 3 durations ==============================
   0.02s setup    tests/api/test_add.py::test_add_from_empty
   0.00s call     tests/cli/test_help.py::test_help[add]
   0.00s call     tests/cli/test_help.py::test_help[update]
   ============================== 83 passed in 0.17s ==============================
