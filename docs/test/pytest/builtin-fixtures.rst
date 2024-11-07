Built-in Fixtures
=================

Die Wiederverwendung gemeinsamer Fixtures ist eine so gute Idee, dass pytest
einige häufig verwendete Fixtures integriert hat. Die eingebauten Fixtures,
helfen euch, einige sehr nützliche Dinge in euren Tests einfach und konsistent
zu tun. Unter anderem enthält pytest eingebaute Fixtures, die mit temporären
Verzeichnissen und Dateien umgehen, auf Kommandozeilenoptionen zugreifen,
zwischen Testsitzungen kommunizieren, Ausgabeströme validieren,
Umgebungsvariablen verändern und Warnungen abfragen können.

``tmp_path`` und ``tmp_path_factory``
-------------------------------------

Die Fixtures :ref:`tmp_path <pytest:tmp_path>` und `tmp_path_factory
<https://docs.pytest.org/en/latest/how-to/tmp_path.html#the-tmp-path-factory-fixture>`_
werden verwendet, um temporäre Verzeichnisse zu erstellen. Die Fixture
``tmp_path`` für den ``function``-Scope gibt eine :class:`pathlib.Path`-Instanz
zurück, die auf ein temporäres Verzeichnis verweist, das während des Tests und
etwas länger bestehen bleibt. Die ``tmp_path_factory`` für eine
``session``-Scope-Fixture gibt ein ``TempPathFactory``-Objekt zurück. Dieses
Objekt hat eine ``mktemp()``-Funktion, die ``Path``-Objekte zurückgibt. Mit
``mktemp()`` könnt ihr mehrere temporäre Verzeichnisse erstellen.

In :doc:`fixtures` haben wir die Standardbibliothek
``tempfile.TemporaryDirectory`` für unser ``db``-Fixture verwendet:

.. code-block:: python

    from pathlib import Path
    from tempfile import TemporaryDirectory


    @pytest.fixture(scope="session")
    def db():
        """ItemsDB object connected to a temporary database"""
        with TemporaryDirectory() as db_dir:
            db_path = Path(db_dir)
            db_ = items.ItemsDB(db_path)
            yield db_
            db_.close()

Lasst uns stattdessen eines der neuen Built-ins verwenden. Da unser
``db``-Fixture im ``session``-Scope liegt, können wir ``tmp_path`` nicht
verwenden, da ``session``-Scope-Fixtures keine ``function``-Scope-Fixtures
verwenden können. Wir können jedoch ``tmp_path_factory`` verwenden:

.. code-block:: python

    @pytest.fixture(scope="session")
    def db(tmp_path_factory):
        """ItemsDB object connected to a temporary database"""
        db_path = tmp_path_factory.mktemp("items_db")
        db_ = items.ItemsDB(db_path)
        yield db_
        db_.close()

.. note::
   Wir können dadurch auch zwei Import-Anweisungen entfernen, da wir weder
   ``pathlib`` noch ``tempfile`` importieren müssen.

.. tipp::
   Verwendet nicht :ref:`tmpdir <pytest:tmpdir>` oder :ref:`tmpdir_factory
   <pytest:tmpdir>`, da diese :class:`py.path.local`-Objekte bereitstellen, ein
   Legacy-Typ.

Das Basisverzeichnis für alle temporären pytest-Verzeichnisse ist system- und
anwendungsabhängig. Es enthält einen :samp:`pytest-{NUM}`-Teil, wobei
:samp:`{NUM}` bei jeder Sitzung erhöht wird. Das Basisverzeichnis wird
unmittelbar nach einer Sitzung unverändert belassen, damit ihr es im Falle von
Testfehlern untersuchen könnt. pytest räumt sie schließlich auf. Nur die letzten
paar temporären Basisverzeichnisse werden auf dem System belassen.

Ihr könnt auch euer eigenes Basisverzeichnis angeben mit :samp:`pytest
--basetemp={MYDIR}`.

.. _capsys-fixture:

``capsys``
----------

