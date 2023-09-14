Testen
======

Grundsätzlich wird zwischen statischen und dynamischen Testverfahren unterschieden.

.. glossary::

   Statische Testverfahren
    werden verwendet um den Quellcode zu überprüfen, wobei dieser jedoch nicht
    ausgeführt wird. Sie unterteilen sich in

    * :ref:`Reviews <code_reviews>` und
    * `Statische Code-Analyse
      <https://de.wikipedia.org/wiki/Statische_Code-Analyse>`_

      Es gibt diverse Python-Pakete, die euch bei der statischen Code-Analyse
      unterstützen können, u.a. :doc:`Python4DataScience:productive/qa/flake8`,
      :doc:`Python4DataScience:productive/qa/pysa` und
      :doc:`Python4DataScience:productive/qa/wily`.

   Dynamische Testverfahren
    dienen dem Auffinden von Fehlern beim Ausführen des Quellcodes. Dabei wird
    zwischen Whitebox- und Backbox-Tests unterschieden.

    Whitebox-Tests
        werden unter Kenntnis des Quellcodes und der Software-Struktur entwickelt.
        In Python stehen euch verschiedene Module zur Verfügung:

        :doc:`unittest`
            unterstützt euch bei der Automatisierung von Tests.
        :doc:`mock`
            erlaubt euch das Erstellen und Verwenden von Mock-Objekten.
        :doc:`doctest`
            ermöglicht das Testen von in Python Docstrings geschriebenen Tests.
        :doc:`tox`
            ermöglicht das Testen in verschiedenen Umgebungen.

    Blackbox-Tests
        werden ohne Kenntnis des Quellcodes entwickelt. Neben :doc:`unittest`
        kann in Python auch :doc:`hypothesis` für solche Tests verwendet werden.

.. seealso::
   * `Python Testing and Continuous Integration
     <http://carpentries-incubator.github.io/python-testing/>`_

.. toctree::
   :titlesonly:
   :hidden:

   unittest
   sqlite
   mock
   doctest
   hypothesis
   pytest
   tox
   unittest2
   coverage
