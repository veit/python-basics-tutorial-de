Beispiele
=========

Mocking von ``datetime.datetime``
---------------------------------

Zunächst wollten wir mit einem einfachen Beispiel starten und überprüfen, ob die
Arbeitstage von Montag bis Freitag korrekt ermittelt werden.

#. Zunächst importieren wir :class:`datetime.datetime` und :class:`Mock
   <python3:unittest.mock.Mock>`:

   .. literalinclude:: test_mock.py
      :language: python
      :lines: 1-2
      :lineno-start: 1

#. Dann definieren wir zwei Testtage:

   .. literalinclude:: test_mock.py
      :language: python
      :lines: 5-6
      :lineno-start: 5

#. Nun definieren wir eine Methode zur Überprüfung der Arbeitstage, wobei die
   datetime-Bibliothek von Python Montage als ``0``  und Sonntage als ``6``
   behandelt:

   .. literalinclude:: test_mock.py
      :language: python
      :lines: 9-11
      :lineno-start: 9

#. Dann mocken wir datetime:

   .. literalinclude:: test_mock.py
      :language: python
      :lines: 14
      :lineno-start: 14

#. Schließlich testen wir unsere beiden Mock-Objekte:

   .. literalinclude:: test_mock.py
      :language: python
      :lines: 17-21
      :lineno-start: 17

   .. literalinclude:: test_mock.py
      :language: python
      :lines: 24-
      :lineno-start: 24

Mocking der CLI
---------------

Für die Tests der Tasks-CLI werden wir uns auch ansehen, wie der von `Typer
<https://typer.tiangolo.com>`_ bereitgestellte ``CliRunner`` beim Testen hilft.
Typer bietet eine Testschnittstelle, womit wir unsere Anwendung aufrufen können,
ohne, wie in dem kurzen :ref:`capsys-fixture`-Beispiel auf
:func:`python3:subprocess.run` zurückgreifen zu müssen. Das ist gut, weil wir
nicht simulieren können, was in einem separaten Prozess läuft. So können wir in
:file:`tests/cli/conftest.py` der :func:`invoke`-Funktion unseres ``runner`` nur
unsere Anwendung ``cusy.tasks.cli.app`` und eine Liste von Strings übergeben,
die den Befehl darstellt: genauer wandeln wir mit
:func:`shlex.split(command_string)` die Befehle, :abbr:`z. B. (zum Beispiel)`
:samp:`list -o "veit"` in :samp:`["list", "-o", "veit"]` um und können die
Ausgabe dann abfangen und zurückgeben.

.. code-block:: python
   :emphasize-lines: 4, 8, 16-17

    import shlex

    import pytest
    from typer.testing import CliRunner

    from cusy import tasks

    runner = CliRunner()


    @pytest.fixture()
    def tasks_cli(db_path, monkeypatch, tasks_db):
        monkeypatch.setenv("ITEMS_DB_DIR", db_path.as_posix())

        def run_cli(command_string):
            command_list = shlex.split(command_string)
            result = runner.invoke(tasks.cli.app, command_list)
            output = result.stdout.rstrip()
            return output

        return run_cli

Anschließend können wir diese Fixture einfach verwenden um :abbr:`z.B. (zum
Beispiel)` die Version in :file:`tests/cli/test_version.py` zu testen:

.. code-block:: python

    from cusy import tasks


    def test_version(tasks_cli):
        assert tasks_cli("version") == tasks.__version__

.. seealso::
   `Typer Learn Testing <https://typer.tiangolo.com/tutorial/testing/>`_

Mocking von Attributen
----------------------

Schauen wir uns an, wie wir Mocking verwenden können, um sicherzustellen, dass
:abbr:`z. B. (zum Beispiel)` auch dreistellige Versionsnummern von
:func:`tasks.__version__` korrekt über die CLI ausgegeben werden. Hierfür werden
wir :func:`mock.patch.object` als Kontextmanager verwenden:

.. code-block:: python
   :emphasize-lines: 1, 7

    from unittest import mock

    from cusy import tasks


    def test_mock_version(tasks_cli):
        with mock.patch.object(tasks, "__version__", "100.0.0"):
            assert tasks_cli("version") == tasks.__version__

