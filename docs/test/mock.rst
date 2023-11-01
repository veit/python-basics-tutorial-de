Mock
====

In diesem Kapitel werden wir die :abbr:`CLI (Befehlszeilenschnittstelle)`
testen. Hierfür werden wir das :doc:`mock <python3:library/unittest.mock>`-Paket
verwenden, das seit Python 3.3 als Teil der Python-Standardbibliothek unter dem
Namen ``unittest.mock`` ausgeliefert wird. Für ältere Versionen von Python könnt
ihr sie installieren mit:

.. tab:: Linux/macOS

   .. code-block:: console

      $ . bin/activate
      $ python -m pip install mock

.. tab:: Windows

   .. code-block:: ps1con

      C:> Scripts\activate.bat
      C:> python -m pip install mock

:term:`Mock`-Objekte werden manchmal auch als Test-Doubles, :term:`Fakes <Fake>`
oder :term:`Stubs` bezeichnet. Mit dem pytest-eigenen
:ref:`monkeypatch-fixture`-Fixture und mock solltet ihr über alle Funktionen
verfügen, die ihr benötigt.

Beispiel
--------

Zunächst wollten wir mit einem einfachen Beispiel starten und überprüfen, ob die
Arbeitstage von Montag bis Freitag korrekt ermittelt werden.

Zunächst importieren wir ``datetime.datetime`` und ``Mock``:

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
      :lines: 17-19
      :lineno-start: 17

   .. literalinclude:: test_mock.py
      :language: python
      :lines: 21-23
      :lineno-start: 21

Testen mit Typer
----------------

Für die Tests der Items-CLI werden wir uns auch ansehen, wie der von `Typer
<https://typer.tiangolo.com>`_ bereitgestellte ``CliRunner`` beim Testen hilft.
Typer bietet eine Testschnittstelle, womit wir unsere Anwendung aufrufen können,
ohne, wie in dem kurzen :ref:`capsys-fixture`-Beispiel auf
:func:`python3:subprocess.run` zurückgreifen zu müssen. Das ist gut, weil wir
nicht simulieren können, was in einem separaten Prozess läuft. So können wir in
:file:`tests/cli/conftest.py` der :func:`invoke`-Funktion unseres ``runner`` nur
unsere Anwendung ``items.cli.app`` und eine Liste von Strings übergeben, die den
Befehl darstellt: genauer wandeln wir mit :func:`shlex.split(command_string)`
die Befehle, :abbr:`z.B. (zum Beispiel)` :samp:`list -o veit" in ["list", "-o", "veit"]` um und können die Ausgabe dann abfangen und zurückgeben.

.. code-block:: python
   :emphasize-lines: 4, 8, 16-17

    import shlex

    import pytest
    from typer.testing import CliRunner

    import items

    runner = CliRunner()


    @pytest.fixture()
    def items_cli(db_path, monkeypatch, items_db):
        monkeypatch.setenv("ITEMS_DB_DIR", db_path.as_posix())

        def run_cli(command_string):
            command_list = shlex.split(command_string)
            result = runner.invoke(items.cli.app, command_list)
            output = result.stdout.rstrip()
            return output

        return run_cli

Anschließend können wir diese Fixture einfach verwenden um :abbr:`z.B. (zum
Beispiel)` die Version in :file:`tests/cli/test_version.py` zu testen:

.. code-block:: python

    import items


    def test_version(items_cli):
        assert items_cli("version") == items.__version__

Mocking von Attributen
----------------------

Schauen wir uns an, wie wir Mocking verwenden können, um sicherzustellen, dass
:abbr:`z.B. (zum Beispiel)` auch dreistellige Versionsnummern von
:func:`items.__version__` korrekt über die CLI ausgegeben werden. Hierfür werden
wir :func:`mock.patch.object` als Kontextmanager verwenden:

.. code-block:: python
   :emphasize-lines: 1, 7

    from unittest import mock

    import items


    def test_mock_version(items_cli):
        with mock.patch.object(items, "__version__", "100.0.0"):
            assert items_cli("version") == items.__version__

