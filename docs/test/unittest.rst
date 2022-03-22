Unittest
========

:doc:`unittest <python3:library/unittest>` unterstützt euch bei der
Testautomatisierung mit gemeinsam genutztem Setup- und TearDown-Code sowie der
Aggregation und Unabhängigkeit von Tests.

Hierfür liefert es die folgenden Testkonzepte:

.. glossary::

   Test Case (Testfall)
       testet eine einzelnes Szenario.

   Test Fixture (Prüfvorrichtung)
       ist eine konsistente Testumgebung.

       .. seealso::
          `pytest fixtures <https://docs.pytest.org/en/stable/fixture.html>`_

   Test Suite
       ist eine Sammlung mehrerer :term:`Test Cases <Test Case (Testfall)>`.

   Test Runner
       durchläuft eine :term:`Test Suite` und stellt die Ergebnisse dar.

Beispiel
--------

Angenommen, ihr habt im Modul :download:`test_arithmetic.py` die folgende
Methode zum Hinzufügen implementiert:

.. literalinclude:: arithmetic.py
   :language: python
   :lines: 1,6
   :lineno-start: 1

… dann könnt ihr diese Methode mit einem Unittest testen.

#. Dazu müsst ihr zunächst euer Modul und das Unittest-Modul importieren:

   .. literalinclude:: test_arithmetic.py
      :language: python
      :lines: 1-2
      :lineno-start: 1

#. Anschließend könnt ihr eine Testmethode schreiben, die eure Additionsmethode
   veranschaulicht:

   .. literalinclude:: test_arithmetic.py
      :language: python
      :lines: 4-7
      :lineno-start: 4

#. Damit die Unittests auch in andere Module importiert werden können, solltet
   ihr die folgenden Zeilen hinzufügen:

   .. literalinclude:: test_arithmetic.py
      :language: python
      :lines: 21-22
      :lineno-start: 21

#. Schließlich können alle Tests in :download:`test_arithmetic.py` ausgeführt
   werden:

   .. tab:: Linux/MacOS

      .. code-block:: console

         $ bin/python test_arithmetic.py
         ....
         ----------------------------------------------------------------------
         Ran 4 tests in 0.000s

         OK

   .. tab:: Windows

      .. code-block:: ps1con

         C:> python test_arithmetic.py
         ....
         ----------------------------------------------------------------------
         Ran 4 tests in 0.000s

         OK

   … oder etwas ausführlicher:

   .. tab:: Linux/MacOS

      .. code-block:: ps1con

         $ python test_arithmetic.py -v
         test_addition (__main__.TestArithmetic) ... ok
         test_division (__main__.TestArithmetic) ... ok
         test_multiplication (__main__.TestArithmetic) ... ok
         test_subtraction (__main__.TestArithmetic) ... ok

         ----------------------------------------------------------------------
         Ran 4 tests in 0.000s

         OK

   .. tab:: Windows

      .. code-block:: ps1con

         C:> Scripts\python test_arithmetic.py -v
         test_addition (__main__.TestArithmetic) ... ok
         test_division (__main__.TestArithmetic) ... ok
         test_multiplication (__main__.TestArithmetic) ... ok
         test_subtraction (__main__.TestArithmetic) ... ok

         ----------------------------------------------------------------------
         Ran 4 tests in 0.000s

         OK

.. seealso::
   * :doc:`python3:library/unittest`