In unserem Testcode importieren wir ``tasks``. Das resultierende tasks-Objekt
ist das, was wir patchen werden. Der Aufruf von :func:`mock.patch.object`, der
als :doc:`Kontextmanager <../../control-flow/with>` innerhalb eines
``with``-Blocks verwendet wird, gibt ein Mock-Objekt zurück, das nach dem
``with``-Block aufgeräumt wird:

#. In diesem Fall wird das Attribut ``__version__`` von ``tasks`` für die Dauer
   des ``with``-Blocks durch ``"100.0.0"`` ersetzt.
#. Anschließend verwenden wir :func:`tasks_cli`, um unsere CLI-Anwendung mit dem
   Befehl ``"version"`` aufzurufen. Wenn die Methode :func:`version` aufgerufen
   wird, ist das Attribut ``__version__`` jedoch nicht der ursprüngliche String,
   sondern der String, den wir mit :func:`mock.patch.object` ersetzt haben.

Mocking von Klassen und Methoden
--------------------------------

In :file:`src/cusy/tasks/cli.py` haben wir :func:`config` folgendermaßen
definiert:

.. code-block:: python

    def config():
        """List the path to the Tasks db."""
        with tasks_db() as db:
            print(db.path())

:func:`tasks_db` ist ein :doc:`Kontextmanager <../../control-flow/with>`, der
ein ``tasks.TasksDB``-Objekt zurückgibt. Das zurückgegebene Objekt wird dann als
``db`` verwendet, um :func:`db.path` aufzurufen. Wir sollten hier also zwei
Dinge zu mocken: ``tasks.TasksDB`` und eine seiner Methoden, :func:`path`.
Beginnen wir mit der Klasse:

.. code-block:: python

    from unittest import mock

    from cusy import tasks


    def test_mock_tasksdb(tasks_cli):
        with mock.patch.object(tasks, "TasksDB") as MockTasksDB:
            mock_db_path = MockTasksDB.return_value.path.return_value = "/foo/"
            assert tasks_cli("config") == str(mock_db_path)

Lasst und sicherstellen, dass es wirklich funktioniert:

.. code-block:: pytest

    $ uv run pytest -v -s tests/cli/test_config.py::test_mock_tasksdb
    ============================= test session starts ==============================
    ...
    configfile: pyproject.toml
    plugins: cov-4.1.0, Faker-19.11.0
    collected 1 item

    tests/cli/test_config.py::test_mock_tasksdb PASSED

    ============================== 1 passed in 0.04s ===============================

Prima, nun müssen wir nur noch den Mock für die Datenbank in eine Fixture
verschieben, denn wir werden ihn in vielen Testmethoden brauchen:

.. code-block:: python

    @pytest.fixture()
    def mock_tasksdb():
        with mock.patch.object(tasks, "TasksDB") as MockTasksDB:
            yield MockTasksDB.return_value

Diese Fixture mockt das ``TasksDB``-Objekt und gibt den ``return_value`` zurück,
so dass Tests ihn verwenden können, um Dinge wie ``path`` zu ersetzen:

.. code-block:: python

    def test_mock_tasksdb(tasks_cli, mock_tasksdb):
        mock_tasksdb.path.return_value = "/foo/"
        result = runner.invoke(app, ["config"])
        assert result.stdout.rstrip() == "/foo/"

Alternativ kann zum Mocken von Klassen oder Objekten auch der
:func:`@mock.patch`-:doc:`Dekorator <../../functions/decorators>` verwendet
werden. In den folgenden Beispielen wird die Ausgabe von ``os.listdir`` gemockt.
Dazu muss ``db_path`` nicht im Dateisystem vorhanden sein:

.. code-block:: python

    import os
    from unittest import mock


    @mock.patch("os.listdir", mock.MagicMock(return_value="db_path"))
    def test_listdir():
        assert "db_path" == os.listdir()

Eine weitere Alternative ist, den Rückgabewert separat zu definieren:

.. code-block:: python

    @mock.patch("os.listdir")
    def test_listdir(mock_listdir):
        mock_listdir.return_value = "db_path"
        assert "db_path" == os.listdir()
