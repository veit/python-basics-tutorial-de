Test-Fixtures
=============

Nachdem ihr nun pytest zum Schreiben und Ausführen von Testfunktionen verwendet
habt, wollen wir uns nun den :term:`Fixtures <Test Fixture (Prüfvorrichtung)>`
zuwenden, die für die Strukturierung von Testcode für fast jedes nicht-triviale
Softwaresystem unerlässlich sind. Fixtures sind Funktionen, die von pytest vor
(und manchmal nach) den eigentlichen Testfunktionen ausgeführt werden. Der Code
in der Fixture kann tun, was immer ihr wollt. Ihr könnt Fixtures verwenden, um
einen Datensatz zu erhalten, mit dem die Tests arbeiten sollen. Ihr könnt
Fixtures verwenden, um ein System in einen bekannten Zustand zu versetzen, bevor
ein Test ausgeführt wird. Fixtures werden auch verwendet, um Daten für mehrere
Tests bereitzustellen.

In diesem Kapitel lernt ihr, wie ihr Fixtures erstellen und mit ihnen arbeiten
könnt. Ihr werden lernen, wie Ihr Fixtures strukturieren, um sowohl Setup- als
auch Teardown-Code zu speichern. Ihr werdet ``scope`` verwenden, um Fixtures
einmal über viele Tests laufen zu lassen, und lernen, wie Tests mehrere Fixtures
verwenden können. Ihr werdet auch lernen, wie ihr die Codeausführung durch
Fixtures und Testcode verfolgen könnt.

Doch bevor ihr euch mit Fixtures vertraut machen und sie zum Testen von Items
verwendet, sehen wir uns zunächst ein kleines Beispiel-Fixture an und erfahren,
wie Fixtures und Testfunktionen miteinander verbunden sind.

.. seealso::
   * `pytest fixtures <https://docs.pytest.org/en/latest/explanation/fixtures.html>`_
   * `pytest fixtures reference
     <https://docs.pytest.org/en/latest/reference/fixtures.html>`_
   * `How to use fixtures
     <https://docs.pytest.org/en/latest/how-to/fixtures.html#how-to-fixtures>`_

Erste Schritte mit Fixtures
---------------------------

Hier ist ein einfaches Fixture, das eine Zahl zurückgibt:

.. code-block:: python

    import pytest


    @pytest.fixture()
    def some_data():
        """The answer to the ultimate question"""
        return 42


    def test_some_data(some_data):
        """Use fixture return value in a test."""
        assert some_data == 42

Der ``@pytest.fixture()``-:doc:`Dekorator </functions/decorators>` wird
verwendet, um pytest mitzuteilen, dass eine Funktion eine Fixture ist. Wenn ihr
den Fixture-Namen in die Parameter-Liste einer Testfunktion aufnehmt, weiß
pytest, dass die Funktion vor der Ausführung des Tests ausgeführt werden soll.
Fixtures können Arbeit verrichten und auch Daten an die Testfunktion
zurückgeben. In diesem Fall dekoriert ``@pytest.fixture()`` die Funktion
:func:`some_data`. Der Test :func:`test_some_data` hat den Namen der Fixture,
:func:`some_data` als Parameter. pytest erkennt dies und sucht nach
einer Fixture mit diesem Namen.

Test-Fixtures in pytest beziehen sich auf den Mechanismus, der die Trennung von
*Vorbereitungen für*- und *Aufräumen nach*-Code von euren Testfunktionen
ermöglicht. pytest behandelt Exceptions während Fixtures anders als während
einer Testfunktion. Eine ``Exception`` oder ein ``assert``-Fehler oder ein
Aufruf von :func:`pytest.fail`, die während des eigentlichen Testcodes auftritt,
führt zu einem ``Fail``-Ergebnis. Während einer Fixture wird die Testfunktion
jedoch als ``Error`` gemeldet. Diese Unterscheidung ist hilfreich bei der
Fehlersuche, wenn ein Test nicht bestanden wurde. Wenn ein Test mit ``Fail``
endet, liegt der Fehler irgendwo in der Testfunktion, wenn ein Test mit
``Error`` endet, liegt der Fehler irgendwo in einer Fixture.

