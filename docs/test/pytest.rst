pytest
======

`pytest <https://docs.pytest.org/>`_ ist eine Alternative zu Pythons
:doc:`unittest`-Modul, die das Testen erheblich vereinfacht.

Merkmale
--------

* Ausführlichere Informationen über fehlgeschlagene ``assert``-Anweisungen
* Automatische Erkennung von Testmodulen und -Funktionen
* Modulare Fixtures für die Verwaltung von kleinen oder parametrisierten,
  langlebigen Testressourcen
* Kann auch Unittests ohne Voreinstellungen ausführen
* Umfangreiche Plugin-Architektur, mit über 800 externen Plugins

#. Pytest installieren:

   .. code-block:: console

    $ python -m pip install pytest
    Collecting pytest
    …
    Successfully installed attrs-21.2.0 iniconfig-1.1.1 pluggy-1.0.0 py-1.10.0 pytest-6.2.5 toml-0.10.2

#. Schreibt einen einzelnen Test:

   .. literalinclude:: test_pytest.py
      :language: python
      :lines: 1-2,4-5
      :lineno-start: 1

#. Schreibt ein :term:`Test Fixture <Test Fixture (Prüfvorrichtung)>` mit dem
   ``@pytest.fixture``-Dekorator:

   .. literalinclude:: test_pytest.py
      :language: python
      :lines: 8-28
      :lineno-start: 8

   ``key``
    Funktion, die aufgerufen wird, um die Elemente der Kollektion zu
    transformieren, bevor sie verglichen werden. Der Parameter, der an ``key``
    übergeben wird, muss aufrufbar sein.
   ``lambda``
    Funktion, die im Falle von ``sorted`` nur einen Parameter benötigt.

#. Führt pytest aus:

   .. code-block:: console

    python -m pytest -v
    ============================= test session starts ==============================
    platform darwin -- Python 3.9.7, pytest-6.2.5, py-1.10.0, pluggy-1.0.0 -- /Users/veit/python-basics/bin/python
    cachedir: .pytest_cache
    rootdir: /Users/veit/python-basics/docs/test
    collected 5 items

    test_pytest.py::test_sorted PASSED                                       [ 20%]
    test_pytest.py::test_sorted__key_example_1 PASSED                        [ 40%]
    test_pytest.py::test_sorted__key_example_2 PASSED                        [ 60%]
    test_pytest.py::test_examples[input0-expected0] PASSED                   [ 80%]
    test_pytest.py::test_examples[zasdqw-expected1] PASSED                   [100%]

    ============================== 5 passed in 0.02s ===============================
