Python erkunden
===============

Egal, ob ihr IDLE oder die interaktive Shell nutzt, es gibt einige nützliche
Funktionen, um Python zu erkunden.

``help()``
----------

``help()`` hat zwei verschiedene Modi. Wenn ihr ``help()`` eingebt, ruft ihr das
Hilfesystem auf, mit dem ihr Informationen zu Modulen, Schlüsselwörtern und
weiteren Themen erhalten könnt. Wenn ihr euch im Hilfesystem befindet, seht ihr
mit ``help>`` eine Eingabeaufforderung. Ihr könnt nun einen Modulnamen eingeben,
:abbr:`z.B. (zum Beispiel)` ``float``, um die `Python-Dokumentation
<https://docs.python.org/>`_ zu diesem Typ zu durchsuchen.

``help()`` ist Teil der :doc:`pydoc <python3:library/pydoc>`-Bibliothek, die
Zugriff auf die in Python-Bibliotheken integrierte Dokumentation bietet. Da jede
Python-Installation mit einer vollständigen Dokumentation ausgeliefert wird,
habt ihr auch offline die gesamte Dokumentation zur Hand.

Alternativ könnt ihr ``help()`` auch gezielter anwenden, indem ihr einen Typ-
oder Variablennamen als Parameter übergebt, :abbr:`z.B. (zum Beispiel)`:

.. code-block:: python

    >>> x = 4.2
    >>> help(x)
    Help on float object:

    class float(object)
     |  float(x=0, /)
     |
     |  Convert a string or number to a floating point number, if possible.
     |
     |  Methods defined here:
     |
     |  __abs__(self, /)
     |      abs(self)
    ...

``dir()``, ``globals()`` und ``locals()``
-----------------------------------------

:py:func:`dir` ist eine weitere nützliche Funktion, die Objekte in einem
bestimmten Namensraum auflistet. Wenn ihr sie ohne Parameter verwendet, könnt
ihr herausfinden, welche Methoden und Daten lokal verfügbar sind. Alternativ
kann sie auch Objekte für ein Modul oder einen Typ auflisten.