.. _setup-and-teardown-fixtures:

Fixtures für Setup und Teardown verwenden
-----------------------------------------

Fixtures werden uns beim Testen der Items-Anwendung eine große Hilfe sein. Die
Items-Anwendung besteht aus einer API, die den Großteil der Arbeit und der Logik
übernimmt, einem schlanken :abbr:`CLI (Command Line Interface)` und eine
Datenbank. Der Umgang mit der Datenbank ist ein Bereich, in dem Fixtures eine
große Hilfe sein werden:

.. code-block:: python

    from pathlib import Path
    from tempfile import TemporaryDirectory

    import items


    def test_empty():
        with TemporaryDirectory() as db_dir:
            db_path = Path(db_dir)
            db = items.ItemsDB(db_path)
            count = db.count()
            db.close()
            assert count == 0

Um :func:`count` aufrufen zu können, benötigen wir ein Datenbankobjekt, das wir
durch den Aufruf von :func:`items.ItemsDB(db_path)` erhalten. Die Funktion
:func:`items.ItemsDB` gibt ein ``ItemsDB``-Objekt zurück. Der Parameter
``db_path`` muss ein ``pathlib.Path``-Objekt sein, das auf das
Datenbankverzeichnis zeigt. Zum Testen funktioniert ein temporäres Verzeichnis,
das wir mit :func:`tempfile.TemporaryDirectory` erhalten.

Diese Testfunktion enthält jedoch einige Probleme: Der Code, um die Datenbank
einzurichten, bevor wir :func:`count` aufrufen, ist nicht wirklich das, was wir
testen wollen. Auch kann die ``assert``-Anweisung nicht vor dem Aufruf von
:func:`db.close` erfolgen, denn wenn die ``assert``-Anweisung fehlschlägt, wird
de Datenbankverbindung nicht mehr geschlossen. Diese Probleme lassen sich mit
pytest-Fixture lösen:

.. code-block:: python

    import pytest


    @pytest.fixture()
    def items_db():
        with TemporaryDirectory() as db_dir:
            db_path = Path(db_dir)
            db = items.ItemsDB(db_path)
            yield db
            db.close()


    def test_empty(items_db):
        assert items_db.count() == 0

Die Testfunktion selbst ist nun viel einfacher zu lesen, da wir die gesamte
Datenbankinitialisierung in eine Fixture namens ``items_db`` ausgelagert haben.
Die Fixture ``items_db`` bereitet den Test vor, indem sie die Datenbank
bereitstellt und anschließend das Datenbankobjekt ausgibt. Erst dann wird der
Test ausgeführt. Und erst nachdem der Test gelaufen ist, wird die Datenbank
wider geschlossen.

Fixture-Funktionen werden vor den Tests ausgeführt, die sie verwenden. Wenn es
in der Funktion einen ``yield`` gibt, wird dort angehalten, die Kontrolle an die
Tests übergeben und in der nächsten Zeile fortgesetzt, nachdem die Tests
abgeschlossen sind. Der Code oberhalb von ``yield`` ist *Setup* und der Code
nach dem ``yield`` ist *Teardown*. Der *Teardown* wird garantiert ausgeführt,
unabhängig davon, was während der Tests passiert.

In unserem Beispiel erfolgt ``yield`` innerhalb eines Kontextmanagers mit einem
temporären Verzeichnis. Dieses Verzeichnis bleibt bestehen, während das Fixture
verwendet wird und die Tests laufen. Nach Beendigung des Tests wird die
Kontrolle wieder an das Fixture übergeben, :func:`db.close` kann ausgeführt
werden und der ``with``-Block kann den Zugriff auf das Verzeichnis schließen.

Wir können Fixtures auch in mehreren Tests verwenden, :abbr:`z.B. (zum
Beispiel)` in

.. code-block:: python

    def test_count(items_db):
        items_db.add_item(items.Item("something"))
        items_db.add_item(items.Item("something else"))
        assert items_db.count() == 2

