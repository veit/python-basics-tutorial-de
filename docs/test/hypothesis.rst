Hypothesis
==========

`Property Testing
<https://en.wikipedia.org/wiki/Software_testing#Property_testing>`_ ist eine
Testmethode, bei der nicht überprüft wird, ob bestimmte Eingaben zu bestimmten
Ausgaben führen, sondern bei der zufällig Eingaben generiert werden, uns das
Programm mit allen diesen Eingaben ausgeführtund dann die Gültigkeit dieser
*Eigenschaft* überprüft wird.

In Python könnt ihr euch mit `Hypothesis <https://hypothesis.readthedocs.io/>`_
solche Eingaben :term:`parametrisiert <Parameter>` generieren lassen, um schnell
Fehler in euren Tests finden zu können.

.. seealso::
   Im :doc:`Jupyter Tutorial <jupyter-tutorial:notebook/testing/hypothesis>` ist
   beschrieben, wie Hypothesis auch in Jupyter Notebooks verwendet werden kann.

Installation
------------

.. tab:: Linux/macOS

   .. code-block:: console

      $ uv add --group tests hypothesis

.. tab:: Windows

   .. code-block:: ps1con

      C:> uv add --group tests hypothesis

Alternativ kann Hypothesis auch mit Erweiterungen installiert werden,
:abbr:`z. B. (zum Beispiel)`:

.. tab:: Linux/macOS

   .. code-block:: console

      $ uv add --group tests "hypothesis[numpy, pandas]"

.. tab:: Windows

   .. code-block:: ps1con

          C:> uv add --group tests "hypothesis[numpy, pandas]"

.. seealso:
   * `First-party extensions
     <https://hypothesis.readthedocs.io/en/latest/extras.html>`_

Beispiel mit ``strategies`` und ``given``
-----------------------------------------

#. Zunächst importieren wir von `hypothesis.strategies
   <https://hypothesis.readthedocs.io/en/latest/reference/strategies.html>`_
   Beispieldaten für `floats
   <https://hypothesis.readthedocs.io/en/latest/reference/strategies.html#hypothesis.strategies.floats>`_
   und `lists
   <https://hypothesis.readthedocs.io/en/latest/reference/strategies.html#hypothesis.strategies.lists>`_.
   Um diese Beispieldaten auf unsere Testfunktion anwenden zu können,
   importieren wir darüberhinaus `hypothesis.given
   <https://hypothesis.readthedocs.io/en/latest/reference/api.html#hypothesis.given>`_:

   .. literalinclude:: test_hypothesis.py
      :language: python
      :lines: 1-3
      :lineno-start: 1

