Testparametrisierung
====================

Durch :term:`Parametrisierung <Parameter>` können wir eine Testfunktion in viele
Testfälle umwandeln, um mit weniger Arbeit gründlicher zu testen. Hierfür
übergeben wir dem Test mehrere Sätze von Argumenten, um neue Testfälle zu
erstellen. Wir werfen einen Blick auf redundanten Code, den wir mit
Parametrisierung vermeiden. Dann werden wir uns drei Möglichkeiten ansehen, und
zwar in der Reihenfolge, in der sie ausgewählt werden sollten:

- Parametrisierung von Funktionen
- Parametrisierung von Fixtures
- Verwendung einer Hook-Funktion namens ``pytest_generate_tests``

Dabei werden wir dasselbe Parametrisierungsproblem mit allen drei Methoden
lösen, auch wenn manchmal eine Lösung der anderen vorzuziehen ist.

Testen ohne ``parametrize``
---------------------------

Das Senden einiger Werte durch eine Funktion und das Überprüfen der Ausgabe auf
Korrektheit ist ein gängiges Muster beim Testen von Software. Der einmalige
Aufruf einer Funktion mit einem Satz von Werten reicht jedoch selten aus, um
die Funktionen vollständig zu testen. Parametrisiertes Testen ist eine
Möglichkeit, mehrere Datensätze durch denselben Test zu schicken und pytest
berichten zu lassen, wenn einer der Datensätze fehlschlägt. Um das Problem zu
verstehen, das parametrisierte Tests zu lösen versuchen, schreiben wir einige
Tests für die API-Methode ``finish()`` aus :file:`src/items/api.py`:

.. code-block:: python

   def finish(self, item_id: int):
       """Set an item state to done."""
       self.update_item(item_id, Item(state="done"))

Die in der Anwendung verwendeten Zustände sind *todo*, *in progress* und *done*,
und ``finish()`` setzt den Zustand einer Karte auf *done*. Um dies zu testen,
könnten wir

#. ein Item-Objekt erstellen und es zur Datenbank hinzufügen, damit wir eine
   Item haben, mit der wir arbeiten können,
#. ``finish()`` aufrufen
#. sicherstellen, dass der Endzustand *done* ist.

Eine Variable ist der Startstatus der Karte. Er könnte "todo", "in progress"
oder sogar schon "done" sein. Lasst uns alle drei testen:

.. code-block:: python

   from items import Item


   def test_finish_from_in_prog(items_db):
       index = items_db.add_item(
           Item("Update pytest section", state="in progress")
       )
       items_db.finish(index)
       item = items_db.get_item(index)
       assert item.state == "done"


   def test_finish_from_done(items_db):
       index = items_db.add_item(
           Item("Update cibuildwheel section", state="done")
       )
       items_db.finish(index)
       item = items_db.get_item(index)
       assert item.state == "done"


   def test_finish_from_todo(items_db):
       index = items_db.add_item(Item("Update mock tests", state="todo"))
       items_db.finish(index)
       item = items_db.get_item(index)
       assert item.state == "done"

Lassen wir es laufen:

.. code-block:: pytest

   pytest -v tests/test_finish.py
   ============================= test session starts ==============================
   …
   collected 3 items

   tests/test_finish.py::test_finish_from_in_prog PASSED                    [ 33%]
   tests/test_finish.py::test_finish_from_done PASSED                       [ 66%]
   tests/test_finish.py::test_finish_from_todo PASSED                       [100%]

   ============================== 3 passed in 0.00s ===============================

Die Testfunktionen sind sehr ähnlich. Die einzigen Unterschiede sind der
Ausgangszustand und die Zusammenfassung.  Eine Möglichkeit, den redundanten Code
zu reduzieren, besteht darin, die drei Funktionen in einer einzigen Funktion
zusammenzufassen, etwa so:

.. code-block:: python

   from items import Item


   def test_finish(items_db):
       for i in [
           Item("Update pytest section", state="done"),
           Item("Update cibuildwheel section", state="in progress"),
           Item("Update mock tests", state="todo"),
       ]:
           index = items_db.add_item(i)
           items_db.finish(index)
           item = items_db.get_item(index)
           assert item.state == "done"