Manchmal soll der Anwendungscode etwas auf ``stdout``, ``stderr`` :abbr:`usw.
(und so weiter)` ausgeben. Das Items-Beispielprojekt hat deswegen auch eine
Kommandozeilenschnittstelle, die wir nun testen wollen.

Der Befehl ``items version`` soll die Version ausgeben:

.. code-block:: console

    $ items version
    0.1.0

Die Version ist auch via Python verfügbar:

.. code-block:: pycon

    >>> import items
    >>> items.__version__
    '0.1.0'

Eine Möglichkeit, dies zu testen, ist

#. den Befehl mit ``subprocess.run()`` auszuführen
#. die Ausgabe zu erfassen
#. sie mit der Version aus der API zu vergleichen

.. code-block:: python

    import subprocess

    import items


    def test_version():
        process = subprocess.run(
            ["items", "version"], capture_output=True, text=True
        )
        output = process.stdout.rstrip()
        assert output == items.__version__

Die Funktion ``rstrip()`` wird verwendet, um den Zeilenumbruch zu entfernen.

Das `capsys
<https://docs.pytest.org/en/latest/reference/reference.html#capsys>`_-Fixture
ermöglicht die Erfassung von Schreibvorgängen auf ``stdout`` und ``stderr``.
Wir können die Methode, die dies im :abbr:`CLI (Command Line Interface)`
implementiert, direkt aufrufen und ``capsys`` zum Lesen der Ausgabe verwenden:

.. code-block::

    import items


    def test_version(capsys):
        items.cli.version()
        output = capsys.readouterr().out.rstrip()
        assert output == items.__version__

Die Methode ``capsys.readouterr()`` gibt ein ``namedtuple`` zurück, das ``out``
und ``err`` enthält. Wir lesen nur den ``out``-Teil und entfernen dann den
Zeilenumbruch mit ``rstrip()``.

Eine weitere Funktion von ``capsys`` ist die Möglichkeit, die normale
Ausgabeerfassung von pytest vorübergehend zu deaktivieren. pytest erfasst
normalerweise die Ausgaben eurer Tests und des Anwendungscodes. Dies schließt
``print``-Anweisungen ein.

.. code-block:: python

    import items


    def test_stdout():
        version = items.__version__
        print("\nitems " + version)

Wenn wir den Test jedoch ausführen, sehen wir keine Ausgabe:

.. code-block:: pytest

    $ pytest tests/test_output.py
    ============================= test session starts ==============================
    …
    collected 1 item

    tests/test_output.py .                                                   [100%]

    ============================== 1 passed in 0.00s ===============================

pytest fängt die gesamte Ausgabe auf. Dies hilft zwar, die Kommandozeilensitzung
sauber zu halten, es kann jedoch vorkommen, dass wir die gesamte Ausgabe sehen
wollen, auch bei bestandenen Tests. Hierfür können die Option ``-s`` oder
``--capture=no`` verwenden:

.. code-block:: pytest
   :emphasize-lines: 7

    $ pytest -s tests/test_output.py
    ============================= test session starts ==============================
    …
    collected 1 item

    tests/test_output.py
    items 0.1.0
    .

    ============================== 1 passed in 0.00s ===============================

Eine andere Möglichkeit, die Ausgabe immer einzuschließen, ist
``capsys.disabled()``:

.. code-block:: python

    import items


    def test_stdout(capsys):
        with capsys.disabled():
            version = items.__version__
            print("\nitems " + version)

Nun wird sie Ausgabe im ``with``-Block immer angezeigt, auch ohne die
``-s``-Option:

.. code-block:: pytest

    $ pytest tests/test_output.py
    ============================= test session starts ==============================
    …
    collected 1 item

    tests/test_output.py
    items 0.1.0
    .                                                   [100%]

    ============================== 1 passed in 0.00s ===============================

.. seealso::

    ``capfd``
        Wie ``capsys``, erfasst aber die Dateideskriptoren 1 und 2, die
        normalerweise dasselbe wie ``stdout`` und ``stderr``
    ``capsysbinary``
        Während capsys Text erfasst, erfasst capsysbinary Bytes
    ``capfdbinary``
        erfasst Bytes in den Dateideskriptoren 1 und 2
    ``caplog``
        erfasst Ausgaben, die mit dem Logging-Paket geschrieben wurden