In unserem Testcode importieren wir ``items``. Das resultierende items-Objekt
ist das, was wir patchen werden. Der Aufruf von :func:`mock.patch.object()`, der
als :doc:`Kontextmanager <../control-flows/with>` innerhalb eines
``with``-Blocks verwendet wird, gibt ein Mock-Objekt zurück, das nach dem
``with``-Block aufgeräumt wird:

#. In diesem Fall wird das Attribut ``__version__`` von ``items`` für die Dauer
   des ``with``-Blocks durch ``"100.0.0"`` ersetzt.
#. Anschließend verwenden wir :func:`items_cli`, um unsere CLI-Anwendung mit dem
   Befehl ``"version"`` aufzurufen. Wenn die Methode :func:`version()`
   aufgerufen wird, ist das Attribut ``__version__`` jedoch nicht der
   ursprüngliche String, sondern der String, den wir mit
   :func:`mock.patch.object()` ersetzt haben.

Mocking von Klassen und Methoden
--------------------------------

In :file:`src/items/cli.py` haben wir :func:`config()` folgendermaßen definiert:

.. code-block:: python

    def config():
        """List the path to the Items db."""
        with items_db() as db:
            print(db.path())

:func:`items_db()` ist ein :doc:`Kontextmanager <../control-flows/with>`, der
ein ``items.ItemsDB``-Objekt zurückgibt. Das zurückgegebene Objekt wird dann als
``db`` verwendet, um :func:`db.path()` aufzurufen. Wir sollten hier also zwei
Dinge zu mocken: ``items.ItemsDB`` und eine seiner Methoden, :func:`path()`.
Beginnen wir mit der Klasse:

.. code-block:: python

    from unittest import mock

    import items


    def test_mock_itemsdb(items_cli):
        with mock.patch.object(items, "ItemsDB") as MockItemsDB:
            mock_db_path = MockItemsDB.return_value.path.return_value = "/foo/"
            assert items_cli("config") == str(mock_db_path)

Lasst und sicherstellen, dass es wirklich funktioniert:

.. code-block:: pytest

    $ pytest -v -s tests/cli/test_config.py::test_mock_itemsdb
    ============================= test session starts ==============================
    ...
    configfile: pyproject.toml
    plugins: cov-4.1.0, Faker-19.11.0
    collected 1 item

    tests/cli/test_config.py::test_mock_itemsdb PASSED

    ============================== 1 passed in 0.04s ===============================

Prima, nun müssen wir nur noch den Mock für die Datenbank in eine Fixture
verschieben, denn wir werden ihn in vielen Testmethoden brauchen:

.. code-block:: python

    @pytest.fixture()
    def mock_itemsdb():
        with mock.patch.object(items"ItemsDB") as MockItemsDB:
            yield MockItemsDB.return_value

Diese Fixture mockt das ``ItemsDB``-Objekt und gibt den ``return_value`` zurück,
so dass Tests ihn verwenden können, um Dinge wie ``path`` zu ersetzen:

.. code-block:: python

    def test_mock_itemsdb(items_cli, mock_itemsdb):
        mock_itemsdb.path.return_value = "/foo/"
        result = runner.invoke(app, ["config"])
        assert result.stdout.rstrip() == "/foo/"

Alternativ kann zum Mocken von Klassen oder Objekten auch der
:func:`@mock.patch`-Dekorator verwendet werden. In den folgenden Beispielen wird
die Ausgabe von ``os.listdir`` gemockt. Dazu muss ``db_path`` nicht im
Dateisystem vorhanden sein:

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

Mocks synchronisieren mit ``autospec``
--------------------------------------