Nun lassen wir :file:`tests/test_finish.py` erneut laufen:

.. code-block:: pytest

   $ pytest -v tests/test_finish.py
   ============================= test session starts ==============================
   …
   collected 1 item

   tests/test_finish.py::test_finish PASSED                                 [100%]

   ============================== 1 passed in 0.00s ===============================

Auch dieser Test ist bestanden, und wir haben den überflüssigen Code eliminiert.
Aber es ist doch nicht dasselbe:

- Es wird nur ein Testfall gemeldet, statt drei.
- Wenn einer der Testfälle fehlschlägt, wissen wir nicht, welcher es ist, ohne
  einen Blick auf den Traceback oder andere Debugging-Informationen zu werfen.
- Wenn einer der Testfälle fehlschlägt, werden die darauf folgenden Testfälle
  nicht ausgeführt. pytest stoppt die Ausführung eines Tests, wenn eine
  Assertion fehlschlägt.

.. _parameterise-functions:

Funktionen parametrisieren
--------------------------

Um eine Testfunktion zu parametrisieren, fügt der Testdefinition Parameter hinzu
und verwendet den ``@pytest.mark.parametrize()``-Dekorator, um die an den Test
zu übergebenden Argumente zu definieren, etwa so:

.. code-block:: python

   import pytest

   from items import Item


   @pytest.mark.parametrize(
       "start_summary, start_state",
       [
           ("Update pytest section", "done"),
           ("Update cibuildwheel section", "in progress"),
           ("Update mock tests", "todo"),
       ],
   )
   def test_finish(items_db, start_summary, start_state):
       initial_item = Item(summary=start_summary, state=start_state)
       index = items_db.add_item(initial_item)
       items_db.finish(index)
       item = items_db.get_item(index)
       assert item.state == "done"

Die ``test_finish()``-Funktion  hat jetzt ihre ursprüngliche
``items_db``-Fixture als Parameter, aber auch zwei neue Parameter:
``start_summary`` und ``start_state``. Diese stimmen direkt mit dem ersten
Argument von ``@pytest.mark.parametrize()`` überein.

#. Das erste Argument von ``@pytest.mark.parametrize()`` ist eine Liste von
   Parameter-Namen. Dieses Argument könnte auch eine Liste von Zeichenketten
   sein, wie :abbr:`z.B. (zum Beispiel)` ``["start_summary", "start_state"]``
   oder eine komma-getrennte Zeichenkette ``"start_summary, start_state"``.
#. Das zweite Argument von ``@pytest.mark.parametrize()`` ist unsere Liste von
   Testfällen. Jedes Element in der Liste ist ein Testfall, der durch ein Tupel
   oder eine Liste dargestellt wird, die ein Element für jedes Argument enthält,
   das an die Testfunktion gesendet wird.

pytest führt diesen Test einmal für jedes ``(start_summary, start_state)``-Paar
durch und meldet jeden als separaten Test:

.. code-block:: console

   $ pytest -v tests/test_finish.py
   ============================= test session starts ==============================
   …
   collected 3 items

   tests/test_finish.py::test_finish[Update pytest section-done] PASSED    [ 33%]
   tests/test_finish.py::test_finish[Update cibuildwheel section-in progress] PASSED [ 66%]
   tests/test_finish.py::test_finish[Update mock tests-todo] PASSED        [100%]

   ============================== 3 passed in 0.00s ===============================

Diese Verwendung von ``parametrize()`` funktioniert für unsere Zwecke.
Allerdings ist es für diesen Test ``start_summary`` nicht wirklich wichtig und
macht jeden Testfall komplexer. Ändern wir die Parametrisierung in
``start_state`` und sehen uns an, wie sich die Syntax ändert:

.. code-block:: python

   import pytest

   from items import Item


   @pytest.mark.parametrize(
       "start_state",
       [
           "done",
           "in progress",
           "todo",
       ],
   )
   def test_finish(items_db, start_state):
       i = Item("Update pytest section", state=start_state)
       index = items_db.add_item(i)
       items_db.finish(index)
       item = items_db.get_item(index)
       assert item.state == "done"

Wenn wir die Tests jetzt ausführen, konzentrieren sie sich auf die Veränderung,
die uns wichtig ist:

.. code-block:: console

   $ pytest -v tests/test_finish.py
   ============================= test session starts ==============================
   …
   collected 3 items

   tests/test_finish.py::test_finish[done] PASSED                           [ 33%]
   tests/test_finish.py::test_finish[in progress] PASSED                    [ 66%]
   tests/test_finish.py::test_finish[todo] PASSED                           [100%]

   ============================== 3 passed in 0.01s ===============================

Die Ausgabe der beiden Beispiele, unterscheidet sich insofern, dass jetzt nur
noch der Ausgangszustand aufgelistet wird, also *todo*, *in progress* und
*done*. Im vorherigen Beispiel zeigte pytest noch die Werte beider Parameter an,
getrennt durch einen Bindestrich ``-``. Wenn sich nur ein Parameter ändert, wird
kein Bindestrich benötigt.

Fixtures parametrisieren
------------------------

Bei der Funktionsparametrisierung rief pytest unsere Testfunktion für jeden Satz
von Argumenten, die wir angegeben haben, jeweils einmal auf. Mit der
Fixture-Parametrisierung verschieben wir diese Parameter in eine Fixture. pytest
ruft die Fixture dann jeweils einmal für jeden Satz von Werten auf, die wir
angeben. Anschließend wird jede Testfunktion, die von der Fixture abhängt, für
jeden Fixture-Wert einmal aufgerufen. Auch die Syntax ist anders:

.. code-block:: python

   import pytest

   from items import Item


   @pytest.fixture(params=["done", "in progress", "todo"])
   def start_state(request):
       return request.param


   def test_finish(items_db, start_state):
       i = Item("Update pytest section", state=start_state)
       index = items_db.add_item(i)
       items_db.finish(index)
       item = items_db.get_item(index)
       assert item.state == "done"

Das bedeutet, dass pytest ``start_state()`` dreimal aufruft, jeweils einmal für
alle Werte in ``params``. Jeder Wert von ``params`` wird in ``request.param``
gespeichert, damit das Fixture ihn verwenden kann. Innerhalb von
``start_state()`` könnten wir Code haben, der vom Parameterwert abhängt. In
diesem Fall wird jedoch nur der Wert des Parameters zurückgegeben.

Die Funktion ``test_finish()`` ist identisch mit der Funktion, die wir bei der
Funktionsparametrisierung verwendet haben, jedoch ohne den Dekorator
``parametrize``. Da sie ``start_state`` als Parameter hat, ruft pytest sie
einmal für jeden Wert auf, der an die ``start_state()``-Fixture übergeben wird.
Und nach all dem sieht die Ausgabe genauso aus wie vorher:

.. code-block:: console

   $ pytest -v tests/test_finish.py
   ============================= test session starts ==============================
   …
   collected 3 items

   tests/test_finish.py::test_finish[done] PASSED                          [ 33%]
   tests/test_finish.py::test_finish[in progress] PASSED                   [ 66%]
   tests/test_finish.py::test_finish[todo] PASSED                          [100%]

   ============================== 3 passed in 0.01s ===============================

Auf den ersten Blick erfüllt die Fixture-Parametrisierung in etwa den gleichen
Zweck wie die Funktionsparametrisierung, allerdings mit etwas mehr Code. Die
Fixture-Parametrisierung hat jedoch den Vorteil, dass für jeden Satz von
Argumenten ein Fixture ausgeführt wird. Dies ist nützlich, wenn ihr *Setup* -
oder *Teardown*-Code habt, der für jeden Testfall ausgeführt werden muss,
:abbr:`z.B. (zum Beispiel)` eine andere Datenbankverbindung oder ein anderer
Dateiinhalt oder was auch immer.

Es hat auch den Vorteil, dass viele Testfunktionen mit demselben Satz von
Parametern ausgeführt werden können. Alle Tests, die die ``start_state``-Fixture
verwenden, werden alle drei Mal aufgerufen, einmal für jeden Startzustand.

Mit ``pytest_generate_tests`` parametrisieren
---------------------------------------------