.. _monkeypatch-fixture:

``monkeypatch``
---------------

Mit ``capsys`` kann ich zwar gut die ``stdout`` und ``stderr``-Ausgabe steuern,
aber es ist immer noch nicht die Art, wie ich die :abbr:`CLI (Command Line
Interface)` testen möchte. Die Items-Anwendung verwendet eine Bibliothek namens `Typer <https://typer.tiangolo.com>`_, die eine Runner-Funktion enthält um
unserem Code so zu testen, wie wir es von einem Befehlszeilentest erwarten
würden, der im Prozess bleibt und uns mit Output-Hooks versorgt, :abbr:`z.B.
(zum Beispiel)`:

.. code-block:: python

    from typer.testing import CliRunner

    import items


    def test_version():
        runner = CliRunner()
        result = runner.invoke(items.app, ["version"])
        output = result.output.rstrip()
        assert output == items.__version__

Wir werden diese Methode der Ausgabentests als Ausgangspunkt für die restlichen
Tests der Items-CLI verwenden. Ich habe mit den CLI-Tests begonnen, indem ich
die Items-Version getestet habe. Um den Rest der CLI zu testen, müssen wir die
Datenbank in ein temporäres Verzeichnis umleiten, so wie wir es beim Testen der
API unter Verwendung von :ref:`Fixtures für Setup und Teardown
<setup-and-teardown-fixtures>` getan haben. Hierfür verwenden wir nun
`monkeypatch
<https://docs.pytest.org/en/latest/reference/reference.html#monkeypatch>`_:

Ein *Monkey Patch* ist eine dynamische Änderung einer Klasse oder eines Moduls
während der Laufzeit. Während des Testens ist *monkey patching* eine bequeme
Möglichkeit, einen Teil der Laufzeitumgebung des Anwendungscodes zu übernehmen
und entweder Eingabe- oder Ausgabeabhängigkeiten durch Objekte oder Funktionen
zu ersetzen, die für das Testen besser geeignet sind. Mit dem eingebauten
Fixture ``monkeypatch`` könnt ihr dies im Kontext eines einzelnen Tests tun. Es
wird verwendet, um Objekte, Dicts, Umgebungsvariablen, ``PYTHONPATH`` oder
das aktuelle Verzeichnis zu ändern. Es ist wie eine Mini-Version von
:doc:`Mocking <../mock>`. Und wenn der Test endet, wird unabhängig davon, ob er
bestanden wurde oder nicht, der ursprüngliche, ungepatchte Code
wiederhergestellt und alles rückgängig gemacht, was durch den Patch geändert
wurde.

.. seealso::
   `How to monkeypatch/mock modules and environments
   <https://docs.pytest.org/en/latest/how-to/monkeypatch.html>`_

Das ``monkeypatch``-Fixture bietet die folgenden Funktionen:

+-------------------------------------------------------+-----------------------+
| Funktion                                              | Beschreibung          |
+=======================================================+=======================+
| :samp:`setattr(TARGET, NAME, VALUE, raising=True)`    | setzt ein Attribut    |
| [1]_                                                  |                       |
+-------------------------------------------------------+-----------------------+
| :samp:`delattr(TARGET, NAME, raising=True)` [1]_      | löscht ein Attribut   |
+-------------------------------------------------------+-----------------------+
| :samp:`setitem(DICT, NAME, VALUE)`                    | setzt einen           |
|                                                       | Dict-Eintrag          |
+-------------------------------------------------------+-----------------------+
| :samp:`delitem(DICT, NAME, raising=True)` [1]_        | löscht einen          |
|                                                       | Dict-Eintrag          |
+-------------------------------------------------------+-----------------------+
| :samp:`setenv(NAME, VALUE, prepend=None)` [2]_        | setzt eine            |
|                                                       | Umgebungsvariable     |
+-------------------------------------------------------+-----------------------+
| :samp:`delenv(NAME, raising=True)` [1]_               | löscht eine           |
|                                                       | Umgebungsvariable     |
+-------------------------------------------------------+-----------------------+
| :samp:`syspath_prepend(PATH)`                         | erweitert den Pfad    |
|                                                       | ``sys.path``          |
+-------------------------------------------------------+-----------------------+
| :samp:`chdir(PATH)`                                   | wechselt das aktuelle |
|                                                       | Arbeitsverzeichnis    |
+-------------------------------------------------------+-----------------------+

