Testsuite strukturieren
=======================

Stellt sicher, dass die Assertions am Ende von Testfunktionen aufbewahrt werden.
Diese Empfehlung ist so verbreitet, dass sie mindestens zwei Namen hat:

Arrange-Act-Assert (AAA)
    wurde als Teil der :term:`testgetriebenen Entwicklung (TDD) <Testgetriebene
    Entwicklung>` populär.
Given-When-Then (GWT)
    wird im Kontext verhaltensgetriebener Entwicklung (BDD) verwendet.

Die Aufteilung in diese frei Phasen hat viele Vorteile. Dies trennt die Teile

Given/Arrange
   Der Ausgangszustand. Hier richtet ihr Daten oder die Umgebung ein, um die
   Aktion vorzubereiten.
When/Act
    Eine Aktion wird ausgeführt. Dies ist der Schwerpunkt des Tests – das
    Verhalten, von dem wir sicherstellen wollen, dass es richtig funktioniert.
Then/Assert
    Ein erwartetes Ergebnis oder ein Endzustand sollte eintreten. Am Ende des
    Tests stellen wir sicher, dass die Aktion zu dem erwarteten Verhalten
    geführt hat.

Ein häufig anzutreffendes Gegenmuster ist das Muster
*Arrange–Assert–Act–Assert–Act–Assert…*, bei dem eine Vielzahl von Aktionen,
gefolgt von Zustands- oder Verhaltensprüfungen, einen Arbeitsablauf validieren.
Dies erscheint vernünftig, bis der Test fehlschlägt. Jede der Aktionen könnte
den Fehler verursacht haben, so dass sich der Test nicht auf das Testen eines
bestimmten Verhaltens konzentriert. Oder es könnte die Einrichtung in *Anordnen*
gewesen sein, die den Fehler verursacht hat. Dieses verschachtelte
``assert``-Muster führt zu Tests, die schwer zu debuggen und zu warten sind. Das
Festhalten an *Given–When–Then* oder *Arrange–Act–Assert* hält den Test
fokussiert und macht ihn wartungsfreundlicher.

Wenden wir diese Struktur als Beispiel auf einen unserer ersten Tests an:

.. code-block:: python

    def test_equality_fail():
        # Given two item objects with known contents
        i1 = Item("do something", "veit")
        i2 = Item("do something else", "veit.schiele")
        # WHEN the two item objects are not identical
        if i1 != i2:
            # THEN the result will be a string
            pytest.fail("The items are not identical!")

Die Struktur hilft dabei, die Testfunktionen zu organisieren und sich auf das
Testen **eines** Verhaltens zu konzentrieren. Die Struktur hilft euch auch
dabei, an andere Testfälle zu denken. Die Konzentration auf einen
Ausgangszustand hilft euch, an andere Zustände zu denken, die für das Testen der
gleichen Aktion relevant sein könnten. Ebenso hilft die Konzentration auf ein
ideales Ergebnis dabei, an andere mögliche Ergebnisse zu denken, wie :abbr:`z.B.
(zum Beispiel)` Ausfallzustände oder Fehlerzustände, die ebenfalls mit anderen
Testfällen getestet werden sollten.

Tests mit Klassen gruppieren
----------------------------

Bislang haben wir Testfunktionen innerhalb von Testmodulen in einem
Dateisystemverzeichnis geschrieben. Diese Strukturierung des Testcodes
funktioniert eigentlich ganz gut und ist für viele Projekte ausreichend.
pytest erlaubt uns jedoch auch, Tests mit Klassen zu gruppieren. Nehmen wir
einige der Testfunktionen, die sich auf die Gleichheit  der Items beziehen, und
gruppieren sie in einer Klasse:

.. code-block:: python

    class TestEquality:
        def test_equality(self):
            i1 = Item("do something", "veit", "todo", 42)
            i2 = Item("do something", "veit", "todo", 42)
            assert i1 == i2

        def test_equality_with_diff_ids(self):
            i1 = Item("do something", "veit", "todo", 42)
            i2 = Item("do something", "veit", "todo", 43)
            assert i1 == i2

        def test_inequality(self):
            i1 = Item("do something", "veit", "todo", 42)
            i2 = Item("do something else", "veit", "done", 42)
            assert i1 != i2

