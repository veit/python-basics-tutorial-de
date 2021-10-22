Testen
======

Konzepte
--------

.. glossary::

   Test Case (Testfall)
       testet eine einzelnes Szenario.

   Test Fixture (Prüfvorrichtung)
       ist eine konsistente Testumgebung.

       .. seealso::
          `pytest fixtures <https://docs.pytest.org/en/stable/fixture.html>`_

   Test Suite
       ist eine Sammlung mehrerer Test Cases.

   Test Runner
       durchläuft eine Test Suite und stellt die Ergebnisse dar.

Python-Testmodule
-----------------

Python enthält mehrere integrierte Module zum Testen Ihres Codes:
:doc:`unittest`, :doc:`mock` und :doc:`doctest`.

.. toctree::
   :titlesonly:
   :hidden:

   unittest
   mock
   doctest

Andere Testwerkzeuge
--------------------

Es gibt weitere Testwerkzeuge für Python, die das Testen erheblich vereinfachen
können:

:doc:`hypothesis`
    ermöglicht euch, Tests zu schreiben, die aus einer Quelle von Beispielen
    parametrisiert werden.
:doc:`pytest`
    vereinfacht das Schreiben von Tests.
:doc:`tox`
    ermöglicht das Testen in verschiedenen Umgebungen.

.. toctree::
   :titlesonly:
   :hidden:

   hypothesis
   pytest
   tox