Mock-Objekte sind in der Regel als Objekte gedacht, die anstelle der echten
Implementierung verwendet werden. Standardmäßig werden sie jedoch jeden Zugriff
akzeptieren. Wenn das echte Objekt beispielsweise :func:`.start(index)` zulässt,
sollen unsere Mock-Objekte ebenfalls :func:`.start(index)` zulassen. Dabei gibt
es jedoch ein Problem. Mock-Objekte sind standardmäßig zu flexibel: sie
würden auch :func:`stort()` oder andere falsch geschriebene, umbenannte oder
gelöschte Methoden oder Parameter akzeptieren. Dabei kann es im Laufe der Zeit
zum :abbr:`sog. (sogenannten)` Mock-Drift kommen, wenn sich die Schnittstelle,
die ihr nachbildet, ändert, euer Mock in eurem Testcode jedoch nicht. Diese Form
des Mock-Drifts kann durch das Hinzufügen von ``autospec=True`` zum Mock während
der Erstellung gelöst werden:

.. code-block:: python
   :emphasize-lines: 3

    @pytest.fixture()
    def mock_itemsdb():
        with mock.patch.object(items"ItemsDB", autospec=True) as MockItemsDB:
            yield MockItemsDB.return_value

Üblicherweise wird dieser Schutz mit ``autospec`` immer eingebaut. Die einzige
mir bekannte Ausnahme ist, wenn die Klasse oder das Objekt, das gemockt wird,
dynamische Methoden hat oder wenn Attribute zur Laufzeit hinzugefügt werden.

.. seealso::
   Die Python-Dokumentation hat einen großen Abschnitt über ``autospec``:
   :ref:`python3:auto-speccing`.

Aufruf überprüfen mit :func:`assert_called_with()`
--------------------------------------------------

Bisher haben wir die Rückgabewerte einer Mocking-Methode verwendet, um
sicherzustellen, dass unser Anwendungscode mit den Rückgabewerten richtig
umgeht. Aber manchmal gibt es keinen nützlichen Rückgabewert, :abbr:`z.B. (zum
Beispiel)` bei :samp:`items add some tasks -o veit`. In diesen Fällen
können wir das Mock-Objekt fragen, ob es korrekt aufgerufen wurde. Nach dem
Aufruf von :func:`items_cli("add some tasks -o veit")` wird nicht die API
verwendet, um zu prüfen, ob das Element in die Datenbank gelangt ist, sondern
ein Mock, um sicherzustellen, dass die CLI die richtige API-Methode korrekt
aufgerufen hat. Die Implementierung des Befehls :func:`add` ruft schließlich
:func:`db.add_item()` mit einem ``Item``-Objekt auf:

.. _test_add_with_owner:

.. code-block:: python
   :emphasize-lines: 4

    def test_add_with_owner(mock_itemsdb, items_cli):
        items_cli("add some task -o veit")
        expected = items.Item("some task", owner="veit", state="todo")
        mock_itemsdb.add_item.assert_called_with(expected)

Wenn :func:`add_item()` nicht aufgerufen wird oder mit dem falschen Typ oder dem
falschen Objektinhalt aufgerufen wird, schlägt der Test fehl. Wenn wir
:abbr:`z.B. (zum Beispiel)` in ``expected`` den String ``"Veit"`` groß
schreiben, aber nicht im CLI-Aufruf, erhalten wir folgende Ausgabe:

.. code-block:: pytest
   :emphasize-lines: 10-13, 16

    $ pytest -s tests/cli/test_add.py::test_add_with_owner
    ============================= test session starts ==============================
    ...
    configfile: pyproject.toml
    plugins: cov-4.1.0, Faker-19.11.0
    collected 1 item

    tests/cli/test_add.py F
    ...
    >           raise AssertionError(_error_message()) from cause
    E           AssertionError: expected call not found.
    E           Expected: add_item(Item(summary='some task', owner='Veit', state='todo', id=None))
    E           Actual: add_item(Item(summary='some task', owner='veit', state='todo', id=None))
    ...
    =========================== short test summary info ============================
    FAILED tests/cli/test_add.py::test_add_with_owner - AssertionError: expected call not found.
    ============================== 1 failed in 0.08s ===============================

.. seealso::
   Es gibt eine ganze Reihe von Varianten von :func:`assert_called()`. Eine
   vollständige Liste und Beschreibung erhaltet ihr in
   `unittest.mock.Mock.assert_called
   <https://docs.python.org/3/library/unittest.mock.html#unittest.mock.Mock.assert_called>`_.

   Wenn die einzige Möglichkeit zum Testen darin besteht, den korrekten Aufruf
   sicherzustellen, erfüllen die verschiedenen :func:`assert_called*`-Methoden
   ihren Zweck.