.. [1] Der ``raising``-Parameter teilt pytest mit, ob eine Exception ausgelöst
       werden soll, wenn das Element (noch) nicht vorhanden ist.
.. [2] Der ``prepend``-Parameter von ``setenv()`` kann ein Zeichen sein. Wenn er
       gesetzt ist, wird der Wert der Umgebungsvariablen in :samp:`{VALUE} +
       prepend + {OLD_VALUE}` geändert.

Wir können ``monkeypatch`` verwenden, um die :abbr:`CLI (Command Line
Interface)` auf ein temporäres Verzeichnis für die Datenbank umzuleiten, und
zwar auf zweierlei Weise. Beide Methoden erfordern Kenntnisse über den
Anwendungscode. Schauen wir uns die Methode ``cli.get_path()`` in
:file:`src/items/cli.py` an:

.. code-block:: python

    import os
    import pathlib


    def get_path():
        db_path_env = os.getenv("ITEMS_DB_DIR", "")
        if db_path_env:
            db_path = pathlib.Path(db_path_env)
        else:
            db_path = pathlib.Path.home() / "items_db"
        return db_path

Diese Methode teilt dem restlichen CLI-Code mit, wo sich die Datenbank
befindet. Um uns den Speicherort der Datenbank auf der Kommandozeile ausgeben zu
lassen, definieren wir nun auch noch ``config()`` in :file:`src/items/cli.py`:

.. code-block:: python

    @app.command()
    def config():
        """Return the path to the Items db."""
        with items_db() as db:
            print(db.path())

.. code-block:: console

    $ items config
    /Users/veit/items_db

Um diese Methoden zu testen, können wir nun entweder die gesamte
``get_path()``-Funktion oder das ``pathlib.Path()``-Attribut ``home`` patchen.
Hierfür definieren wir in :file:`tests/test_config.py` zunächst eine
Hilfsfunktion ``run_items_cli``, die dasselbe ausgibt wie ``items`` auf der
Kommandozeile:

.. code-block:: python

    from typer.testing import CliRunner

    import items


    def run_items_cli(*params):
        runner = CliRunner()
        result = runner.invoke(items.app, params)
        return result.output.rstrip()

Anschließend können wir dann unseren Test schreiben, der die gesamte
``get_path()``-Funktion patcht:

.. code-block:: python

    def test_get_path(monkeypatch, tmp_path):
        def fake_get_path():
            return tmp_path

        monkeypatch.setattr(items.cli, "get_path", fake_get_path)
        assert run_items_cli("config") == str(tmp_path)

Die Funktion ``get_path()`` aus ``items.cli`` kann nicht einfach durch
``tmp_path`` ersetzt werden, da dies ein ``pathlib.Path``-Objekt ist, das nicht
aufrufbar ist. Daher wird sie durch die ``fake_get_path()``-Funktion ersetzt.
Alternativ können wir jedoch auch das ``home``-Attribut von ``pathlib.Path``
patchen:

.. code-block:: python

    def test_home(monkeypatch, tmp_path):
        items_dir = tmp_path / "items_db"

        def fake_home():
            return tmp_path

        monkeypatch.setattr(items.cli.pathlib.Path, "home", fake_home)
        assert run_items_cli("config") == str(items_dir)

