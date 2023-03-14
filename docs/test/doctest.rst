Doctest
=======

Das Python-Modul  :doc:`doctest <python3:library/doctest>` prüft, ob die in
einem Docstring angegebenen Tests erfüllt sind.

#. In :download:`arithmetic.py` könnt ihr folgenden Docstring hinzufügen:

   .. literalinclude:: arithmetic.py
      :language: python
      :lines: 9-18
      :lineno-start: 9

#. Anschließend könnt ihr ihn testen mit:

   .. tab:: Linux/macOS

      .. code-block:: console

        $ python -m doctest test/arithmetic.py -v
        Trying:
            add(7,6)
        Expecting:
            13
        ok
        Trying:
            x, y, z = 7, -6.0, 0
        Expecting nothing
        ok
        Trying:
            divide(x, y)
        Expecting:
            -1.1666666666666667
        ok
        Trying:
            divide(x, z)
        Expecting:
            Traceback (most recent call last):
              File "<stdin>", line 1, in <module>
            ZeroDivisionError: division by zero
        ok
        Trying:
            multiply(7,6)
        Expecting:
            42
        ok
        Trying:
            subtract(7,6)
        Expecting:
            1
        ok
        1 items had no tests:
            arithmetic
        4 items passed all tests:
           1 tests in arithmetic.add
           3 tests in arithmetic.divide
           1 tests in arithmetic.multiply
           1 tests in arithmetic.subtract
        6 tests in 5 items.
        6 passed and 0 failed.
        Test passed.

   .. tab:: Windows

      .. code-block:: console

        C:> Scripts\python -m doctest arithmetic.py -v
        Trying:
            add(7,6)
        Expecting:
            13
        ok
        Trying:
            x, y, z = 7, -6.0, 0
        Expecting nothing
        ok
        Trying:
            divide(x, y)
        Expecting:
            -1.1666666666666667
        ok
        Trying:
            divide(x, z)
        Expecting:
            Traceback (most recent call last):
              File "<stdin>", line 1, in <module>
            ZeroDivisionError: division by zero
        ok
        Trying:
            multiply(7,6)
        Expecting:
            42
        ok
        Trying:
            subtract(7,6)
        Expecting:
            1
        ok
        1 items had no tests:
            arithmetic
        4 items passed all tests:
           1 tests in arithmetic.add
           3 tests in arithmetic.divide
           1 tests in arithmetic.multiply
           1 tests in arithmetic.subtract
        6 tests in 5 items.
        6 passed and 0 failed.
        Test passed.

#. Damit die Doctests auch in andere Module importiert werden können, solltet
   ihr die folgenden Zeilen hinzufügen:

   .. literalinclude:: arithmetic.py
      :language: python
      :lines: 38-
      :lineno-start: 38