Fehlerbedingungen erstellen
---------------------------

Lasst uns nun überprüfen, ob die Items-CLI Fehlerbedingungen korrekt behandelt. Hier ist :abbr:`z.B. (zum Beispiel)` die Implementierung des Löschbefehls:

.. code-block:: python

    @app.command()
    def delete(item_id: int):
        """Remove item in db with given id."""
        with items_db() as db:
            try:
                db.delete_item(item_id)
            except items.InvalidItemId:
                print(f"Error: Invalid item id {item_id}")

Um zu testen, wie die CLI mit einer Fehlerbedingung umgeht, können wir so tun,
als ob :func:`delete_item` eine Exception erzeugt, indem wir dem Mock-Objekt die
Exception dem Attribut `side_effect
<https://docs.python.org/3/library/unittest.mock.html#unittest.mock.Mock.side_effect>`_
des Mock-Objekts zuweisen, etwa so:

.. code-block:: python

    def test_delete_invalid(mock_itemsdb, items_cli):
        mock_itemsdb.delete_item.side_effect = items.api.InvalidItemId
        out = items_cli("delete 42")
        assert "Error: Invalid item id 42" in out

Das ist alles, was wir brauchen, um die CLI zu testen: Mocking von
Rückgabewerten, Überprüfen der Aufrufe von Mock-Funktionen und das Mocking von
Exceptions. Es gibt jedoch noch eine ganze Reihe weiterer Mocking-Techniken, die
wir nicht behandelt haben. Lest also unbedingt
:doc:`python3:library/unittest.mock`, wenn ihr Mocking ausgiebig nutzen möchtet.

Grenzen des Mocking
-------------------

Eines der größten Probleme bei der Verwendung von Mocks besteht darin, dass wir
bei in einem Test nicht mehr das Verhalten, sondern die Implementierung testen.
Dies ist jedoch nicht nur zeitaufwändig, sondern auch gefährlich: Ein gültiges
Refactoring :abbr:`z.B. (zum Beispiel)` das Ändern eines Variablennamens, kann
Tests zum Scheitern bringen, wenn diese bestimmte Variable gemockt wurde. Wir
wollen jedoch, dass unsere Tests nur dann fehlschlagen, wenn es Brüche im
Verhalten gibt, nicht jedoch nur bei Codeänderungen.

Manchmal ist Mocking jedoch der einfachste Weg, Exceptions oder
Fehlerbedingungen zu erzeugen und sicherzustellen, dass euer Code diese korrekt
behandelt. Es gibt auch Fälle, in denen das Testen von Verhalten unzumutbar ist,
wie :abbr:`z.B. (zum Beispiel)` beim Zugriff auf eine Zahlungs-API oder beim
Senden von E-Mails. In diesen Fällen ist es eine gute Option zu testen, ob euer
Code eine bestimmte API-Methode zum richtigen Zeitpunkt und mit den richtigen
Parametern aufruft.

.. seealso::
   * Hynek Schlawack: `“Don’t Mock What You Don’t Own”
     <https://hynek.me/articles/what-to-mock-in-5-mins/>`_

Mocking vermeiden mit Tests auf mehreren Ebenen
-----------------------------------------------

Wir können die Items-CLI auch ohne Mocks testen indem wir auch die API
verwenden. Dabei werden wir nicht die API testen, sondern sie nur verwenden, um
das Verhalten von Aktionen zu überprüfen, die über die CLI ausgeführt werden.
Das Beispiel :ref:`test_add_with_owner <test_add_with_owner>` können wir auch
folgendermaßen testen:

.. code-block:: python

    def test_add_with_owner(items_db, items_cli):
        items_cli("add some task -o veit")
        expected = items.Item("some task", owner="veit", state="todo")
        all = items_db.list_items()
        assert len(all) == 1
        assert all[0] == expected

