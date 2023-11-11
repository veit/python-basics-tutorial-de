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

Alternativ könnt ihr ``help()`` auch gezielter anwenden, indem ihr einen
Typ- oder Variablennamen als Parameter übergebt, :abbr:`z.B. (zum Beispiel)`:

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
bestimmten :doc:`Namensraum <oop/namespaces>` auflistet. Wenn ihr sie ohne
Parameter verwendet, könnt ihr herausfinden, welche Methoden und Daten lokal
verfügbar sind. Alternativ kann sie auch Objekte für ein Modul oder einen Typ
auflisten.

.. code-block:: python

   >>> dir()
   ['__annotations__', '__builtins__', '__doc__', '__loader__', '__name__', '__package__', '__spec__', 'x']
   >>> dir(x)
   ['__abs__', '__add__', '__bool__', '__ceil__', '__class__', '__delattr__', '__dir__', '__divmod__', '__doc__', '__eq__', '__float__', '__floor__', '__floordiv__', '__format__', '__ge__', '__getattribute__', '__getformat__', '__getnewargs__', '__getstate__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__int__', '__le__', '__lt__', '__mod__', '__mul__', '__ne__', '__neg__', '__new__', '__pos__', '__pow__', '__radd__', '__rdivmod__', '__reduce__', '__reduce_ex__', '__repr__', '__rfloordiv__', '__rmod__', '__rmul__', '__round__', '__rpow__', '__rsub__', '__rtruediv__', '__setattr__', '__sizeof__', '__str__', '__sub__', '__subclasshook__', '__truediv__', '__trunc__', 'as_integer_ratio', 'conjugate', 'fromhex', 'hex', 'imag', 'is_integer', 'real']

Im Gegensatz zu :py:func:`dir` zeigen sowohl :py:func:`globals` als auch
:py:func:`locals` die mit den Objekten verbundenen Werte an. Aktuell geben beide
Funktionen dasselbe zurück:

.. code-block:: python

   >>> globals()
   {'__name__': '__main__', '__doc__': None, '__package__': None, '__loader__': <class '_frozen_importlib.BuiltinImporter'>, '__spec__': None, '__annotations__': {}, '__builtins__': <module 'builtins' (built-in)>, 'x': 4.2}