Der Code sieht so ziemlich genauso aus wie vorher, mit der Ausnahme, dass jede
Methode ein anfängliches ``self``-Argument haben muss. Wir können nun alle diese
Methoden zusammen ausführen, indem wir die Klasse angeben:

.. code-block:: pytest

    $ pytest -v tests/test_classes.py::TestEquality
    ============================= test session starts ==============================
    …
    collected 3 items

    tests/test_classes.py::TestEquality::test_equality PASSED                [ 33%]
    tests/test_classes.py::TestEquality::test_equality_with_diff_ids PASSED  [ 66%]
    tests/test_classes.py::TestEquality::test_inequality PASSED              [100%]

    ============================== 3 passed in 0.00s ===============================

Wir können immer noch zu einer einzigen Methode kommen:

.. code-block:: pytest

    $ pytest -v tests/test_classes.py::TestEquality::test_equality
    ============================= test session starts ==============================
    …
    collected 1 item

    tests/test_classes.py::TestEquality::test_equality PASSED                [100%]

    ============================== 1 passed in 0.00s ===============================

Wenn ihr mit :doc:`/oop/index` und :doc:`Klassenvererbung </oop/inheritance>`
vertraut seid, könnt ihr Hierarchien von Testklassen für vererbte Hilfsmethoden
verwenden. Ich empfehle euch, Testklassen auch in produktivem Testcode nur
sparsam und hauptsächlich zur Gruppierung zu verwenden. Wenn ihr euch mit der
Vererbung von Testklassen zu viel Mühe gebt, wird das zukünftig verwirrend
werden.

Teilmenge von Tests ausführen
-----------------------------

Im vorangegangenen Abschnitt haben wir Testklassen verwendet, um eine Teilmenge
von Tests ausführen zu können. Die Ausführung einer kleinen Gruppe von Tests ist
beim Debuggen sehr praktisch, oder wenn ihr die Tests auf einen bestimmten
Abschnitt der Codebasis beschränken wollt, an dem ihr gerade arbeitet.
pytest erlaubt euch, eine Teilmenge von Tests auf verschiedene Arten
auszuführen:

+-----------------------------------------------+-----------------------------------------------------------------------+
| Teilmenge                                     | Syntax                                                                |
+===============================================+=======================================================================+
| Alle Tests in einem                           | :samp:`pytest {path}`                                                 |
| Verzeichnis                                   |                                                                       |
+-----------------------------------------------+-----------------------------------------------------------------------+
| Alle Tests in einem Modul                     | :samp:`pytest {path}/test_{module}.py`                                |
+-----------------------------------------------+-----------------------------------------------------------------------+
| Alle im Arbeitsverzeichnis                    | :samp:`pytest $(git diff --name-only 'tests/test_*.py')`              |
| eines :doc:`Git                               |                                                                       |
| <Python4DataScience:productive/git/index>`    |                                                                       |
| Repository geänderten Dateien                 |                                                                       |
+-----------------------------------------------+-----------------------------------------------------------------------+
| Alle Tests in einer Klasse                    | :samp:`pytest {path}/test_{module}.py::Test{Class}`                   |
+-----------------------------------------------+-----------------------------------------------------------------------+
| Einzelne Testfunktion                         | :samp:`pytest {path}/test_{module}.py::test_{function}`               |
+-----------------------------------------------+-----------------------------------------------------------------------+
| Einzelne Testmethode                          | :samp:`pytest {path}/test_{module}.py::Test{Class}::test_{method}`    |
+-----------------------------------------------+-----------------------------------------------------------------------+
| Tests, die einem Namensmuster                 | :samp:`pytest -k {pattern}`                                           |
| entsprechen                                   |                                                                       |
+-----------------------------------------------+-----------------------------------------------------------------------+
| Tests nach Marker                             | siehe :doc:`markers`                                                  |
+-----------------------------------------------+-----------------------------------------------------------------------+

Ob ``pytest`` euren Testcode findet, hängt von der Namensgebung ab:

* Testdateien sollten :samp:`test_{something}.py` oder
  :samp:`{something}_test.py`.
* Testmethoden und Funktionen sollten :samp:`test_{SOMETHING}` genannt werden.
* Testklassen sollten den Namen :samp:`Test{Something}` tragen.

.. tip::
   Verwendet eine Verzeichnisstruktur, die der Art und Weise entspricht, wie ihr
   euren Code ausführen möchtet, denn es ist einfach, ein komplettes
   Unterverzeichnis auszuführen. So könnt ihr Features und Funktionen
   unterteilen oder Subsysteme als Grundlage nehmen oder euch an der
   Code-Struktur orientieren.

Ihr könnt auch :samp:`-k {pattern}` verwenden, um Verzeichnisse, Klassen oder
Testpräfixe zu filtern, also :abbr:`z.B. (zum Beispiel)` alle Tests der Klasse
``TestEquality``

.. code-block:: pytest

    $ pytest -v -k TestEquality
    ============================= test session starts ==============================
    …
    collected 7 items / 4 deselected / 3 selected

    test_classes.py::TestEquality::test_equality PASSED                      [ 33%]
    test_classes.py::TestEquality::test_equality_with_diff_ids PASSED        [ 66%]
    test_classes.py::TestEquality::test_inequality PASSED                    [100%]

    ======================= 3 passed, 4 deselected in 0.00s ========================

oder alle Tests mit ``equality`` im Namen:

.. code-block:: pytest

    pytest -v --tb=no -k equality
    ============================= test session starts ==============================
    …
    collected 7 items / 3 deselected / 4 selected

    test_classes.py::TestEquality::test_equality PASSED                      [ 25%]
    test_classes.py::TestEquality::test_equality_with_diff_ids PASSED        [ 50%]
    test_classes.py::TestEquality::test_inequality PASSED                    [ 75%]
    test_item_fail.py::test_equality_fail FAILED                             [100%]

    =========================== short test summary info ============================
    FAILED test_item_fail.py::test_equality_fail - Failed: The items are not identical!
    ================== 1 failed, 3 passed, 3 deselected in 0.01s ===================

Eines davon ist leider unser Fehlerbeispiel. Wir können es beseitigen, indem wir
den Ausdruck erweitern:

.. code-block:: pytest

    $ pytest -v --tb=no -k "equality and not equality_fail"
    ============================= test session starts ==============================
    …
    collected 7 items / 4 deselected / 3 selected

    test_classes.py::TestEquality::test_equality PASSED                      [ 33%]
    test_classes.py::TestEquality::test_equality_with_diff_ids PASSED        [ 66%]
    test_classes.py::TestEquality::test_inequality PASSED                    [100%]

    ======================= 3 passed, 4 deselected in 0.00s ========================

Die Schlüsselwörter ``and``, ``not``, ``or`` und ``()`` sind erlaubt, um
komplexe Ausdrücke zu erstellen. Hier ist ein Testlauf aller Tests mit oder "ids" im Namen, aber nicht in der Klasse "TestEquality":

.. code-block:: pytest

    $ pytest -v --tb=no -k "(inequality or id) and not _fail"
    ============================= test session starts ==============================
    …
    collected 7 items / 4 deselected / 3 selected

    test_classes.py::TestEquality::test_equality_with_diff_ids PASSED        [ 33%]
    test_classes.py::TestEquality::test_inequality PASSED                    [ 66%]
    test_helper.py::test_ident PASSED                                        [100%]

    ======================= 3 passed, 4 deselected in 0.00s ========================

.. _keyword:

Die Keyword-Option ``-k`` bietet zusammen mit ``and``, ``not`` und ``or`` eine
große Flexibilität bei der Auswahl der Tests, die ihr ausführen möchtet. Dies
erweist sich bei der Fehlersuche oder der Entwicklung neuer Tests als sehr
hilfreich.

.. tip::
   Es ist eine gute Idee, Anführungszeichen zu verwenden, wenn ihr einen
   Test zur Ausführung auswählt, da die Bindestriche, Klammern und Leerzeichen
   die Shells durcheinander bringen können.