:func:`test_count` verwendet dasselbe ``items_db``-Fixture. Diesmal nehmen wir
die leere Datenbank und fügen zwei Items hinzu, bevor wir die Anzahl überprüfen.
Wir können ``items_db`` nun für jeden Test verwenden, der eine konfigurierte
Datenbank benötigt. Die einzelnen Tests, wie :func:`test_empty` und
:func:`test_count`, können kleiner gehalten werden und konzentrieren sich auf
das, was wir wirklich testen wollen, und nicht auf *Setup* und *Teardown*.

Fixture-Ausführung mit ``--setup-show`` anzeigen
------------------------------------------------

Da wir nun zwei Tests haben, die dieselbe Fixture verwenden, wäre es interessant
zu wissen, in welcher Reihenfolge sie aufgerufen werden. pytest bietet die
Kommandozeilen-Option ``--setup-show``, das uns die Reihenfolge der Operationen
von Tests und Fixtures anzeigt, einschließlich der Setup- und Teardown-Phasen
der Fixtures:

.. code-block:: pytest

    $ pytest --setup-show tests/test_count.py
    ============================= test session starts ==============================
    …
    collected 2 items

    tests/test_count.py
            SETUP    F items_db
            tests/test_count.py::test_empty (fixtures used: items_db).
            TEARDOWN F items_db
            SETUP    F items_db
            tests/test_count.py::test_count (fixtures used: items_db).
            TEARDOWN F items_db

    ============================== 2 passed in 0.01s ===============================

Wir können sehen, dass unser Test läuft, umgeben von den ``SETUP``- und
``TEARDOWN``-Teilen der ``items_db``-Fixture. Das ``F`` vor dem Namen der
Fixture zeigt an, dass die Fixture den Funktionsumfang verwendet, :abbr:`d.h.
(das heißt)` die Fixture wird vor jeder Testfunktion aufgerufen, die sie
verwendet, und danach wieder abgebaut. Schauen wir uns als nächstes den
Funktionsumfang an.

Umfang einer Fixture festlegen
------------------------------

Jedes Fixture hat einen bestimmten Umfang, der die Reihenfolge der Ausführung
von *Setup*  und *Teardown*  im Verhältnis zur Ausführung aller Testfunktionen,
die das Fixture verwenden, festlegt. Der Geltungsbereich bestimmt, wie oft
*Setup* und *Teardown* ausgeführt werden, wenn sie von mehreren Testfunktionen
verwendet werden.

Wenn das Einrichten und Verbinden mit der Datenbank oder das Erzeugen großer
Datensätze jedoch zeitaufwändig ist, kann es jedoch vorkommen, dass ihr dies
nicht für jeden einzelnen Test ausführen wollt. Wir können einen Bereich so
ändern, dass der langsame Teil nur einmal für mehrere Tests passiert. Ändern wir
den Bereich unserer Fixture so, dass die Datenbank nur einmal geöffnet wird,
indem ``scope="module"`` zum Fixture Decorator hinzugefügt wird:

.. code-block:: python

    @pytest.fixture(scope="module")
    def items_db():
        with TemporaryDirectory() as db_dir:
            db_path = Path(db_dir)
            db = items.ItemsDB(db_path)
            yield db
            db.close()

.. code-block:: pytest

    $ pytest --setup-show tests/test_count.py
    ============================= test session starts ==============================
    …
    collected 2 items

    tests/test_count.py
        SETUP    M items_db
            tests/test_count.py::test_empty (fixtures used: items_db).
            tests/test_count.py::test_count (fixtures used: items_db).
        TEARDOWN M items_db

    ============================== 2 passed in 0.01s ===============================

Wir haben diese Einrichtungszeit für die zweite Testfunktion eingespart. Durch
die Änderung des Modulumfangs kann jeder Test in diesem Modul, der die
``items_db``-Fixture verwendet, dieselbe Instanz davon nutzen, ohne dass
zusätzliche Einrichtungs- und Abbauzeit anfällt.

Der Fixture-Parameter ``scope`` erlaubt jedoch mehr als nur ``module``:

+-----------------------+-----------------------------------------------+
| ``scope``-Werte       | Beschreibung                                  |
+=======================+===============================================+
| ``scope='function'``  | Standardwert. Wird einmal pro Testfunktion    |
|                       | ausgeführt.                                   |
+-----------------------+-----------------------------------------------+
| ``scope='class'``     | Wird einmal pro Testklasse ausgeführt,        |
|                       | unabhängig davon, wie viele Testmethoden die  |
|                       | Klasse enthält.                               |
+-----------------------+-----------------------------------------------+
| ``scope='module'``    | Wird einmal pro Modul ausgeführt, unabhängig  |
|                       | davon, wie viele Testfunktionen oder          |
|                       | -methoden oder andere Fixtures im Modul es    |
|                       | verwenden.                                    |
+-----------------------+-----------------------------------------------+
| ``scope='package'``   | Wird einmal pro Paket oder Testverzeichnis    |
|                       | ausgeführt, unabhängig davon, wie viele       |
|                       | Testfunktionen oder -methoden oder andere     |
|                       | Fixtures in dem Paket verwendet werden.       |
+-----------------------+-----------------------------------------------+
| ``scope='session'``   | Wird einmal pro Sitzung ausgeführt. Alle      |
|                       | Testmethoden und -funktionen, die ein Fixture |
|                       | mit Session-Scope verwenden, teilen sich      |
|                       | einen Aufruf zum Einrichten und Abbauen.      |
+-----------------------+-----------------------------------------------+

Der Geltungsbereich wird also bei der Definition einer Fixture festgelegt und
nicht an der Stelle, an der sie aufgerufen wird. Die Testfunktionen, die ein
Fixture verwenden, steuern nicht, wie oft ein Fixture auf- und abgebaut wird.

Bei einer Fixture, die innerhalb eines Testmoduls definiert ist, verhalten sich
die Session- und Package-Scopes genau wie die Module-Scopes. Um diese anderen
Bereiche nutzen zu können, müssen wir eine :file:`conftest.py`-Datei verwenden.

Gemeinsame Nutzung von Fixtures mit :file:`conftest.py`
-------------------------------------------------------

Ihr könnt Fixtures in einzelne Testdateien einfügen, aber um Fixtures für
mehrere Testdateien freizugeben, müsst ihr eine :file:`conftest.py`-Datei
entweder im selben Verzeichnis wie die Testdatei, die sie verwendet, oder in
einem übergeordneten Verzeichnis verwenden. Dabei ist die Datei
:file:`conftest.py` optional. Sie wird von pytest als ein *lokales Plugin*
betrachtet und kann Hook-Funktionen und Fixtures enthalten. Beginnen wir damit,
das ``items_db``-Fixture aus :file:`test_count.py` in eine
:file:`conftest.py`-Datei im selben Verzeichnis zu verschieben:

.. code-block:: python

    from pathlib import Path
    from tempfile import TemporaryDirectory

    import pytest

    import items


    @pytest.fixture(scope="session")
    def items_db():
        """ItemsDB object connected to a temporary database"""
        with TemporaryDirectory() as db_dir:
            db_path = Path(db_dir)
            db = items.ItemsDB(db_path)
            yield db
            db.close()

.. note::
   Fixtures können nur von anderen Fixtures desselben oder eines größeren
   Bereichs abhängen. Eine Fixture mit Funktionsumfang kann also von anderen
   Fixtures mit Funktionsumfang abhängen. Ein Function-Scope-Fixture kann auch
   von ``class``-, ``module``- und ``session``-Scope-Fixtures abhängen, aber
   nicht umgekehrt.

.. warning::
   Obwohl :file:`conftest.py` ein Python-Modul ist, sollte es nicht von
   Testdateien importiert werden. Die Datei :file:`conftest.py` wird automatisch
   von pytest gelesen, so dass ihr nirgendwo ``conftest`` importieren müsst.

Finden, wo Fixtures definiert sind
----------------------------------

Wir haben eine Fixture aus dem Testmodul in eine :file:`conftest.py`-Datei
verschoben. Wir können :file:`conftest.py`-Dateien auf wirklich jeder Ebene
unseres Testverzeichnisses haben. Die Tests können jede Fixture verwenden, die
sich im selben Testmodul wie eine Testfunktion befindet, oder in einer
:file:`conftest.py`-Datei im selben Verzeichnis, oder auf jeder Ebene des
übergeordneten Verzeichnisses bis hin zur Wurzel der Tests.

