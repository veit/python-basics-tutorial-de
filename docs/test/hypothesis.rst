Hypothesis
==========

`Hypothesis <https://hypothesis.readthedocs.io/>`_ ist eine Bibliothek, mit der
ihr Tests schreiben könnt, die aus einer Quelle von Beispielen parametrisiert
werden. Anschließend werden einfache und verständliche Beispiele generiert, die
dazu verwendet werden können, eure Tests fehlschlagen zu lassen und Fehler mit
wenig Aufwand zu finden.

#. Installiert Hypothesis:

   .. tab:: Linux/macOS

      .. code-block:: console

         $ python -m pip install hypothesis

   .. tab:: Windows

      .. code-block:: ps1con

         C:> python -m pip install hypothesis

   Alternativ kann Hypothesis auch mit Erweiterungen installiert werden, z.B.:

   .. tab:: Linux/macOS

      .. code-block:: console

         $ python -m pip install hypothesis[numpy,pandas]

   .. tab:: Windows

      .. code-block:: ps1con

         C:> python -m pip install hypothesis[numpy,pandas]

#. Schreibt einen Test:

   #. Importe:

      .. literalinclude:: test_hypothesis.py
         :language: python
         :lines: 1-3
         :lineno-start: 1

   #. Testen:

      .. literalinclude:: test_hypothesis.py
         :language: python
         :lines: 6-
         :lineno-start: 6

#. Test durchführen:

   .. tab:: Linux/macOS

      .. code-block:: console

           $ python -m pytest test_hypothesis.py
           ============================= test session starts ==============================
           platform darwin -- Python 3.13.0, pytest-8.3.3, pluggy-1.5.0
           rootdir: /Users/veit/cusy/trn/python-basics/docs/test
           plugins: hypothesis-6.114.1
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

      .. code-block:: console

           C:> python -m pytest test_hypothesis.py
           ============================= test session starts ==============================
           platform win32 -- Python 3.13.0, pytest-8.3.3, pluggy-1.5.0
           rootdir: C:\Users\veit\python-basics\docs\test
           plugins: plugins: hypothesis-6.114.1
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

.. seealso::
   `Hypothesis for the Scientific Stack
   <https://hypothesis.readthedocs.io/en/latest/numpy.html>`_