#. Für unseren Test verwenden wir nun ``hypothesis.given`` als :doc:`Dekorator
   <../functions/decorators>` um die Testfunktion in eine parametrisierte
   umzuwandeln, die dann mit einer großen Varianz passender Daten ausgeführt
   wird:

   .. literalinclude:: test_hypothesis.py
      :language: python
      :lines: 6-8, 10
      :lineno-start: 6

#. Schließlich führen wir den Test aus:

   .. tab:: Linux/macOS

      .. code-block:: pytest

         $ uv run pytest docs/test/test_hypothesis.py
         ============================= test session starts ==============================
         platform darwin -- Python 3.13.0, pytest-9.0.3, pluggy-1.6.0
         rootdir: /Users/veit/cusy/trn/python-basics-tutorial-de
         plugins: hypothesis-6.152.1
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
         E         +  where 1.7976931348623157e+308 = max([9.9792015476736e+291, 1.7976931348623157e+308])

         test_hypothesis.py:8: AssertionError
         ---------------------------------- Hypothesis ----------------------------------
         Falsifying example: test_mean(
             ls=[9.9792015476736e+291, 1.7976931348623157e+308],
         )
         =========================== short test summary info ============================
         FAILED test_hypothesis.py::test_mean - assert inf <= 1.7976931348623157e+308
         ============================== 1 failed in 0.44s ===============================

   .. tab:: Windows

      .. code-block:: pytest

         C:> uv run pytest docs/test/test_hypothesis.py
         ============================= test session starts ==============================
         platform darwin -- Python 3.13.0, pytest-9.0.3, pluggy-1.6.0
         rootdir: C:\Users\veit\python-basics-tutorial-de
         plugins: hypothesis-6.152.1
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

   In der Liste ``[9.9792015476736e+291, 1.7976931348623157e+308]`` ergibt die
   Mittelwertsberechnung ``inf`` und ``inf`` ist nicht kleiner als die größere
   der beiden Zahlen.

Beispiel mit regulären Ausdrücken
---------------------------------

#. Im folgenden Beispiel versuchen wir, aus der E-Mail-Adresse ``username`` und
   ``domain`` zu ermitteln mit:

   .. code-block:: python

      def parse_email(email):
          result = re.match("(?P<username>\w+).(?P<domain>[\w\.]+)", email).groups()
          return result

#. Nun schreiben wir einen Test :func:`test_parse_email` zum Überprüfen unserer
   Funktion. Als Eingabewerte verwenden wir die `emails
   <https://hypothesis.readthedocs.io/en/latest/reference/strategies.html#hypothesis.strategies.emails>`_-Strategie
   von Hypothesis. Als ``result`` erwarten wir :abbr:`z. B. (zum Beispiel) bei
   :samp:`veit@cusy.io` als ``username`` veit und als ``domain`` cusy.io.

#. In unserem Test nehmen wir einerseits an, dass immer zwei Einträge
   zurückgegeben werden und im zweiten Eintrag ein Punkt (``.``) vorkommt:

   .. code-block:: python

       @given(emails())
       def test_parse_email(email):
           result = parse_email(email)
           assert len(result) == 2
           assert "." in result[1]

#. Nun führen wir den Test aus:

   .. code-block:: pytest
      :emphasize-lines: 26

      $ uv run pytest docs/test/test_emails.py
      ============================= test session starts ==============================
      platform darwin -- Python 3.13.0, pytest-9.0.3, pluggy-1.6.0
      rootdir: /Users/veit/cusy/trn/python-basics-tutorial-de
      configfile: pyproject.toml
      plugins: hypothesis-6.152.1
      collected 1 item

      docs/test/test_emails.py F                                               [100%]

      =================================== FAILURES ===================================
      _______________________________ test_parse_email _______________________________
        + Exception Group Traceback (most recent call last):
        |   File "/Users/veit/cusy/trn/python-basics-tutorial-de/docs/test/test_emails.py", line 12, in test_parse_email
        |     def test_parse_email(email):
        |                    ^^^
        |   File "/Users/veit/cusy/trn/python-basics-tutorial-de/.venv/lib/python3.13/site-packages/hypothesis/core.py", line 2264, in wrapped_test
        |     raise the_error_hypothesis_found
        | ExceptionGroup: Hypothesis found 2 distinct failures. (2 sub-exceptions)
        +-+---------------- 1 ----------------
          | Traceback (most recent call last):
          |   File "/Users/veit/cusy/trn/python-basics-tutorial-de/docs/test/test_emails.py", line 16, in test_parse_email
          |     assert '.' in result[1]
          | AssertionError: assert '.' in '0'
          | Falsifying example: test_parse_email(
          |     email='0/0@A.AC',
          | )
          +---------------- 2 ----------------
          | Traceback (most recent call last):
          |   File "/Users/veit/cusy/trn/python-basics-tutorial-de/docs/test/test_emails.py", line 13, in test_parse_email
          |     result = parse_email(email)
          |   File "/Users/veit/cusy/trn/python-basics-tutorial-de/docs/test/test_emails.py", line 8, in parse_email
          |     result = re.match(r"(?P<username>\w+).(?P<domain>[\w\.]+)", email).groups()
          |              ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
          | AttributeError: 'NoneType' object has no attribute 'groups'
          | Falsifying example: test_parse_email(
          |     email='/@A.AC',
          | )
          +------------------------------------
      =========================== short test summary info ============================
      FAILED docs/test/test_emails.py::test_parse_email - ExceptionGroup: Hypothesis found 2 distinct failures. (2 sub-exceptions)
      ============================== 1 failed in 0.14s ===============================

   Das von Hypothesis gegebene E-Mail-Adresse ``0/0@A.ac`` verdeutlicht, dass
   unser regulärer Ausdruck in der :func:`parse_email`-Methode noch nicht
   hinreichend ist. Daher passen wir nun unseren regulären Ausdruck an und rufen
   anschließend den Test erneut auf:

   .. literalinclude:: test_emails_2.py
      :diff: test_emails.py

   .. code-block:: pytest

        $ uv run pytest docs/test/test_emails_2.py
        ============================= test session starts ==============================
        platform darwin -- Python 3.13.0, pytest-9.0.3, pluggy-1.6.0
        rootdir: /Users/veit/cusy/trn/python-basics-tutorial-de
        configfile: pyproject.toml
        plugins: hypothesis-6.152.1
        collected 1 item

        docs/test/test_emails_2.py .                                             [100%]

        ============================== 1 passed in 0.29s ===============================

Erweiterungen von Drittanbietern
--------------------------------

Es gibt eine Reihe von Open-Source-Bibliotheken, die :doc:`index` erweitern. Auf
`Third-party extensions
<https://hypothesis.readthedocs.io/en/latest/extensions.html>`_ sind einige
davon aufgeführt; weitere findet ihr auf :term:`PyPI`, wenn ihr nach
`Stichworten <https://pypi.org/search/?q=hypothesis>`_ oder nach
`Framework-Classifier
<https://pypi.org/search/?c=Framework+%3A%3A+Hypothesis>`_ durchsucht.