Das bringt ein Problem mit sich, wenn man sich nicht mehr daran erinnern kann,
wo sich eine bestimmte Fixture befindet und man den Quellcode sehen möchte.
Mit ``pytest --fixtures`` können wir uns anzeigen lassen, wo die Fixtures
definiert sind:

.. code-block:: pytest

    pytest --fixtures
    ============================= test session starts ==============================
    …
    collected 10 items
    cache -- .../_pytest/cacheprovider.py:532
        Return a cache object that can persist state between testing sessions.
    …
    tmp_path_factory [session scope] -- .../_pytest/tmpdir.py:245
        Return a :class:`pytest.TempPathFactory` instance for the test session.

    tmp_path -- .../_pytest/tmpdir.py:260
        Return a temporary directory path object which is unique to each test
        function invocation, created as a sub directory of the base temporary
        directory.


    --------------------- fixtures defined from tests.conftest ---------------------
    items_db [session scope] -- conftest.py:10
        ItemsDB object connected to a temporary database


    ------------------ fixtures defined from tests.test_fixtures -------------------
    some_data -- test_fixtures.py:5
        The answer to the ultimate question


    ============================ no tests ran in 0.00s =============================

pytest zeigt uns eine Liste aller verfügbaren Fixtures, die unser Test verwenden
kann. Diese Liste enthält eine Reihe von eingebauten Fixtures, die wir uns in
:doc:`builtin-fixtures` ansehen werden, sowie Fixtures, die von :doc:`plugins`
bereitgestellt werden. Die Fixtures, die in :file:`conftest.py`-Dateien gefunden
werden, stehen am Ende der Liste. Wenn ihr ein Verzeichnis angebt, listet pytest
die Fixtures auf, die für Tests in diesem Verzeichnis verfügbar sind. Wenn ihr
den Namen einer Testdatei angebt, schließt pytest auch die in den Testmodulen
definierten Fixtures ein.

Die Ausgabe von pytest enthält

* die erste Zeile des Docstrings der Fixture

  Durch Hinzufügen von ``-v`` wird der gesamte Docstring eingeschlossen.

* die Datei- und Zeilennummer, in der die Fixture definiert ist
* der Pfad, wenn die Fixture sich nicht im aktuellen Verzeichnis befindet

.. note::
   Beachtet, dass wir für pytest 6.x ``-v`` verwenden müssen, um den Pfad und
   die Zeilennummern zu erhalten. Erst ab pytest 7 werden diese ohne weitere
   Option hinzugefügt.

Ihr könnt auch ``--fixtures-per-test`` verwenden, um zu sehen, welche Fixtures
von jedem Test verwendet werden und wo die Fixtures definiert sind:

.. code-block:: pytest

    pytest --fixtures-per-test test_count.py::test_empty
    ============================= test session starts ==============================
    …
    collected 1 item

    ------------------------- fixtures used by test_empty --------------------------
    ------------------------------ (test_count.py:5) -------------------------------
    items_db -- conftest.py:10
        ItemsDB object connected to a temporary database

    ============================ no tests ran in 0.00s =============================

In diesem Beispiel haben wir einen einzelnen Test angegeben:
``test_count.py::test_empty``. Es können jedoch auch Dateien oder Verzeichnisse
angegeben werden.

Mehrere Fixture-Levels verwenden
--------------------------------

Unser Testcode ist momentan noch problematisch, da beide Tests davon abhängen,
dass die Datenbank zu Beginn leer ist. Dieses Problem wird sehr deutlich, wenn
wir einen dritten Test hinzufügen:

.. code-block:: pytest

    $ pytest test_count.py::test_count2
    ============================= test session starts ==============================
    …
    collected 1 item

    test_count.py .                                                          [100%]

    ============================== 1 passed in 0.00s ===============================

Es funktioniert einzeln ausgeführt, aber nicht, wenn er nach
``test_count.py::test_count`` ausgeführt wird:

.. code-block:: pytest

    $ pytest test_count.py
    ============================= test session starts ==============================
    …
    collected 3 items

    test_count.py ..F                                                        [100%]

    =================================== FAILURES ===================================
    _________________________________ test_count2 __________________________________

    items_db = <items.api.ItemsDB object at 0x103d3a390>

        def test_count2(items_db):
            items_db.add_item(items.Item("something different"))
    >       assert items_db.count() == 1
    E       assert 3 == 1
    E        +  where 3 = <bound method ItemsDB.count of <items.api.ItemsDB object at 0x103d3a390>>()
    E        +    where <bound method ItemsDB.count of <items.api.ItemsDB object at 0x103d3a390>> = <items.api.ItemsDB object at 0x103d3a390>.count

    test_count.py:15: AssertionError
    =========================== short test summary info ============================
    FAILED test_count.py::test_count2 - assert 3 == 1
    ========================= 1 failed, 2 passed in 0.03s ==========================

Es gibt drei Items in der Datenbank, weil der vorherige Test bereits zwei
Elemente hinzugefügte, bevor ``test_count2`` ausgeführt wurde. Tests sollten
sich jedoch nicht auf die Ausführungsreihenfolge verlassen. ``test_count2`` ist
nur erfolgreich, wenn er alleine ausgeführt wird, schlägt aber fehl, wenn er
nach ``test_count`` ausgeführt wird.

Wenn wir immer noch versuchen wollen, mit einer offenen Datenbank zu arbeiten,
aber alle Tests mit null Items in der Datenbank starten sollen, können wir das
tun, indem wir eine weitere Fixture in :file:`conftest.py` hinzufügen:

.. code-block:: python

    @pytest.fixture(scope="session")
    def db():
        """ItemsDB object connected to a temporary database"""
        with TemporaryDirectory() as db_dir:
            db_path = Path(db_dir)
            db_ = items.ItemsDB(db_path)
            yield db_
            db_.close()


    @pytest.fixture(scope="function")
    def items_db(db):
        """ItemsDB object that's empty"""
        db.delete_all()
        return db

Ich habe die alte ``items_db`` in ``db`` umbenannt und sie in den
Session-Bereich verschoben.

Die ``items_db``-Fixture hat ``db`` in ihrer Parameter-Liste, was bedeutet, dass
sie von der ``db``-Fixture abhängt. Außerdem ist ``items_db`` im
``function``-Bereich, was einen engeren Bereich als ``db`` darstellt. Wenn
Fixtures von anderen Fixtures abhängen, können sie nur Fixtures verwenden, die
den gleichen oder einen größeren Geltungsbereich haben.

Schauen wir mal, ob es funktioniert:

.. code-block:: pytest

    $ pytest --setup-show test_count.py
    ============================= test session starts ==============================
    …
    collected 3 items

    test_count.py
    SETUP    S db
            SETUP    F items_db (fixtures used: db)
            test_count.py::test_empty (fixtures used: db, items_db).
            TEARDOWN F items_db
            SETUP    F items_db (fixtures used: db)
            test_count.py::test_count (fixtures used: db, items_db).
            TEARDOWN F items_db
            SETUP    F items_db (fixtures used: db)
            test_count.py::test_count2 (fixtures used: db, items_db).
            TEARDOWN F items_db
    TEARDOWN S db

    ============================== 3 passed in 0.00s ===============================

Wir sehen, dass die Einrichtung für ``db`` zuerst erfolgt und den
Geltungsbereich der Session hat (vom ``S``). Das Setup für ``items_db`` erfolgt
als nächstes und vor jedem Test-Funktionsaufruf und hat den Geltungsbereich der
Funktion (vom ``F``). Außerdem werden alle drei Tests bestanden.

Die Verwendung von Fixtures für mehrere Stufen kann unglaubliche
Geschwindigkeitsvorteile bieten und die Unabhängigkeit der Testreihenfolge
wahren.

Mehrere Fixtures pro Test oder Fixture verwenden
------------------------------------------------

Eine weitere Möglichkeit, mehrere Fixtures zu verwenden, besteht darin, mehr als
eine in einer Funktion oder einem Fixture zu verwenden. Zum Beispiel können wir
einige vorgeplante Items zusammenstellen, um sie in einem Fixture zu testen:

