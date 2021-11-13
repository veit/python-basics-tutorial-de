Mock
====

`Mock-Objekte <https://de.wikipedia.org/wiki/Mock-Objekt>`_ fördern Tests, die
auf dem Verhalten von Objekten basieren. Die Python-Bibliothek :doc:`mock
<python3:library/unittest.mock>` ermöglicht euch, Teile des zu testenden Systems
durch Scheinobjekte zu ersetzen und Aussagen über deren Verwendung zu treffen.

Installation
------------

:doc:`mock <python3:library/unittest.mock>` ist seit Python 3.3 in der
Python-Standardbibliothek enthalten. Für ältere Versionen von Python könnt ihr
sie installieren mit:

.. tabs::

   .. tab:: Linux/MacOS

      .. code-block:: console

         $ bin/python -m pip install mock

   .. tab:: Windows

      .. code-block:: ps1con

         C:> Scripts\python -m pip install mock

Beispiel
--------

In unserem Beispiel wollen wir prüfen, ob die Arbeitstage von Montag bis Freitag
korrekt ermittelt werden.

Zunächst importieren wir ``datetime.datetime`` und ``Mock``:

   .. literalinclude:: test_mock.py
      :language: python
      :lines: 1-2
      :lineno-start: 1

#. Dann definieren wir zwei Testtage:

   .. literalinclude:: test_mock.py
      :language: python
      :lines: 5-6
      :lineno-start: 5

#. Nun definieren wir eine Methode zur Überprüfung der Arbeitstage, wobei die
   datetime-Bibliothek von Python Montage als ``0``  und Sonntage als ``6``
   behandelt:

   .. literalinclude:: test_mock.py
      :language: python
      :lines: 8-10
      :lineno-start: 8

#. Dann mocken wir datetime:

   .. literalinclude:: test_mock.py
      :language: python
      :lines: 12
      :lineno-start: 12

#. Schließlich testen wir unsere beiden Mock-Objekte:

   .. literalinclude:: test_mock.py
      :language: python
      :lines: 15,17
      :lineno-start: 15

   .. literalinclude:: test_mock.py
      :language: python
      :lines: 19,21
      :lineno-start: 19
