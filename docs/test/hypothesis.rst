Hypothesis
==========

`Hypothesis <https://hypothesis.readthedocs.io/>`_ ist eine Bibliothek, mit der
ihr Tests schreiben könnt, die aus einer Quelle von Beispielen parametrisiert
werden. Anschließend werden einfache und verständliche Beispiele generiert, die
dazu verwendet werden können, eure Tests fehlschlagen zu lassen und Fehler mit
wenig Aufwand zu finden.

#. Installiert Hypothesis:

   .. tabs::

      .. tab:: Linux/MacOS

         .. code-block:: console

            $ bin/python -m pip install hypothesis

      .. tab:: Windows

         .. code-block:: console

            C:> Scripts\python -m pip install hypothesis

#. Schreibt einen Test:

   #. Importe:

      .. literalinclude:: test_hypothesis.py
         :language: python
         :lines: 1-3
         :lineno-start: 1

   #. Testen:

      .. literalinclude:: test_hypothesis.py
         :language: python
         :lines: 5-
         :lineno-start: 5

#. Test durchführen:

   .. tabs::

      .. tab:: Linux/MacOS

         .. code-block:: python

              $ bin/python -m pytest test_hypothesis.py
              ============================= test session starts ==============================
              platform darwin -- Python 3.9.7, pytest-6.2.5, py-1.10.0, pluggy-1.0.0
              rootdir: /Users/veit/cusy/trn/python-basics/docs/test
              plugins: hypothesis-6.23.2
              collected 1 item

              test_hypothesis.py F                                                     [100%]

              =================================== FAILURES ===================================
              __________________________________ test_mean ___________________________________

                  @given(lists(floats(allow_nan=False, allow_infinity=False), min_size=1))
              >   def test_mean(ls):

              test_hypothesis.py:6:
              _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _

              ls = [9.9792015476736e+291, 1.7976931348623157e+308]

                  @given(lists(floats(allow_nan=False, allow_infinity=False), min_size=1))
                  def test_mean(ls):
                      mean = sum(ls) / len(ls)
              >       assert min(ls) <= mean <= max(ls)
              E       assert inf <= 1.7976931348623157e+308
              E        +  where 1.7976931348623157e+308 = max([9.9792015476736e+291, 1.7976931348623157e+308])

              test_hypothesis.py:8: AssertionError
              ---------------------------------- Hypothesis ----------------------------------
              Falsifying example: test_mean(
                  ls=[9.9792015476736e+291, 1.7976931348623157e+308],
              )
              =========================== short test summary info ============================
              FAILED test_hypothesis.py::test_mean - assert inf <= 1.7976931348623157e+308
              ============================== 1 failed in 0.44s ===============================

      .. tab:: Windows

         .. code-block:: python

              C:> Scripts\python -m pytest test_hypothesis.py
              ============================= test session starts ==============================
              platform win32 -- Python 3.9.7, pytest-6.2.5, py-1.10.0, pluggy-1.0.0
              rootdir: C:\Users\veit\python-basics\docs\test
              plugins: hypothesis-6.23.2
              collected 1 item

              test_hypothesis.py F                                                     [100%]

              =================================== FAILURES ===================================
              __________________________________ test_mean ___________________________________

                  @given(lists(floats(allow_nan=False, allow_infinity=False), min_size=1))
              >   def test_mean(ls):

              test_hypothesis.py:6:
              _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _

              ls = [9.9792015476736e+291, 1.7976931348623157e+308]

                  @given(lists(floats(allow_nan=False, allow_infinity=False), min_size=1))
                  def test_mean(ls):
                      mean = sum(ls) / len(ls)
              >       assert min(ls) <= mean <= max(ls)
              E       assert inf <= 1.7976931348623157e+308
              E        +  where 1.7976931348623157e+308 = max([9.9792015476736e+291, 1.7976931348623157e+308])

              test_hypothesis.py:8: AssertionError
              ---------------------------------- Hypothesis ----------------------------------
              Falsifying example: test_mean(
                  ls=[9.9792015476736e+291, 1.7976931348623157e+308],
              )
              =========================== short test summary info ============================
              FAILED test_hypothesis.py::test_mean - assert inf <= 1.7976931348623157e+308
              ============================== 1 failed in 0.44s ===============================