Mocking testet die Implementierung der Befehlszeilenschnittstelle und stellt
sicher, dass ein API-Aufruf mit bestimmten Parametern erfolgt. Beim
Mixed-Layer-Ansatz wird das Verhalten getestet, um sicherzustellen, dass das
Ergebnis unseren Vorstellungen entspricht. Diese Ansatz ist viel weniger
ein Change-Detector und hat eine größere Chance, während eines Refactorings
gültig zu bleiben. Interessanterweise sind die Tests auch etwa doppelt so
schnell:

.. code-block:: pytest

    $ pytest -s tests/cli/test_add.py::test_add_with_owner
    ============================= test session starts ==============================
    ...
    configfile: pyproject.toml
    plugins: cov-4.1.0, Faker-19.11.0
    collected 1 item

    tests/cli/test_add.py .

    ============================== 1 passed in 0.03s ===============================

Wir könnten Mocking auch auf eine andere Weise vermeiden. Wir könnten das
Verhalten vollständig über die CLI testen. Dazu müsste möglicherweise die
Ausgabe der Items-Liste geparst werden, um den korrekten Datenbankinhalt zu
überprüfen.

In der API gibt :func:`add_item()` einen Index zurück und bietet eine
:func:`get_item(index)`-Methode, die beim Testen hilft. Beide Methoden sind in
der CLI nicht vorhanden, könnten es aber sein. Wir könnten vielleicht die
Befehle ``items get index`` oder ``items info index`` hinzufügen, damit wir ein
Item abrufen können, anstatt ``items list für`` alles verwenden zu müssen.
``list`` unterstützt auch bereits Filterung. Vielleicht würde das Filtern nach
``index`` funktionieren, anstatt einen neuen Befehl hinzuzufügen. Und wir
könnten ``items add`` eine Ausgabe hinzufügen, die etwas sagt wie *Item
hinzugefügt bei Index 3*. Diese Änderungen würden in die Kategorie *Design for
Testability* fallen. Sie scheinen auch keine tiefen Eingriffe in die
Schnittstelle zu sein und sollten vielleicht in zukünftigen Versionen
berücksichtigt werden.

Plugins zur Unterstützung von Mocking
-------------------------------------

Wir haben uns bisher auf die direkte Verwendung von :doc:`mock
<python3:library/unittest.mock>` konzentriert. Es gibt jedoch viele Plugins, die
beim Mocking helfen, wie :abbr:`z.B. (zum Beispiel)` `pytest-mock
<https://pypi.org/project/pytest-mock/>`_, das eine ``mocker``-Fixture
bereitstellt. Ein Vorteil ist, dass das Fixture nach sich selbst aufräumt, so
dass ihr keinen ``with``-Block verwenden müsst, wie wir es in unseren Beispielen
getan haben.

Es gibt auch einige spezielle Mocking-Bibliotheken:

- Für das Mocking von Datenbankzugriffen eignen sich

  - `pytest-postgresql <https://pypi.org/project/pytest-postgresql/>`_
  - `pytest-mongo <https://pypi.org/project/pytest-mongo/>`_
  - `pytest-mysql <https://pypi.org/project/pytest-mysql/>`_
  - `pytest-dynamodb <https://pypi.org/project/pytest-dynamodb/>`_.

- Zum Testen von HTTP-Servern könnt ihr `pytest-httpserver
  <https://pypi.org/project/pytest-httpserver/>`_ verwenden.
- Zum Mocken von `requests  <https://pypi.org/project/requests/>`_ könnt ihr
  `responses <https://pypi.org/project/responses/>`_ oder `betamax
  <https://pypi.org/project/betamax/>`_ verwenden.
- Weitere Tools für verschiedene Anforderungen sind

  - `pytest-rabbitmq <https://pypi.org/project/pytest-rabbitmq/>`_
  - `pytest-solr <https://pypi.org/project/pytest-solr/>`_
  - `pytest-elasticsearch <https://pypi.org/project/pytest-elasticsearch/>`_ und
    `pytest-redis <https://pypi.org/project/pytest-redis/>`_.
