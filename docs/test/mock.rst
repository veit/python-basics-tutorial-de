Mock
====

`Mock-Objekte <https://de.wikipedia.org/wiki/Mock-Objekt>`_ fördern Tests, die
auf dem Verhalten von Objekten basieren. Die Python-Bibliothek :doc:`mock
<python3:library/unittest.mock>` ermöglicht euch, Teile des zu testenden Systems
durch Scheinobjekte zu ersetzen und Aussagen über deren Verwendung zu treffen.

.. seealso::

   Mit `responses <https://github.com/getsentry/responses>`_ könnt ihr
   Mock-Objekte für die `Requests
   <https://jupyter-tutorial.readthedocs.io/de/latest/data-processing/requests/index.html>`_-Bibliothek
   erstellen.

Installation
------------

:doc:`mock <python3:library/unittest.mock>` ist seit Python 3.3 in der
Python-Standardbibliothek enthalten. Für ältere Versionen von Python könnt ihr
sie installieren mit:

.. tab:: Linux/macOS

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
      :lines: 9-11
      :lineno-start: 9

#. Dann mocken wir datetime:

   .. literalinclude:: test_mock.py
      :language: python
      :lines: 14
      :lineno-start: 14

#. Schließlich testen wir unsere beiden Mock-Objekte:

   .. literalinclude:: test_mock.py
      :language: python
      :lines: 17-19
      :lineno-start: 17

   .. literalinclude:: test_mock.py
      :language: python
      :lines: 21-23
      :lineno-start: 21

patch-Dekorator
---------------

Um Mock-Klassen oder Objekte zu erzeugen, kann der ``patch``-Dekorator verwendet
werden. In den folgenden Beispielen wird die Ausgabe von ``os.listdir`` gemockt.
Dazu muss die Datei ``example.txt`` nicht im Verzeichnis vorhanden sein:

.. code-block:: python

    import os
    from unittest import mock


    @mock.patch("os.listdir", mock.MagicMock(return_value="example.txt"))
    def test_listdir():
        assert "example.txt" == os.listdir()

Alternativ kann der Rückgabewert auch separat definiert werden:

.. code-block:: python

    @mock.patch("os.listdir")
    def test_listdir(mock_listdir):
        mock_listdir.return_value = "example.txt"
        assert "example.txt" == os.listdir()