.. code-block:: python

    @pytest.fixture(scope="session")
    def items_list():
        """List of different Item objects"""
        return [
            items.Item("Add Python 3.12 static type improvements", "veit", "todo"),
            items.Item("Add tips for efficient testing", "veit", "wip"),
            items.Item("Update cibuildwheel section", "veit", "done"),
            items.Item("Add backend examples", "veit", "done"),
        ]

Dann können wir sowohl ``empty_db`` als auch ``items_list`` in
:file:`test_add.py` verwenden:

.. code-block:: python

    def test_add_list(items_db, items_list):
        expected_count = len(items_list)
        for i in items_list:
            items_db.add_item(i)
        assert items_db.count() == expected_count

Und auch Fixtures können mehrere andere Fixtures verwenden:

.. code-block:: python

    @pytest.fixture(scope="function")
    def populated_db(items_db, items_list):
        """ItemsDB object populated with 'items_list'"""
        for i in items_list:
            items_db.add_item(i)
        return items_db

Die Fixture ``populated_db`` muss im ``function``-Bereich liegen, da sie
``items_db`` verwendet, das bereits im ``function``-Bereich liegt. Wenn ihr
versuchen solltet, ``populated_db`` in den ``module``-Bereich oder einen
größeren Bereich zu setzen, wird pytest einen Fehler ausgeben. Vergesst nicht,
dass ihr, wenn ihr keinen Bereich angebt, Fixtures im ``function``-Bereich
erhaltet. Tests, die eine gefüllte Datenbank benötigen, können dies nun einfach
tun mit

.. code-block:: python

    def populated(populated_db):
        assert populated_db.count() > 0

Wir haben gesehen, wie verschiedene Fixture-Scopes funktionieren und wie
verschiedene Scopes in verschiedenen Fixtures genutzt werden können. Es kann
jedoch vorkommen, dass ihr einen Bereich zur Laufzeit festlegen müsst. Das ist
mit dynamischem Scoping möglich.

Fixture-Scope dynamisch festlegen
---------------------------------

Nehmen wir an, wir haben die Fixtures so eingerichtet wie jetzt, mit ``db`` im
``session``-Scope und ``items_db`` im ``function``-Bereich. Nun besteht jedoch
die Gefahr, dass das ``items_db``-Fixture leer ist, weil es :func:`delete_all`
aufruft. Deshalb wollen wir eine Möglichkeit schaffen, die Datenbank für jede
Testfunktion vollständig einzurichten, indem wir den Scope der ``db``-Fixture
zur Laufzeit dynamisch festlegen. Hierfür ändern wir zuerst den Scope von
``db`` in der :file:`conftest.py`-Datei:

.. code-block:: python

    @pytest.fixture(scope=db_scope)
    def db():
        """ItemsDB object connected to a temporary database"""
        with TemporaryDirectory() as db_dir:
            db_path = Path(db_dir)
            db_ = items.ItemsDB(db_path)
            yield db_
            db_.close()

Anstelle eines bestimmten Bereichs haben wir einen Funktionsnamen eingegeben:
``db_scope``. Nun müssen wir also noch diese Funktion schreiben:

.. code-block:: python

    def db_scope(fixture_name, config):
        if config.getoption("--fdb", None):
            return "function"
        return "session"

Es gibt viele Möglichkeiten, wie wir herausfinden können, welchen Bereich wir
verwenden sollen. In diesem Fall habe ich mich für eine neue
Kommandozeilen-Option ``--fdb`` für den ``function``-Bereich der Datenbank
entschieden. Damit wir diese neue Option mit pytest verwenden können, müssen wir
eine Hook-Funktion in der :file:`conftest.py`-Datei schreiben, die ich in
:doc:`plugins` näher erläutern werde:

.. code-block:: python

    def pytest_addoption(parser):
        parser.addoption(
            "--fdb",
            action="store_true",
            default=False,
            help="Create new db for each test",
        )

Nach all dem ist das Standardverhalten dasselbe wie vorher, mit ``db`` im
``session``-Scope:

.. code-block:: pytest

    $ pytest --setup-show test_count.py
    ============================= test session starts ==============================
    …
    collected 3 items

    test_count.py
    SETUP    S db
            SETUP    F items_db (fixtures used: db)
            test_count.py::test_empty (fixtures used: db, items_db).
            TEARDOWN F items_db
            SETUP    F items_db (fixtures used: db)
            test_count.py::test_count (fixtures used: db, items_db).
            TEARDOWN F items_db
            SETUP    F items_db (fixtures used: db)
            test_count.py::test_count2 (fixtures used: db, items_db).
            TEARDOWN F items_db
    TEARDOWN S db

    ============================== 3 passed in 0.00s ===============================

Wenn wir jedoch die neue Option verwenden, erhalten wir eine ``db``-Fixture im
``function``-Bereich:

.. code-block:: pytest

    $ pytest --fdb --setup-show test_count.py
    ============================= test session starts ==============================
    …
    collected 3 items

    test_count.py
            SETUP    F db
            SETUP    F items_db (fixtures used: db)
            test_count.py::test_empty (fixtures used: db, items_db).
            TEARDOWN F items_db
            TEARDOWN F db
            SETUP    F db
            SETUP    F items_db (fixtures used: db)
            test_count.py::test_count (fixtures used: db, items_db).
            TEARDOWN F items_db
            TEARDOWN F db
            SETUP    F db
            SETUP    F items_db (fixtures used: db)
            test_count.py::test_count2 (fixtures used: db, items_db).
            TEARDOWN F items_db
            TEARDOWN F db

    ============================== 3 passed in 0.00s ===============================

Die Datenbank wird nun vor jeder Testfunktion aufgebaut und danach wieder
abgebaut.

``autouse`` für Fixtures, die immer verwendet werden
----------------------------------------------------

Bisher wurden alle von Tests verwendeten Fixtures durch die Tests oder eine
andere Fixture in einer Parameter-Liste benannt. Ihr könnt jedoch
``autouse=True`` verwenden, um ein Fixture immer laufen zu lassen. Dies eignet
sich gut für Code, der zu bestimmten Zeiten ausgeführt werden soll, aber Tests
sind nicht wirklich von einem Systemzustand oder Daten aus der Fixture abhängig,
:abbr:`z.B. (zum Beispiel)`:

.. code-block::

    import os


    @pytest.fixture(autouse=True, scope="session")
    def setup_test_env():
        found = os.environ.get("APP_ENV", "")
        os.environ["APP_ENV"] = "TESTING"
        yield
        os.environ["APP_ENV"] = found

.. code-block:: pytest

    pytest --setup-show test_count.py
    ============================= test session starts ==============================
    …
    collected 3 items

    test_count.py
    SETUP    S setup_test_env
    SETUP    S db
            SETUP    F items_db (fixtures used: db)
            test_count.py::test_empty (fixtures used: db, items_db, setup_test_env).
            TEARDOWN F items_db
            SETUP    F items_db (fixtures used: db)
            test_count.py::test_count (fixtures used: db, items_db, setup_test_env).
            TEARDOWN F items_db
            SETUP    F items_db (fixtures used: db)
            test_count.py::test_count2 (fixtures used: db, items_db, setup_test_env).
            TEARDOWN F items_db
    TEARDOWN S db
    TEARDOWN S setup_test_env

    ============================== 3 passed in 0.00s ===============================

.. tip::
   Das ``autouse``-Feature sollte eher die Ausnahme als die Regel sein.
   Entscheidet euch für benannte Fixtures, es sei denn, ihr habt einen wirklich
   triftigen Grund, dies nicht zu tun.

Fixtures umbenennen
-------------------

Der Name einer Fixture, der in der Parameter-Liste von Tests und anderen
Fixtures aufgeführt ist, die diese Fixture verwenden, ist normalerweise
derselbe wie der Funktionsname der Fixture. Pytest erlaubt jedoch das Umbenennen
von Fixtures mit einem Namensparameter an ``@pytest.fixture``:

.. code-block:: python

    import pytest


    from items import cli


    @pytest.fixture(scope="session", name="db")
    def _db():
        """The db object"""
        yield db()


    def test_empty(db):
        assert items_db.count() == 0

Ein Fall, in dem eine Umbenennung sinnvoll sein kann, ist, wenn der
naheliegendste Fixture-Name bereits als Variablen- oder Funktionsname existiert.