*Monkey patching* und *Mocking* verkomplizieren jedoch das Testen, sodass wir
nach Möglichkeiten suchen werden, dies zu vermeiden, wann immer es möglich ist.
In unserem Fall könnte sinnvoll sein, eine Umgebungsvariable
:envvar:`ITEMS_DB_DIR` zu setzen, die einfach gepatcht werden kann:

.. code-block:: python

    def test_env_var(monkeypatch, tmp_path):
        monkeypatch.setenv("ITEMS_DB_DIR", str(tmp_path))
        assert run_items_cli("config") == str(tmp_path)

Verbleibende Built-in-Fixtures
------------------------------

+-------------------------------+-----------------------------------------------+
| Built-in-Fixture              | Beschreibung                                  |
+===============================+===============================================+
| ``capfd``,                    | Varianten von ``capsys``, die mit             |
| ``capfdbinary``,              | Dateideskriptoren und/oder binärer Ausgabe    |
| ``capsysbinary``              | arbeiten.                                     |
+-------------------------------+-----------------------------------------------+
| ``caplog``                    | ähnlich wie ``capsys``; wird für Meldungen    |
|                               | verwendet, die mit Pythons Logging-System     |
|                               | erstellt werden.                              |
+-------------------------------+-----------------------------------------------+
| ``cache``                     | wird zum Speichern und Abrufen von Werten     |
|                               | über mehrere Pytest-Läufe hinweg verwendet.   |
|                               |                                               |
|                               | Es erlaubt ``last-failed``, ``failed-first``  |
|                               | und ähnliche Optionen.                        |
+-------------------------------+-----------------------------------------------+
| ``doctest_namespace``         | nützlich, wenn ihr pytest verwenden möchtet,  |
|                               | um :doc:`Doctests                             |
|                               | <../../document/doctest>`                     |
|                               | durchzuführen.                                |
+-------------------------------+-----------------------------------------------+
| ``pytestconfig``              | wird verwendet, um Zugriff auf                |
|                               | Konfigurationswerte, Plugin-Manager und       |
|                               | -Hooks zu erhalten.                           |
+-------------------------------+-----------------------------------------------+
| ``record_property``,          | wird verwendet, um dem Test oder der          |
| ``record_testsuite_property`` | Testsuite zusätzliche Eigenschaften           |
|                               | hinzuzufügen.                                 |
|                               |                                               |
|                               | Besonders nützlich für das Hinzufügen von     |
|                               | Daten zu einem Bericht, der von :abbr:`CI     |
|                               | (Continuous Integration)`-Tools verwendet     |
|                               | wird.                                         |
+-------------------------------+-----------------------------------------------+
| ``recwarn``                   | wird verwendet, um Warnmeldungen zu testen.   |
|                               |                                               |
+-------------------------------+-----------------------------------------------+
| ``request``                   | wird verwendet, um Informationen über die     |
|                               | ausgeführte Testfunktion bereitzustellen.     |
|                               |                                               |
|                               | wird meist bei der Parametrisierung von       |
|                               | Fixtures verwendet                            |
+-------------------------------+-----------------------------------------------+
| ``pytester``, ``testdir``     | Wird verwendet, um ein temporäres             |
|                               | Testverzeichnis bereitzustellen, um die       |
|                               | Ausführung und das Testen von pytest-Plugins  |
|                               | zu unterstützen. ``pytester`` ist der         |
|                               | ``pathlib``-basierte Ersatz für das           |
|                               | ``py.path``-basierte ``testdir``.             |
+-------------------------------+-----------------------------------------------+
| ``tmpdir``,                   | ähnlich wie ``tmp_path`` und                  |
| ``tmpdir_factory``            | ``tmp_path_factory``; dient der Rückgabe      |
|                               | eines ``py.path.local``-Objekts anstelle      |
|                               | eines ``pathlib.Path``-Objekts.               |
+-------------------------------+-----------------------------------------------+

Ihr könnt die vollständige Liste der Built-in-Fixtures erhalten, indem ihr
``pytest --fixtures`` ausführt.

.. seealso::
   * `Built-in fixtures
     <https://docs.pytest.org/en/latest/reference/fixtures.html#built-in-fixtures>`_
