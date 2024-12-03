Markers
=======

Marker in pytest kann man sich wie Tags oder Etiketten vorstellen. Wenn einige
Tests langsam sind, könnt ihr sie mit ``@pytest.mark.slow`` markieren und pytest
diese Tests überspringen lassen, wenn ihr in Eile seid. Ihr könnt eine Handvoll
Tests aus einer Testsuite auswählen und sie mit ``@pytest.mark.smoke`` markieren
und diese als erste Stufe einer Testpipeline in einem :term:`CI`-System
ausführen. Ihr könnt wirklich für jeden Grund, den ihr habt, um nur einige Tests
auszuführen, Marker verwenden.

pytest enthält eine Handvoll Built-in-Marker, die das Verhalten der
Testausführung verändern. Eine davon, ``@pytest.mark.parametrize``, haben wir
bereits in :ref:`parameterise-functions` verwendet. Zusätzlich zu den
benutzerdefinierten Markierungen, die wir erstellen und zu unseren Tests
hinzufügen können, weisen die Built-in-Marker pytest an, etwas Besonderes mit
den markierten Tests zu tun.

Im Folgenden werden wir beide Arten von Markern genauer untersuchen: die
Built-in-Marker, die das Verhalten ändern, und die benutzerdefinierten Marker,
die wir erstellen können, um auszuwählen, welche Tests ausgeführt werden sollen.
Wir können Marker auch verwenden, um Informationen an eine Fixture zu übergeben,
die von einem Test verwendet wird.

Built-in-Markers verwenden
--------------------------

Die Built-in-Markers von pytest werden verwendet, um die Testausführung zu
verändern. Hier ist die vollständige Liste der Built-in-Markers, die in pytest
enthalten sind:

:samp:`@pytest.mark.filterwarnings({WARNUNG})`
    Dieser Marker fügt dem angegebenen Test einen Warnfilter hinzu.
:samp:`@pytest.mark.skip(reason={None})`
    Mit diesem Marker wird der Test mit einem optionalen Grund übersprungen.
:samp:`@pytest.mark.skipif({BEDINGUNG}, ...*, {GRUND})`
    Diese Markierung überspringt den Test, wenn eine der Bedingungen True ist.
:samp:`@pytest.mark.xfail({BEDINGUNG}, ...* {GRUND}, run={True}, raises={None}, strict={xfail_strict})`
    Dieser Marker teilt pytest mit, dass wir das Fehlschlagen erwarten.
:samp:`@pytest.mark.parametrize({ARG1, ARG2, ...`
    Dieser Marker ruft eine Testfunktion mehrfach auf, wobei nacheinander
    verschiedene Argumente übergeben werden.
:samp:`@pytest.mark.usefixtures({FIXTURE1, FIXTURE2, ...`
    Dieser Marker kennzeichnet Tests, die alle angegebenen Fixtures benötigen.

:doc:`@pytest.mark.parametrize <params>` haben wir bereits verwendet. Lasst uns
die drei anderen, am häufigsten verwendeten Built-in-Markers mit einigen
Beispielen durchgehen, um zu sehen, wie sie funktionieren.

Überspringen von Tests mit ``@pytest.mark.skip``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Der Marker ``skip`` erlaubt es uns, einen Test zu überspringen. Nehmen wir an,
wir wollen in einer zukünftigen Version der Items-Anwendung die Möglichkeit zum
Sortieren hinzufügen und möchten, dass die ``Item``-Klasse Vergleiche
unterstützt. Wir schreiben einen Test für den Vergleich von ``Item``-Objekten
mit ``<`` wie folgt:

.. code-block:: python

    from items import Item


    def test_less_than():
        i1 = Item("Update pytest section")
        i2 = Item("Update cibuildwheel section")
        assert i1 < i2


    def test_equality():
        i1 = Item("Update pytest section")
        i2 = Item("Update pytest section")
        assert i1 == i2

Und er scheitert:

.. code-block:: pytest

    pytest --tb=short tests/test_compare.py
    ============================= test session starts ==============================
    ...
    collected 2 items

    tests/test_compare.py F.                                                 [100%]

    =================================== FAILURES ===================================
    ________________________________ test_less_than ________________________________
    tests/test_compare.py:7: in test_less_than
        assert i1 < i2
    E   TypeError: '<' not supported between instances of 'Item' and 'Item'
    =========================== short test summary info ============================
    FAILED tests/test_compare.py::test_less_than - TypeError: '<' not supported between instances of 'Item' and 'Item'
    ========================= 1 failed, 1 passed in 0.03s ==========================

Der Fehler liegt einfach daran, dass wir diese Funktion noch nicht implementiert
haben. Dennoch müssen wir diesen Test nicht wieder wegwerfen; wir können ihn
einfach auslassen:

.. code-block:: python
   :emphasize-lines: 1, 6

    import pytest

    from items import Item


    @pytest.mark.skip(reason="Items do not yet allow a < comparison")
    def test_less_than():
        i1 = Item("Update pytest section")
        i2 = Item("Update cibuildwheel section")
        assert i1 < i2

Der Marker ``@pytest.mark.skip()`` weist pytest an, den Test zu überspringen.
Die Angabe eines Grundes ist zwar optional, aber sie hilft bei der weiteren
Entwicklung.  Wenn wir übersprungene Tests ausführen, werden sie als ``s``
angezeigt:

.. code-block::
   :emphasize-lines: 6

    $ pytest --tb=short tests/test_compare.py
    ============================= test session starts ==============================
    ...
    collected 2 items

    tests/test_compare.py s.                                                 [100%]

    ========================= 1 passed, 1 skipped in 0.00s =========================

… oder verbos als ``SKIPPED``:

.. code-block::
   :emphasize-lines: 1, 10

    $ pytest -v -ra tests/test_compare.py
    ============================= test session starts ==============================
    ...
    collected 2 items

    tests/test_compare.py::test_less_than SKIPPED (Items do not yet allo...) [ 50%]
    tests/test_compare.py::test_equality PASSED                              [100%]

    =========================== short test summary info ============================
    SKIPPED [1] tests/test_compare.py:6: Items do not yet allow a < comparison
    ========================= 1 passed, 1 skipped in 0.00s =========================

Da wir pytest mit ``-r`` angewiesen haben, eine kurze Zusammenfassung unserer
Tests auszugeben, erhalten wir eine zusätzliche Zeile am unteren Ende, die den
Grund auflistet, den wir im Marker angegeben haben. Das ``a`` in ``-ra`` steht
für *all except passed*. Die Optionen ``-ra`` sind die gebräuchlichsten, da wir
fast immer wissen wollen, warum bestimmte Tests nicht bestanden haben.

.. seealso::
   * `Skipping test functions
     <https://docs.pytest.org/en/latest/how-to/skipping.html#skipping-test-functions>`_

Bedingtes Überspringen von Tests mit ``@pytest.mark.skipif``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Angenommen, wir wissen, dass wir die Sortierung in den Versionen 0.1.x der App
Items nicht unterstützen werden, wohl aber in Version 0.2.x. Dann können wir
pytest anweisen, den Test für alle Versionen von Items, die kleiner als 0.2.x
sind, wie folgt zu überspringen:

.. code-block:: python
   :emphasize-lines: 2, 4, 8-11

    import pytest
    from packaging.version import parse

    import items
    from items import Item


    @pytest.mark.skipif(
        parse(items.__version__).minor < 2,
        reason="The comparison with < is not yet supported in version 0.1.x.",
    )
    def test_less_than():
        i1 = Item("Update pytest section")
        i2 = Item("Update cibuildwheel section")
        assert i1 < i2

Mit dem ``skipif``-Marker könnt ihr beliebig viele Bedingungen eingeben, und
wenn eine davon wahr ist, wird der Test übersprungen. In unserem Fall verwenden
wir ``packaging.version.parse``, um die Minor-Version zu isolieren und sie mit
der Zahl 2 zu vergleichen.

In diesem Beispiel wird als zusätzliches Paket `packaging
<https://pypi.org/project/packaging/>`_ verwendet. Wenn ihr das Beispiel
ausprobieren möchtet, installiert es zunächst mit ``python -m pip install
packaging``.

.. tip::
   ``skipif`` ist auch hervorragend geeignet, wenn Tests für verschiedene
   Betriebssysteme unterschiedlich geschrieben werden müssen.

.. seealso::
   * `skipif <https://docs.pytest.org/en/latest/how-to/skipping.html#id1>`_

``@pytest.mark.xfail``
~~~~~~~~~~~~~~~~~~~~~~

Wenn wir alle Tests durchführen wollen, auch die, von denen wir wissen, dass sie
fehlschlagen werden, können wir den Marker ``xfail`` oder genauer
:samp:`@pytest.mark.xfail({CONDITION}, {... *, {REASON}, run={True},
raises={None}, strict={True})` verwenden. Der erste Satz von Parametern
für diese Fixture ist der gleiche wie bei ``skipif``.

``run``
    Der Test wird standardmäßig ausgeführt, außer wenn ``run=False`` gesetzt
    ist.
``raises``
    erlaubt euch, einen Ausnahmetyp oder ein Tupel von Ausnahmetypen anzugeben,
    die zu einem ``xfail`` führen sollen. Jede andere Ausnahme führt dazu, dass
    der Test fehlschlägt.
