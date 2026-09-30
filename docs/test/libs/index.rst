Testbibliotheken
================

:doc:`unittest` und :doc:`pytest/index` unterstützen euch bei folgenden
Testkonzepten:

.. glossary::

   Test Case (Testfall)
       testet eine einzelnes Szenario.

   Test Fixture (Prüfvorrichtung)
       ist eine konsistente Testumgebung.

   Test Suite
       ist eine Sammlung mehrerer :term:`Test Cases <Test Case (Testfall)>`.

   Test Runner
       durchläuft eine :term:`Test Suite` und stellt die Ergebnisse dar.

:doc:`tox` stellt darüberhinaus verschiedene Umgebungen bereit, in denen Tests
ausgeführt werden können. :doc:`hypothesis` schließlich unterstützt euch bei
:term:`Blackbox-Tests <Blackbox-Test>`.

.. toctree::
   :titlesonly:
   :hidden:

   unittest
   pytest/index
   tox
   mock/index
   hypothesis
