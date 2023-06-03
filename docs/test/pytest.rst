pytest
======

`pytest <https://docs.pytest.org/>`_ ist eine Alternative zu Pythons
:doc:`unittest`-Modul, die das Testen noch weiter vereinfacht.

Merkmale
--------

* Ausführlichere Informationen über fehlgeschlagene ``assert``-Anweisungen
* Automatische Erkennung von Testmodulen und -Funktionen
* Modulare Fixtures für die Verwaltung von kleinen oder parametrisierten,
  langlebigen Testressourcen
* Kann auch Unittests ohne Voreinstellungen ausführen
* Umfangreiche Plugin-Architektur, mit über 800 externen Plugins

Installation
------------

.. tab:: Linux/macOS

   .. code-block:: console

      $ bin/python -m pip install pytest
      Collecting pytest
      …
      Successfully installed attrs-21.2.0 iniconfig-1.1.1 pluggy-1.0.0 py-1.10.0 pytest-6.2.5 toml-0.10.2

.. tab:: Windows

   .. code-block:: ps1con

      C:> Scripts\python -m pip install pytest
      Collecting pytest
      …
      Successfully installed attrs-21.2.0 iniconfig-1.1.1 pluggy-1.0.0 py-1.10.0 pytest-6.2.5 toml-0.10.2

Einzlener Test
--------------

.. literalinclude:: test_pytest.py
   :language: python
   :lines: 1-4, 19-24
   :lineno-start: 1

Test-Fixture
------------

Schreibt ein :term:`Test Fixture <Test Fixture (Prüfvorrichtung)>` mit dem
``@pytest.fixture``-Dekorator:

.. literalinclude:: test_pytest.py
   :language: python
   :lines: 9-16
   :lineno-start: 9

``key``
     Funktion, die aufgerufen wird, um die Elemente der Kollektion zu
     transformieren, bevor sie verglichen werden. Der Parameter, der an ``key``
     übergeben wird, muss aufrufbar sein.
``lambda``
    Funktion, die im Falle von ``sorted`` nur einen Parameter benötigt.

Testparametrisierung
--------------------

.. literalinclude:: test_pytest.py
   :language: python
   :lines: 35-
   :lineno-start: 35

Tests ausführen
---------------

.. tab:: Linux/macOS

   .. code-block:: console

      $ bin/python -m pytest -v
      ============================= test session starts ==============================
      platform darwin -- Python 3.9.7, pytest-6.2.5, py-1.10.0, pluggy-1.0.0 -- /Users/veit/python-basics/bin/python
      rootdir: /Users/veit/python-basics/docs/test
      plugins: hypothesis-6.23.2
      collected 5 items

      test_pytest.py::test_sorted PASSED                                       [ 20%]
      test_pytest.py::test_sorted__key_example_1 PASSED                        [ 40%]
      test_pytest.py::test_sorted__key_example_2 PASSED                        [ 60%]
      test_pytest.py::test_examples[input0-expected0] PASSED                   [ 80%]
      test_pytest.py::test_examples[zasdqw-expected1] PASSED                   [100%]

      ============================== 5 passed in 0.02s ===============================

.. tab:: Windows

   .. code-block:: ps1con

      C:> Scripts\python -m pytest -v
      ============================= test session starts ==============================
      platform win32 -- Python 3.9.7, pytest-6.2.5, py-1.10.0, pluggy-1.0.0
      rootdir: C:\Users\veit\python-basics\docs\test
      plugins: hypothesis-6.23.2
      collected 5 items

      test_pytest.py::test_sorted PASSED                                       [ 20%]
      test_pytest.py::test_sorted__key_example_1 PASSED                        [ 40%]
      test_pytest.py::test_sorted__key_example_2 PASSED                        [ 60%]
      test_pytest.py::test_examples[input0-expected0] PASSED                   [ 80%]
      test_pytest.py::test_examples[zasdqw-expected1] PASSED                   [100%]

      ============================== 5 passed in 0.02s ===============================

Plugins
-------

`pytest-asyncio <https://pypi.org/project/pytest-asyncio/>`_
    erleichtert das Testen von Code, wenn ihr die
    :doc:`asyncio <python3:library/asyncio>`-Bibliothek verwendet.
`pytest-cov <https://pypi.org/project/pytest-cov/>`_
    erstellt Coverage-Reports.
:doc:`pytest-grpc <jupyter-tutorial:data-processing/apis/grpc/test>`
    ist ein Pytest-Plugin für
    :doc:`jupyter-tutorial:data-processing/apis/grpc/index`.
`pytest-icdiff <https://pypi.org/project/pytest-icdiff/>`_
    verbessert Diffs in den Fehlermeldungen der Pytest-Assertion mit `ICDiff
    <https://www.jefftk.com/icdiff>`_.

.. seealso::
   `Plugin List <https://docs.pytest.org/en/7.1.x/reference/plugin_list.html>`_