``strict``
    teilt pytest mit, ob bestandene Tests ``(strict=False)`` als ``XPASS`` oder
    mit ``strict=True`` als ``FAIL`` markiert werden sollen.

Schauen wir uns ein Beispiel an:

.. code-block:: python
   :emphasize-lines: 8-15, 17-21, 23-

    import pytest
    from packaging.version import parse

    import items
    from items import Item


    @pytest.mark.xfail(
        parse(items.__version__).minor < 2,
        reason="The comparison with < is not yet supported in version 0.1.x.",
    )
    def test_less_than():
        i1 = Item("Update pytest section")
        i2 = Item("Update cibuildwheel section")
        assert i1 < i2


    @pytest.mark.xfail(reason="Feature #17: not implemented yet")
    def test_xpass():
        i1 = Item("Update pytest section")
        i2 = Item("Update pytest section")
        assert i1 == i2


    @pytest.mark.xfail(reason="Feature #17: not implemented yet", strict=True)
    def test_xfail_strict():
        i1 = Item("Update pytest section")
        i2 = Item("Update pytest section")
        assert i1 == i2

Wir haben hier drei Tests: einen, von dem wir wissen, dass er fehlschlägt, und
zwei, von denen wir wissen, dass sie bestanden wird. Diese Tests demonstrieren
sowohl das Scheitern als auch das Bestehen der Verwendung von ``xfail`` und die
Auswirkungen der Verwendung von ``strict``. Das erste Beispiel verwendet auch
den optionalen Parameter ``condition``, der wie die Bedingungen von ``skipif``
funktioniert. Und so sieht das Ergebnis aus:

.. code-block::

    pytest -v -ra tests/test_xfail.py
    ============================= test session starts ==============================
    ...
    collected 3 items

    tests/test_xfail.py::test_less_than XFAIL (The comparison with < is ...) [ 33%]
    tests/test_xfail.py::test_xpass XPASS (Feature #17: not implemented yet) [ 66%]
    tests/test_xfail.py::test_xfail_strict FAILED                            [100%]

    =================================== FAILURES ===================================
    ______________________________ test_xfail_strict _______________________________
    [XPASS(strict)] Feature #17: not implemented yet
    =========================== short test summary info ============================
    XFAIL tests/test_xfail.py::test_less_than - The comparison with < is not yet supported in version 0.1.x.
    XPASS tests/test_xfail.py::test_xpass Feature #17: not implemented yet
    FAILED tests/test_xfail.py::test_xfail_strict
    =================== 1 failed, 1 xfailed, 1 xpassed in 0.02s ====================

Tests, die mit ``xfail`` gekennzeichnet sind:

- Nicht bestandene Tests werden mit ``XFAIL`` angezeigt.
- Bestandene Tests mit  ``strict=False`` führen zu ``XPASSED``.
- Bestandene Tests mit ``strict=True`` führen zu ``FAILED``.

Wenn ein Test fehlschlägt, der mit ``xfail`` markiert ist, also mit ``XFAIL``
ausgegeben wird, hatten wir Recht in der Annahme, dass der Test fehlschlagen
wird.

Bei Tests, die mit ``xfail`` markiert wurden, jedoch tatsächlich bestanden
wurden, gibt es zwei Möglichkeiten: Wenn sie zu ``XFAIL`` führen sollen, dann
solltet ihr die Finger von ``strict`` lassen. Wenn sie hingegen ``FAILED``
ausgeben sollen, dann setzt ``strict``.  Ihr könnt ``strict`` entweder als
Option für den ``xfail``-Marker setzen, wie wir es in diesem Beispiel getan
haben, oder ihr könnt es auch global mit der Einstellung ``xfail_strict=True``
in der pytest-Konfigurationsdatei :file:`pytest.ini` setzen.

Ein pragmatischer Grund, immer ``xfail_strict=True`` zu verwenden, ist, dass wir
uns alle fehlgeschlagenen Tests üblicherweise genauer anzuschauen. Und so sehen
wir uns dann auch die Fälle an, in denen die Erwartungen an den Test nicht mit
dem Ergebnis übereinstimmen.

``xfail`` kann sehr hilfreich sein wenn ihr in einer testgetriebenen Entwicklung
arbeitet und ihr Testfälle schreibt, von denen ihr wisst, dass sie noch nicht
implementiert sind, die ihr aber in Kürze implementieren wollt. Lasst dabei die
``xfail``-Tests auf dem Feature-Branch, in dem die Funktion implementiert wird.

Oder etwas geht kaputt, ein oder mehrere Test schlagen fehl, und ihr könnt nicht
sofort sofort an der Behebung arbeiten. Das Markieren der Tests als ``xfail``,
``strict=true`` mit der Angabe der Fehler-/Issue-Report-ID in ``reason``, ist
eine gute Möglichkeit, den Test weiterlaufen zu lassen und ihn nicht zu
vergessen.

Wenn ihr jedoch nur ein Brainstorming über die Behaviors eurer Anwendung macht,
solltet ihr noch keine Tests schreiben und sie mit ``xfail`` oder ``skip``
markieren: hier würde ich euch :abbr:`YAGNI (‘You Aren’t Gonna Need It’,
deutsch: „Du wirst es nicht brauchen“)` entgegenhalten. Implementiert Dinge
immer erst dann, wenn sie tatsächlich gebraucht werden und niemals, wenn ihr nur
ahnt, dass ihr sie brauchen werdet.

.. tip::
   * Ihr solltet :samp:`xfail_strict = True` in :file:`pytest.ini` setzen, um
     alle ``XPASSED``-Ergebnisse in ``FAILED`` zu verwandeln.
   * Zudem solltet ihr immer :samp:`-ra` oder zumindest :samp:`-rxX` verwenden
     um euch den Grund anzeigen zu lassen.
   * Und schließlich solltet ihr eine Fehlernummer in ``reason`` angeben.
   * ``pytest --runxfail`` ignoriert grundsätzlich die ``xfail``-Marker. Dies
     ist sehr nützlich in den letzten Phasen des Pre-Production-Testing.

.. _select-tests-with-markers:

Auswahl von Tests mit eigenen Markern
-------------------------------------

Eigene Marker könnt ihr euch wie Tags oder Etiketten vorstellen. Sie können
verwendet werden, um Tests auszuwählen, die ausgeführt oder übersprungen werden
sollen.

Nehmen wir an, wir wollen einige unserer Tests mit ``smoke`` kennzeichnen. Die
Segmentierung einer Teilmenge von Tests in eine Smoke-Test-Suite ist eine
gängige Praxis, um einen repräsentativen Satz von Tests ausführen zu können, der
uns schnell sagen kann, ob irgendetwas mit einem der Hauptsysteme nicht in
Ordnung ist. Darüber hinaus werden wir einige unserer Tests mit ``exception``
kennzeichnen – diejenigen, die auf erwartete Ausnahmen prüfen:

.. code-block:: python
   :emphasize-lines: 6

    import pytest

    from items import InvalidItemId, Item


    @pytest.mark.smoke
    def test_start(items_db):
        """
        Change state from ‘todo’ to ‘in progress’
        """
        i = items_db.add_item(Item("Update pytest section", state="todo"))
        items_db.start(i)
        s = items_db.get_item(i)
        assert s.state == "in progress"

Jetzt sollten wir in der Lage sein, nur diesen Test auszuwählen, indem wir die
Option ``-m smoke`` verwenden:

.. code-block:: pytest

    $ pytest -v -m smoke tests/test_start.py
    ============================= test session starts ==============================
    ...
    collected 2 items / 1 deselected / 1 selected

    tests/test_start.py::test_start PASSED                                   [100%]

    =============================== warnings summary ===============================
    tests/test_start.py:6
      /Users/veit/items/tests/test_start.py:6: PytestUnknownMarkWarning: Unknown pytest.mark.smoke - is this a typo?  You can register custom marks to avoid this warning - for details, see https://docs.pytest.org/en/stable/how-to/mark.html
        @pytest.mark.smoke

    -- Docs: https://docs.pytest.org/en/stable/how-to/capture-warnings.html
    ================== 1 passed, 1 deselected, 1 warning in 0.00s ==================

Nun konnten wir zwar nur einen Test durchzuführen, aber wir haben auch eine
Warnung erhalten: ``PytestUnknownMarkWarning: Unknown pytest.mark.smoke - is this a typo?`` Sie hilft, Tippfehler zu vermeiden. pytest möchte, dass wir
benutzerdefinierte Marker registrieren, indem wir einen Marker-Abschnitt zu
:file:`pytest.ini` hinzufügen, :abbr:`z.B. (zum Beispiel)`:

.. code-block:: ini

    [pytest]
    markers =
        smoke: Small subset of all tests

Jetzt warnt uns pytest nicht mehr vor einem unbekannten Marker:

.. code-block::
   :emphasize-lines: 4

    $ pytest -v -m smoke tests/test_start.py
    ============================= test session starts ==============================
    ...
    configfile: pytest.ini
    collected 2 items / 1 deselected / 1 selected

    tests/test_start.py::test_start PASSED                                   [100%]

    ======================= 1 passed, 1 deselected in 0.00s ========================

Machen wir dasselbe mit der ``exception``-Markierung für
``test_start_non_existent``.

#. Zuerst registrieren wir den Marker in :file:`pytest.ini`:

   .. code-block:: ini
      :emphasize-lines: 4

      [pytest]
      markers =
          smoke: Small subset of tests
          exception: Only run expected exceptions

#. Dann fügen wir den Marker zum Test hinzu:

   .. code-block:: python
      :emphasize-lines: 1

      @pytest.mark.exception
      def test_start_non_existent(items_db):
          """
          Shouldn’t start a non-existent item.
          """
          # any_number will be invalid, db is empty
          any_number = 44

          with pytest.raises(InvalidItemId):
              items_db.start(any_number)

#. Schließlich führen wir den Test mit ``-m exception`` aus:

   .. code-block:: pytest

      $ pytest -v -m exception tests/test_start.py
      ============================= test session starts ==============================
      ...
      configfile: pytest.ini
      collected 2 items / 1 deselected / 1 selected

      tests/test_start.py::test_start_non_existent PASSED                      [100%]

      ======================= 1 passed, 1 deselected in 0.01s ========================

Marker für Dateien, Klassen und Parameter
-----------------------------------------

Mit den Tests in :file:`test_start.py` haben wir
:samp:`@pytest.mark.{MARKER_NAME}`-Dekoratoren zu Testfunktionen hinzugefügt.
Wir können auch ganze Dateien oder Klassen mit Markern versehen, um mehrere
Tests zu markieren, oder in parametrisierte Tests hineingehen und einzelne
Parametrisierungen markieren. Wir können sogar mehrere Marker auf einen einzigen
Test setzen. Zunächst setzen wir in :file:`test_finish.py` mit einem Marker auf
Dateiebene:

.. code-block:: python
   :emphasize-lines: 5

    import pytest

    from items import Item

    pytestmark = pytest.mark.finish

Wenn pytest ein ``pytestmark``-Attribut in einem Testmodul sieht, wird es den
oder die Marker auf alle Tests in diesem Modul anwenden. Wenn ihr mehr als einen
Marker auf die Datei anwenden wollt, könnt ihr eine Listenform verwenden:
:samp:`pytestmark = [pytest.mark.{MARKER_ONE}, pytest.mark.{MARKER_TWO}]`.

Eine andere Möglichkeit, mehrere Tests gleichzeitig zu markieren, besteht darin,
Tests in einer Klasse zu haben und Markierungen auf Klassenebene zu verwenden:

.. code-block:: python
   :emphasize-lines: 1

    @pytest.mark.smoke
    class TestFinish:
        def test_finish_from_todo(self, items_db):
            i = items_db.add_item(Item("Update pytest section", state="todo"))
            items_db.finish(i)
            s = items_db.get_item(i)
            assert s.state == "done"

        def test_finish_from_in_prog(self, items_db):
            i = items_db.add_item(
                Item("Update pytest section", state="in progress")
            )
            items_db.finish(i)
            s = items_db.get_item(i)
            assert s.state == "done"

        def test_finish_from_done(self, items_db):
            i = items_db.add_item(Item("Update pytest section", state="done"))
            items_db.finish(i)
            s = items_db.get_item(i)
            assert s.state == "done"

Die Testklasse :class:`TestFinish` ist mit ``@pytest.mark.smoke``
gekennzeichnet. Wenn ihr eine Testklasse auf diese Weise markiert, wird jede
Testmethode in der Klasse mit dem gleichen Marker versehen.

Wir können auch nur bestimmte Testfälle eines parametrisierten Tests markieren:

.. code-block:: python
   :emphasize-lines: 5

    @pytest.mark.parametrize(
        "states",
        [
            "todo",
            pytest.param("in progress", marks=pytest.mark.smoke),
            "done",
        ],
    )
    def test_finish(items_db, start_state):
        i = items_db.add_item(Item("Update pytest section", state=states))
        items_db.finish(i)
        s = items_db.get_item(i)
        assert s.state == "done"

Die :func:`test_finish`-Funktion ist nicht direkt markiert, sondern nur einer
ihrer Parameter: :samp:`pytest.param("in progress", marks=pytest.mark.smoke)`.
Ihr könnt mehr als einen Marker verwenden, indem ihr die Listenform verwendet:
:samp:`marks=[pytest.mark.{ONE}, pytest.mark.{TWO}]`. Wenn ihr alle Testfälle
eines parametrisierten Tests markieren wollt, fügt ihr den Marker wie bei einer
normalen Funktion entweder über oder unter dem Dekorator ``parametrize`` ein.

Das vorherige Beispiel bezog sich auf die Funktionsparametrisierung. Ihr könnt
jedoch auch Fixtures auf die gleiche Weise markieren:

.. code-block:: python
   :emphasize-lines: 8-9, 12

    @pytest.fixture(
        params=[
            "todo",
            pytest.param("in progress", marks=pytest.mark.smoke),
            "done",
        ]
    )
    def start_state_fixture(request):
        return request.param


    def test_finish(items_db, start_state_fixture):
        i = items_db.add_item(
            Item("Update pytest section", state=start_state_fixture)
        )
        items_db.finish(i)
        s = items_db.get_item(i)
        assert s.state == "done"

Wenn ihr einer Funktion mehr als eine Markierung hinzufügen wollt, könnt ihr
einfach stapeln. Zum Beispiel wird :func:`test_finish_non_existent` sowohl mit
``@pytest.mark.smoke`` als auch mit ``@pytest.mark.exception`` markiert:

.. code-block:: python
   :emphasize-lines: 4-5

    from items import InvalidItemId, Item


    @pytest.mark.smoke
    @pytest.mark.exception
    def test_finish_non_existent(items_db):
        i = 44  # any_number will be invalid, db is empty
        with pytest.raises(InvalidItemId):
            items_db.finish(i)

Wir haben in :file:`test_finish.py` eine Reihe von Markern auf verschiedene
Weise hinzugefügt. Dabei verwenden wir die Marker, um die auszuführenden Tests
anstatt eine Testdatei auszuwählen:

.. code-block:: pytest

    $ cd tests
    $ tests % pytest -v -m exception
    ============================= test session starts ==============================
    ...
    configfile: pytest.ini
    collected 36 items / 34 deselected / 2 selected

    test_finish.py::test_finish_non_existent PASSED                          [ 50%]
    test_start.py::test_start_non_existent PASSED                            [100%]

    ======================= 2 passed, 34 deselected in 0.07s =======================

Marker zusammen mit ``and``, ``or``, ``not`` und ``()``
-------------------------------------------------------

Wir können Marker logisch verknüpfen, um Tests auszuwählen, genau wie wir ``-k``
zusammen mit Schlüsselwörtern zur Auswahl von Testfällen in :ref:`Testsuite
<keyword>` verwendet haben. So können wir nur die ``finish``-Tests, die sich mit
``exception`` befassen:

.. code-block:: pytest

    pytest -v -m "finish and exception"
    ============================= test session starts ==============================
    ...
    configfile: pytest.ini
    collected 36 items / 35 deselected / 1 selected

    test_finish.py::test_finish_non_existent PASSED                          [100%]

    ======================= 1 passed, 35 deselected in 0.08s =======================

Wir können auch alle logischen Verknüpfungen zusammen verwenden:

.. code-block:: pytest

     $ pytest -v -m "(exception or smoke) and (not finish)"
    ============================= test session starts ==============================
    ...
    configfile: pytest.ini
    collected 36 items / 34 deselected / 2 selected

    test_start.py::test_start PASSED                                         [ 50%]
    test_start.py::test_start_non_existent PASSED                            [100%]

    ======================= 2 passed, 34 deselected in 0.08s =======================

Schließlich können wir auch Marker und Keywords für die Auswahl kombinieren,
:abbr:`z.B. (zum Beispiel)` um Smoke-Tests auszuführen, die nicht Teil der
Klasse :class:`TestFinish` sind:

.. code-block::

    $ pytest -v -m smoke -k "not TestFinish"
    ============================= test session starts ==============================
    ...
    configfile: pytest.ini
    collected 36 items / 33 deselected / 3 selected

    test_finish.py::test_finish[in progress] PASSED                          [ 33%]
    test_finish.py::test_finish_non_existent PASSED                          [ 66%]
    test_start.py::test_start PASSED                                         [100%]

    ======================= 3 passed, 33 deselected in 0.07s =======================

Bei der Verwendung von Markern und Keywords ist zu beachten, dass die Namen der
Marker bei der Option :samp:`-m {MARKERNAME}` vollständig sein müssen, während
Keywords bei der Option :samp:`-k {KEYWORD}` eher einen Substring darstellen.

``--strict-markers``
--------------------

Üblicherweise erhalten wir eine Warnung, wenn ein Marker nicht registriert ist.
Wenn diese Warnung stattdessen ein Fehler sein soll, können wir die Option
``--strict-markers`` verwenden. Dies hat zwei Vorteile:

#. Der Fehler wird bereits ausgegeben, wenn die auszuführenden Tests gesammelt
   werden und nicht erst zur Laufzeit. Wenn ihr eine Testsuite habt, die länger
   als ein paar Sekunden dauert, werdet ihr es zu schätzen wissen, wenn ihr
   diese Rückmeldung schnell erhaltet.
#. Zweitens sind Fehler manchmal leichter zu erkennen als Warnungen, besonders
   in Systemen mit :term:`kontinuierlicher Integration <Kontinuierliche
   Integration>`.

.. tip::
   Es empfiehlt sich daher, immer ``--strict-markers`` zu verwenden. Anstatt die
   Option jedoch immer wieder einzugeben, könnt ihr ``--strict-markers`` in den
   Abschnitt ``addopts`` der :file:`pytest.ini` einfügen:

   .. code-block:: ini
      :emphasize-lines: 3-4

      [pytest]
      ...
      addopts =
          --strict-markers

.. _marker_fixtures_combined:

Marker mit Fixtures kombinieren
-------------------------------

Marker können in Verbindung mit Fixtures, Plugins und Hook-Funktionen verwendet
werden.  Die Built-in-Marker benötigen Parameter, während die
benutzerdefinierten Marker, die wir bisher verwendet haben, keine Parameter
benötigen. Erstellen wir einen neuen Marker namens ``num_items``, den wir an die
``items_db-Fixture`` übergeben können. Die ``items_db``-Fixture bereinigt
derzeit die Datenbank für jeden Test, der sie verwenden möchte:

.. code-block:: python

    @pytest.fixture(scope="function")
    def items_db(session_items_db):
        db = session_items_db
        db.delete_all()
        return db

Wenn wir zum Beispiel vier Items in der Datenbank haben wollen, wenn unser Test
beginnt, können wir einfach eine andere, aber ähnliche Fixture schreiben:

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


    @pytest.fixture(scope="function")
    def populated_db(items_db, items_list):
        """ItemsDB object populated with 'items_list'"""
        for i in items_list:
            items_db.add_item(i)
        return items_db

Dann könnten wir die ursprüngliche Fixture für Tests verwenden, die eine leere
Datenbank bereitstellt, und die neue Fixture für Tests, die eine Datenbank mit
vier Items enthält:

.. code-block:: python

    def test_zero_item(items_db):
        assert items_db.count() == 0


    def test_four_items(populated_db):
        assert populated_db.count() == 4

Wir haben nun die Möglichkeit, entweder null oder vier Items in der Datenbank
zu testen. Was aber, wenn wir keine, vier oder 13 Items haben wollen? Dann
wollen wir nicht jedesmal eine neue Fixture schreiben. Marker erlauben uns,
einem Test zu sagen, wieviele Items wir haben wollen. Hierfür sind drei Schritte
notwendig:

#. Zunächst definieren wir drei verschiedene Tests in :file:`test_items.py` mit
   dem unserem Marker ``@pytest.mark.num_items``:

   .. code-block:: python

       @pytest.mark.num_items
       def test_zero_item(items_db):
           assert items_db.count() == 0


       @pytest.mark.num_items(4)
       def test_four_items(items_db):
           assert items_db.count() == 4


       @pytest.mark.num_items(13)
       def test_thirteen_items(items_db):
           assert items_db.count() == 13

#. Diesen Marker müssen wir dann in der :file:`pytest.ini`-Datei deklarieren:

   .. code-block:: ini
      :emphasize-lines: 4

      [pytest]
      markers =
          ...
          num_items: Number of items to be pre-filled for the items_db fixture

#. Nun modifizieren wir die ``items_db``-Fixture in der
   :file:`conftest.py`-Datei, um den Marker verwenden zu können. Um die
   Item-Informationen nicht hart kodieren zu müssen, werden wir das Python-Paket
   `Faker <https://faker.readthedocs.io/>`_ verwenden, das wir mit ``python -m
   pip install faker`` installieren können:

   .. code-block:: python
      :linenos:
      :emphasize-lines: 5, 13-

      import os
      from pathlib import Path
      from tempfile import TemporaryDirectory

      import faker
      import pytest

      import items

      ...


      @pytest.fixture(scope="function")
      def items_db(session_items_db, request, faker):
          db = session_items_db
          db.delete_all()
          # Support for random selection "@pytest.mark.num_items({NUMBER})`.
          faker.seed_instance(99)
          m = request.node.get_closest_marker("num_items")
          if m and len(m.args) > 0:
              num_items = m.args[0]
              for _ in range(num_items):
                  db.add_item(
                      Item(summary=faker.sentence(), owner=faker.first_name())
                  )
          return db

   Hier gibt es eine Menge Änderungen, die wir jetzt durchgehen wollen.

   Zeile 13
    Wir haben ``request`` und ``faker`` in die Liste der ``items_db``-Parameter
    aufgenommen.
   Zeile 18
    Dies setzt die Zufälligkeit von Faker, so dass wir jedes Mal die gleichen
    Daten erhalten. Dabei verwenden wir Faker hier nicht für sehr zufällige
    Daten, sondern um zu vermeiden, dass wir selbst Daten erfinden müssen.
   Zeile 19
    Hier verwenden wir ``request``, genauer ``request.node`` für die
    pytest-Repräsentation eines Tests.  ``get_closest_marker('num_items')`` gibt
    ein Marker-Objekt zurück, wenn der Test mit ``num_items`` markiert ist,
    andernfalls gibt es ``None`` zurück. Die
    :func:`get_closest_marker`-Funktion gibt den Marker zurück, der dem Test am
    nächsten liegt, und das ist normalerweise das, was wir wollen.
   Zeile 20
    Der Ausdruck ist wahr, wenn der Test mit ``num_items`` markiert ist und ein
    Argument angegeben wird. Die zusätzliche ``len``-Prüfung dient dazu, dass,
    falls jemand versehentlich nur ``pytest.mark.num_items`` verwendet, ohne die
    Anzahl der Items anzugeben, dieser Teil übersprungen wird.
   Zeile 21–24
    Sobald wir wissen, wie viele Items wir erstellen müssen, lassen wir Faker
    einige Daten für uns erstellen. Faker stellt die Faker-Fixture zur
    Verfügung.

    * Für das Feld ``summary`` funktioniert die Methode
      :func:`faker.sentence`.
    * Für das Feld ``Owner`` funktioniert die Methode
      :func:`faker.first_name`.

    .. seealso::
       * Es gibt noch viele andere Möglichkeiten, die ihr mit Faker nutzen
         könnt. Schaut hierfür in die `Faker-Dokumentation
         <https://faker.readthedocs.io/>`_.

       * Neben Faker gibt es nach weitere Bibliotheken, die Fake-Daten
         bereitstellen, siehe :ref:`Fake Plugins <fake_plugins>`.

Führen wir die Tests nun aus, um sicherzustellen, dass alles richtig
funktioniert:

.. code-block:: pytest

    $ pytest -v -s test_items.py
    ============================= test session starts ==============================
    ...
    configfile: pytest.ini
    plugins: Faker-19.10.0
    collected 3 items

    test_items.py::test_zero_item PASSED
    test_items.py::test_four_items PASSED
    test_items.py::test_thirteen_items PASSED

    ============================== 3 passed in 0.09s ===============================

.. note::
   Damit ihr einen Eindruck bekommt, wie die Daten von Faker aussehen, könnt ihr
   eine ``print``-Anweisung zu :func:`test_four_items` hinzufügen:

   .. code-block:: python
      :emphasize-lines: 4-

      @pytest.mark.num_items(4)
      def test_four_items(items_db):
          assert items_db.count() == 4
          print()
          for i in items_db.list_items():
              print(i)

   Anschließend könnt ihr die Tests in :file:`test_items.py` erneut aufrufen:

   .. code-block:: pytest
      :emphasize-lines: 10-13

      $ pytest -v -s test_items.py
      ============================= test session starts ==============================
      ...
      configfile: pytest.ini
      plugins: Faker-19.10.0
      collected 3 items

      test_items.py::test_zero_item PASSED
      test_items.py::test_four_items
      Item(summary='Herself outside discover card beautiful rock.', owner='Alyssa', state='todo', id=1)
      Item(summary='Bed perhaps current reveal open society small.', owner='Lynn', state='todo', id=2)
      Item(summary='Charge produce sure full water.', owner='Allison', state='todo', id=3)
      Item(summary='Light I especially account.', owner='James', state='todo', id=4)
      PASSED
      test_items.py::test_thirteen_items PASSED

      ============================== 3 passed in 0.09s ===============================

Marker auflisten
----------------

Wir haben bereits eine Menge Marker behandelt: die Built-in-Marker ``skip``,
``skipif`` und ``xfail``, unsere eigenen Marker ``smoke``, ``exception``,
``finish`` und ``num_items`` und es gibt auch noch ein paar weitere
Built-in-Marker. Und wenn wir anfangen, :doc:`plugins` zu verwenden, können noch
weitere Marker hinzukommen. Um alle verfügbaren Marker mit Beschreibungen und
Parameter aufzulisten, könnt ihr ``pytest --markers`` ausführen:

.. code-block:: console

   $ pytest --markers
   @pytest.mark.exception: Only run expected exceptions

   @pytest.mark.finish: Only run finish tests

   @pytest.mark.smoke: Small subset of all tests

   @pytest.mark.num_items: Number of items to be pre-filled for the items_db fixture

   @pytest.mark.filterwarnings(warning): add a warning filter to the given test. see https://docs.pytest.org/en/stable/how-to/capture-warnings.html#pytest-mark-filterwarnings
   ...

Dies ist eine sehr praktische Funktion, mit der wir schnell nach Markern suchen
können, und ein guter Grund, nützliche Beschreibungen zu unseren eigenen Markern
hinzuzufügen.