Die dritte Möglichkeit der Parametrisierung ist die Verwendung einer
Hook-Funktion namens ``pytest_generate_tests``. Hook-Funktionen werden oft von
:doc:`plugins` verwendet, um den normalen Arbeitsablauf von pytest zu verändern.
Aber wir können viele von ihnen in Testdateien und :file:`conftest.py`-Dateien
verwenden.

Die Implementierung des gleichen Ablaufs wie zuvor mit ``pytest_generate_tests``
sieht wie folgt aus:

.. code-block:: python

   from items import Item


   def pytest_generate_tests(metafunc):
       if "start_state" in metafunc.fixturenames:
           metafunc.parametrize("start_state", ["done", "in progress", "todo"])


   def test_finish(items_db, start_state):
       i = Item("Update pytest section", state=start_state)
       index = items_db.add_item(i)
       items_db.finish(index)
       item = items_db.get_item(index)
       assert item.state == "done"

Die ``test_finish()``-Funktion hat sich nicht geändert; wir haben nur die Art
und Weise geändert, wie pytest den Wert für ``initial_state`` bei jedem
Testaufruf einträgt.

Die ``pytest_generate_tests``-Funktion, die wir bereitstellen, wird von pytest
aufgerufen, wenn es seine Liste der auszuführenden Tests erstellt. Sie ist sehr
leistungsfähig und unser Beispiel ist nur ein einfacher Fall, um die
Funktionalität früherer Parametrisierungsmethoden abzugleichen.
``pytest_generate_tests`` ist jedoch besonders nützlich, wenn wir die
Parametrisierungsliste zur Zeit der Testsammlung auf interessante Weise ändern
wollen. Hier sind ein paar Möglichkeiten:

- Wir könnten unsere Parametrisierungsliste auf einer Kommandozeilen-Option
  basierend ändern, die uns :samp:`metafunc.config.getoption("--SOME_OPTION")`
  [#]_ gibt. Vielleicht fügen wir eine ``--excessive``- Option hinzu, um mehr
  Werte zu testen, oder eine ``--quick-Option``, um nur einige wenige zu testen.
- Die Parametrisierungsliste eines Parameters kann auf dem Vorhandensein eines
  anderen Parameters basieren. Bei Testfunktionen, die zwei zusammenhängende
  Parameter abfragen, können wir beispielsweise beide mit einem anderen Satz von
  Werten parametrisieren, als wenn der Test nur einen der Parameter abfragt.
- Wir können zwei verwandte Parameter gleichzeitig parametrisieren zum Beispiel
  :samp:`metafunc.parametrize({"TUTORIAL, TOPIC", [("PYTHON BASICS",
  "TESTING"), ("PYTHON BASICS", "DOCUMENTING"), ("PYTHON FOR DATA SCIENCE,
  "GIT"), …]})`.

Wir haben nun drei Möglichkeiten der Parametrisierung von Tests kennengelernt.
Obwohl wir damit im :samp:`{finish()}`-Beispiel nur drei Testfälle aus einer
Testfunktion erstellen, kann die Parametrisierung eine große Anzahl von
Testfällen erzeugen.

.. seealso::
   * `Basic pytest_generate_tests example
     <https://docs.pytest.org/en/stable/how-to/parametrize.html#basic-pytest-generate-tests-example>`_
   * `Generating parameters combinations, depending on command line
     <https://docs.pytest.org/en/stable/example/parametrize.html#generating-parameters-combinations-depending-on-command-line>`_
   * `A quick port of “testscenarios”
     <https://docs.pytest.org/en/stable/example/parametrize.html#a-quick-port-of-testscenarios>`_
   * `Deferring the setup of parametrized resources
     <https://docs.pytest.org/en/stable/example/parametrize.html#deferring-the-setup-of-parametrized-resources>`_
   * `Parametrizing test methods through per-class configuration
     <https://docs.pytest.org/en/stable/example/parametrize.html#parametrizing-test-methods-through-per-class-configuration>`_

Aufschieben der Einrichtung parametrisierter Ressourcen
-------------------------------------------------------

Die Parametrisierung von Testfunktionen erfolgt zum Zeitpunkt der Erfassung. Es
empfiehlt sich daher, aufwändige Ressourcen wie Datenbankverbindungen oder
Unterprozesse erst dann einzurichten, wenn der eigentliche Test ausgeführt wird.
Hier ist ein einfaches Beispiel, wie ihr dies erreichen könnt.

.. code-block:: python
   :caption: test_backends.py

   import pytest


   def test_db_initialised(items_db):
       # An example test
       if items_db.__class__.__name__ == "Sqlite":
           pytest.fail("Deliberately failing for demonstration purposes")

Wir können nun eine Testkonfiguration hinzufügen, die zwei Aufrufe der Funktion
``test_db_initialised`` generiert und außerdem eine Factory implementiert, die
ein Datenbankobjekt für die eigentlichen Testaufrufe erstellt:

.. code-block:: python
   :caption: conftest.py

   import pytest


   def pytest_generate_tests(metafunc):
       if "items_db" in metafunc.fixturenames:
           metafunc.parametrize("items_db", ["json", "sqlite"], indirect=True)


   class Json:
       "JSON object"


   class Sqlite:
       "Sqlite database object"


   @pytest.fixture
   def items_db(request):
       if request.param == "json":
           return Json()
       elif request.param == "sqlite":
           return Sqlite()
       else:
           raise ValueError("Invalid internal test config")

Schauen wir uns zunächst einmal an, wie es zum Zeitpunkt der Einrichtung
aussieht:

.. code-block:: pytest
   :emphasize-lines: 12-13

   $ uv run pytest tests/test_backends.py --collect-only
   ============================= test session starts ==============================
   platform darwin -- Python 3.14.0b4, pytest-8.4.1, pluggy-1.6.0
   rootdir: /Users/veit/sandbox/items
   configfile: pyproject.toml
   plugins: anyio-4.9.0, Faker-37.4.0, cov-6.2.1
   collected 2 items

   <Dir items>
     <Dir tests>
       <Module test_backends.py>
         <Function test_db_initialised[json]>
         <Function test_db_initialised[sqlite]>

   ========================== 2 test collected in 0.01s ===========================

.. code-block:: pytest

   $ uv run pytest -q tests/test_backends.py
   .F                                                                   [100%]
   ================================= FAILURES =================================
   _______________________ test_db_initialised[sqlite] ________________________

   db = <conftest.Sqlite object at 2491125695488>

       def test_db_initialised(items_db):
           # An example test
           if db.__class__.__name__ == "Sqlite":
   >           pytest.fail("Deliberately failing for demo purposes")
   E           Failed: Deliberately failing for demo purposes

   test_backends.py:8: Failed
   ========================= short test summary info ==========================
   FAILED tests/test_backends.py::test_db_initialised[sqlite] - Failed: deli...
   1 failed, 1 passed in 0.03s

Parametrisierte Exceptions
--------------------------

`pytest.raises() <https://docs.pytest.org/en/latest/reference/reference.html#pytest.raises>`_
    kann mit dem Dekorator ``pytest.mark.parametrize`` verwendet werden, um
    parametrisierte Tests zu schreiben, in denen einige Tests Exceptions
    auslösen und andere nicht.

`contextlib.nullcontext <https://docs.python.org/3/library/contextlib.html#contextlib.nullcontext>`_
    kann verwendet werden, um Testfälle zu testen, die voraussichtlich keine
    Ausnahmen auslösen, aber zu einem bestimmten Wert führen sollten. Der Wert
    wird als Parameter ``enter_result`` angegeben, der als Ziel der
    ``with``-Anweisung verfügbar ist.

Beispiel für parametrisierte Exceptions
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: python

   from contextlib import nullcontext

   import pytest


   @pytest.mark.parametrize(
       "divisor, expectation",
       [
           (3, nullcontext(2)),
           (2, nullcontext(3)),
           (1, nullcontext(6)),
           (0, pytest.raises(ZeroDivisionError)),
       ],
   )
   def test_division(divisor, expectation):
       """Test expected division results."""
       with expectation as e:
           assert (6 / divisor) == e

Die ersten drei Testfälle sollten ohne Exceptions ausgeführt werden, während der
vierte, wie von pytest erwartet, eine ``ZeroDivisionError``-Exception auslösen
sollte.

----

.. [#] https://docs.pytest.org/en/latest/reference/reference.html#metafunc
