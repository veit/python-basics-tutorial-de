Test-Funktionen schreiben
=========================

``assert``-Anweisungen
----------------------

Wenn ihr Testfunktionen schreibt, ist die normale pytest-``assert``-Anweisung
euer wichtigstes Werkzeug. Die Einfachheit dieser Anweisung bringt viele
Entwickler dazu, pytest gegenüber anderen Frameworks zu bevorzugen. Im Folgenden
findet ihr eine Liste einiger ``assert``-Formen und ``assert``-Hilfsfunktionen
von :doc:`../unittest`:

+-------------------------------+---------------------------------------+
| pytest                        | unittest                              |
+===============================+=======================================+
| :samp:`assert {something}`    | :samp:`assertTrue({something})`       |
+-------------------------------+---------------------------------------+
| :samp:`assert not {something}`| :samp:`assertFalse({something})`      |
+-------------------------------+---------------------------------------+
| :samp:`assert {x} == {y}`     | :samp:`assertEqual({x}, {y})`         |
+-------------------------------+---------------------------------------+
| :samp:`assert {x} != {y}`     | :samp:`assertNotEqual({x}, {y})`      |
+-------------------------------+---------------------------------------+
| :samp:`assert {x} <= {y}`     | :samp:`assertLessEqual({x}, {y})`     |
+-------------------------------+---------------------------------------+
| :samp:`assert {x} is None`    | :samp:`assertIsNone({x})`             |
+-------------------------------+---------------------------------------+
| :samp:`assert {x} is not None`| :samp:`assertIsNotNone({x})`          |
+-------------------------------+---------------------------------------+

Mit pytest könnt ihr :samp:`assert {AUSDRUCK}` mit einem beliebigen Ausdruck
verwenden. Wenn der Ausdruck bei einer Konvertierung in einen booleschen Wert zu
``False`` ausgewertet würde, würde der Test fehlschlagen.

pytest enthält eine Funktion namens ``assert rewriting``, die ``assert``-Aufrufe
abfängt und sie durch etwas ersetzt, das euch mehr darüber sagen kann, warum
eure Annahmen fehlgeschlagen sind. Sehen wir uns an, wie hilfreich dieses
Rewriting ist, indem wir uns einen fehlgeschlagenen ``assert``-Test ansehen:

.. code-block:: python

    def test_equality_fails():
        i1 = Item("do something", "veit")
        i2 = Item("do something else", "veit")
        assert i1 == i2

Dieser Test schlägt fehl, aber interessant sind die Traceback-Informationen:

.. code-block:: pytest

    $ pytest tests/test_item_fails.py
    ============================= test session starts ==============================
    …
    collected 1 item

    tests/test_item_fails.py F                                               [100%]

    =================================== FAILURES ===================================
    _____________________________ test_equality_fails ______________________________

        def test_equality_fails():
            i1 = Item("do something", "veit")
            i2 = Item("do something else", "veit.schiele")
    >       assert i1 == i2
    E       AssertionError: assert Item(summary=...odo', id=None) == Item(summary=...odo', id=None)
    E
    E         Omitting 1 identical items, use -vv to show
    E         Differing attributes:
    E         ['summary', 'owner']
    E
    E         Drill down into differing attribute summary:
    E           summary: 'do something' != 'do something else'...
    E
    E         ...Full output truncated (8 lines hidden), use '-vv' to show

    tests/test_item_fails.py:7: AssertionError
    =========================== short test summary info ============================
    FAILED tests/test_item_fails.py::test_equality_fails - AssertionError: assert Item(summary=...odo', id=None) == Item(summary=...od...
    ============================== 1 failed in 0.03s ===============================

Das sind eine Menge Informationen:

Für jeden fehlgeschlagenen Test wird die genaue Zeile des Fehlers mit einem
``>`` angezeigt, das auf den Fehler verweist.

Die ``E``-Zeilen zeigen Ihnen zusätzliche Informationen über den
``assert``-Fehler, damit ihr herausfinden könnt, was falsch gelaufen ist.
Ich habe absichtlich zwei Fehlanpassungen in ``test_equality_fails()``
eingegeben, aber nur die erste wurde angezeigt. Versuchen wir es noch einmal mit
der ``-vv``-Option, wie in der Fehlermeldung vorgeschlagen:

.. code-block:: pytest

    $ pytest -vv tests/test_item_fails.py
    ============================= test session starts ==============================
    …
    collected 1 item

    tests/test_item_fails.py::test_equality_fails FAILED                     [100%]

    =================================== FAILURES ===================================
    _____________________________ test_equality_fails ______________________________

        def test_equality_fails():
            i1 = Item("do something", "veit")
            i2 = Item("do something else", "veit.schiele")
    >       assert i1 == i2
    E       AssertionError: assert Item(summary='do something', owner='veit', state='todo', id=None) == Item(summary='do something else', owner='veit.schiele', state='todo', id=None)
    E
    E         Matching attributes:
    E         ['state']
    E         Differing attributes:
    E         ['summary', 'owner']
    E
    E         Drill down into differing attribute summary:
    E           summary: 'do something' != 'do something else'
    E           - do something else
    E           ?             -----
    E           + do something
    E
    E         Drill down into differing attribute owner:
    E           owner: 'veit' != 'veit.schiele'
    E           - veit.schiele
    E           + veit

    tests/test_item_fails.py:7: AssertionError
    =========================== short test summary info ============================
    FAILED tests/test_item_fails.py::test_equality_fails - AssertionError: assert Item(summary='do something', owner='veit', state='to...
    ============================== 1 failed in 0.03s ===============================

pytest hat genau aufgelistet, welche Attribute übereinstimmen und welche nicht.
Zudem wurden die genauen Abweichungen hervorgehoben.

Zum Vergleich können wir uns anzeigen lassen, was Python bei ``assert``-Fehlern
anzeigt. Um den Test direkt von Python aus aufrufen zu können, müssen wir einen Block am Ende von :file:`tests/test_item_fails.py` einfügen:

.. code-block:: python

    if __name__ == "__main__":
        test_equality_fails()

Wenn wir den Test nun mit Python durchführen, erhalten wir folgendes Ergebnis:

.. code-block:: python

    python tests/test_item_fails.py
    Traceback (most recent call last):
      File "tests/test_item_fails.py", line 11, in <module>
        test_equality_fails()
      File "tests/test_item_fails.py", line 7, in test_equality_fails
        assert i1 == i2
               ^^^^^^^^
    AssertionError

Das sagt uns nicht viel. Die pytest-Ausgabe gibt uns viel mehr Informationen
darüber, warum unsere Annahmen fehlgeschlagen sind.

Fehlschlagen mit ``pytest.fail()`` und Exceptions
-------------------------------------------------

Das Fehlschlagen von Behauptungen ist die Hauptursache dafür, dass Tests
fehlgeschlagen. Aber das ist nicht der einzige Weg. Ein Test schlägt auch fehl,
wenn es eine nicht abgefangene :doc:`/control-flows/exceptions` gibt. Das kann
passieren, wenn

* eine ``assert``-Anweisung fehlschlägt, was zu einer
  ``AssertionError``-Exception führt,
* der Testcode ``pytest.fail()`` aufruft, was zu einer Exception führt, oder
* eine andere Exception ausgelöst wird.

Obwohl jede Exception einen Test fehlschlagen lassen kann, ziehe ich es vor,
``assert`` zu verwenden. In seltenen Fällen, in denen ``assert`` nicht geeignet
ist, verwende ich meist ``pytest.fail()``.

Hier ist ein Beispiel für die Verwendung der Funktion ``fail()`` von pytest, um
einen Test explizit fehlschlagen zu lassen:

.. code-block:: python

    def test_with_fail():
        i1 = Item("do something", "veit")
        i2 = Item("do something else", "veit.schiele")
        if i1 != i2:
            pytest.fail("The items are not identical!")

Die Ausgabe sieht wie folgt aus:

.. code-block:: pytest

    pytest tests/test_item_fails.py
    ============================= test session starts ==============================
    …
    collected 1 item

    tests/test_item_fails.py F                                               [100%]

    =================================== FAILURES ===================================
    ________________________________ test_with_fail ________________________________

        def test_with_fail():
            i1 = Item("do something", "veit")
            i2 = Item("do something else", "veit.schiele")
            if i1 != i2:
    >           pytest.fail("The items are not identical!")
    E           Failed: The items are not identical!

    tests/test_item_fails.py:10: Failed
    =========================== short test summary info ============================
    FAILED tests/test_item_fails.py::test_with_fail - Failed: The items are not identical!
    ============================== 1 failed in 0.03s ===============================

Beim Aufruf von ``pytest.fail()`` oder dem Auslösen einer Exception, erhalten
wir nicht das von pytest angebotene ``assert``-Rewriting. Es gibt jedoch
sinnvolle Gelegenheiten, ``pytest.fail()`` zu verwenden, wie :abbr:`z.B. (zum
Beispiel)` in einem ``assertion``-Hilfsprogramm.

Schreiben von ``assertion``-Hilfsfunktionen
-------------------------------------------

Eine ``assertion``-Hilfsfunktion dient dazu, eine komplizierte
``assertion``-Prüfung zu verpacken. Ein Beispiel: Die Datenklasse ``Item``
ist so eingerichtet, dass zwei Items mit unterschiedlichen IDs trotzdem
Gleichheit berichten. Wenn wir eine strengere Prüfung wünschen, könnten wir eine
Hilfsfunktion namens ``assert_ident`` wie folgt schreiben:

.. code-block:: python

    import pytest

    from items import Item


    def assert_ident(i1: Item, i2: Item):
        __tracebackhide__ = True
        assert i1 == i2
        if i1.id != i2.id:
            pytest.fail(f"The IDs do not match: {i1.id} != {i2.id}")


    def test_ident():
        i1 = Item("something to do", id=42)
        i2 = Item("something to do", id=42)
        assert_ident(i1, i2)


    def test_ident_fail():
        i1 = Item("something to do", id=42)
        i2 = Item("something to do", id=43)
        assert_ident(i1, i2)

Die ``assert_ident``-Funktion setzt ``__tracebackhide__ = True``. Die Folge ist,
dass fehlgeschlagene Tests nicht in den Traceback aufgenommen werden. Das
normale ``assert i1 == i2`` wird dann verwendet, um alles außer ``id`` auf
Gleichheit zu prüfen.

Schließlich werden die IDs überprüft ``pytest.fail()`` verwendet, um den Test
mit einer hilfreichen Meldung fehlschlagen zu lassen. Schauen wir uns an, wie
das nach der Ausführung aussieht:

.. code-block:: pytest

    $ pytest tests/test_helper.py
    ============================= test session starts ==============================
    …
    collected 2 items

    tests/test_helper.py .F                                                  [100%]

    =================================== FAILURES ===================================
    _______________________________ test_ident_fail ________________________________

        def test_ident_fail():
            i1 = Item("something to do", id=42)
            i2 = Item("something to do", id=43)
    >       assert_ident(i1, i2)
    E       Failed: The IDs do not match: 42 != 43

    tests/test_helper.py:22: Failed
    =========================== short test summary info ============================
    FAILED tests/test_helper.py::test_ident_fail - Failed: The IDs do not match: 42 != 43
    ========================= 1 failed, 1 passed in 0.03s ==========================

Testen auf erwartete Exceptions
-------------------------------

Wir haben uns angesehen, wie jede Exception einen Test zum Scheitern bringen
kann. Was aber, wenn ein Teil des Codes, den wir testen, eine Exception auslösen
soll? Hierfür verwenden wir ``pytest.raises()``, um auf erwartete Exceptions zu
testen. Ein Beispiel hierfür wäre die Items-API, die eine ``ItemsDB``-Klasse
hat, die ein Pfadargument benötigt.

.. code-block:: python

    from items.api import ItemsDB


    def test_db_exists():
        ItemsDB()

.. code-block:: pytest

    $ pytest --tb=short tests/test_db.py
    ============================= test session starts ==============================
    …
    collected 1 item

    tests/test_db.py F                                                       [100%]

    =================================== FAILURES ===================================
    ________________________________ test_db_exists ________________________________
    tests/test_db.py:5: in test_db_exists
        ItemsDB()
    E   TypeError: ItemsDB.__init__() missing 1 required positional argument: 'db_path'
    =========================== short test summary info ============================
    FAILED tests/test_db.py::test_db_exists - TypeError: ItemsDB.__init__() missing 1 required positional argument: 'db_p...
    ============================== 1 failed in 0.03s ===============================

Hier habe ich das kürzere Traceback-Format ``--tb=short`` verwendet, weil wir
nicht den vollständigen Traceback sehen müssen, um herauszufinden, welche
Exception ausgelöst wurde.

Die Exception ``TypeError`` erscheint sinnvoll, da der Fehler beim Versuch
auftritt, den benutzerdefinierten ItemsDB-Typ zu initialisieren. Wir können
einen Test schreiben, um sicherzustellen, dass diese Exception ausgelöst wird,
etwa so:

.. code-block:: python

    import pytest

    from items.api import ItemsDB


    def test_db_exists():
        with pytest.raises(TypeError):
            ItemsDB()

Die Anweisung ``with pytest.raises(TypeError):`` besagt, dass der nächste
Codeblock eine ``TypeError``-Exception auslösen soll. Wenn keine Ausnahme
ausgelöst wird oder eine andere Ausnahme ausgelöst wird, schlägt der Test fehl.

Wir haben gerade in ``test_db_exists()`` den Typ der Exception überprüft. Wir
können auch überprüfen, ob die Meldung korrekt ist, oder jeden anderen Aspekt
der Exception, wie :abbr:`z.B. (zum Beispiel)` zusätzliche Parameter:

.. code-block:: python

    def test_db_exists():
        match_regex = "missing 1 .* positional argument"
        with pytest.raises(TypeError, match=match_regex):
            ItemsDB()


oder

.. code-block:: python

    def test_db_exists():
        with pytest.raises(TypeError) as exc_info:
            ItemsDB()
        expected = "missing 1 required positional argument"
        assert expected in str(exc_info.value)
