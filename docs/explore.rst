Python erkunden
===============

Egal, ob ihr :ref:`idle` oder die :ref:`interactive_shell` nutzt, es gibt einige
nützliche Funktionen, um Python zu erkunden.

.. code-block:: pycon

   >>> x = 4.2

``type()``
----------

Mit :py:func:`type` könnt ihr euch den Objekttyp anzeigen lassen, :abbr:`z. B.
(zum Beispiel)`:

.. code-block:: pycon

   >>> type(x)
   <class 'float'>

.. _help:

``help()``
----------

:py:func:`help` hat zwei verschiedene Modi. Wenn ihr :func:`help` eingebt, ruft
ihr das Hilfesystem auf, mit dem ihr Informationen zu Modulen, Schlüsselwörtern
und weiteren Themen erhalten könnt. Wenn ihr euch im Hilfesystem befindet, seht
ihr mit ``help>`` eine Eingabeaufforderung. Ihr könnt nun einen Modulnamen
eingeben, :abbr:`z.B. (zum Beispiel)` ``float``, um die `Python-Dokumentation
<https://docs.python.org/>`_ zu diesem Typ zu durchsuchen.

:func:`help` ist Teil der :doc:`pydoc <python3:library/pydoc>`-Bibliothek, die
Zugriff auf die in Python-Bibliotheken integrierte Dokumentation bietet. Da jede
Python-Installation mit einer vollständigen Dokumentation ausgeliefert wird,
habt ihr auch offline die gesamte Dokumentation zur Hand.

Alternativ könnt ihr :func:`help` auch gezielter anwenden, indem ihr einen
Typ- oder Variablennamen als Parameter übergebt, :abbr:`z. B. (zum Beispiel)`:

.. code-block:: pycon

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
     |  is_integer(self, /)
     |      Return True if the float is an integer.
     ...

So erfahrt ihr :abbr:`z. B. (zum Beispiel)`, dass ``x`` vom Typ ``float`` ist
und eine Funktion :func:`is_integer` hat, die ihr mit Punkt-Notation verwenden
könnt:

.. code-block:: pycon

   >>> x.is_integer()
   False

``id()``
--------

:py:func:`id` gibt die Identifikationsnummer eines Objekts an, :abbr:`z. B. (zum
Beispiel)`:

.. code-block:: pycon

   >>> id(x)
   4304262800

``dir()``
---------

:py:func:`dir` ist eine weitere nützliche Funktion um herauszufinden, welche
Methoden und Daten lokal oder für ein bestimmtes Objekt verfügbar sind:

.. code-block:: pycon

   >>> dir()
   ['__annotations__', '__builtins__', '__doc__', '__loader__', '__name__', '__package__', '__spec__', 'x']

So können wir uns :abbr:`z.B. (zum Beispiel)` mit ``dir(__builtins__)`` eine
Liste dessen anzeigen lassen, was in der Python-Standardbibliothek bereits
verfügbar ist:

.. code-block:: pycon

   >>> dir(__builtins__)
   ['ArithmeticError', 'AssertionError', 'AttributeError', 'BaseException', 'BaseExceptionGroup', 'BlockingIOError', 'BrokenPipeError', 'BufferError', 'BytesWarning', 'ChildProcessError', 'ConnectionAbortedError', 'ConnectionError', 'ConnectionRefusedError', 'ConnectionResetError', 'DeprecationWarning', 'EOFError', 'Ellipsis', 'EncodingWarning', 'EnvironmentError', 'Exception', 'ExceptionGroup', 'False', 'FileExistsError', 'FileNotFoundError', 'FloatingPointError', 'FutureWarning', 'GeneratorExit', 'IOError', 'ImportError', 'ImportWarning', 'IndentationError', 'IndexError', 'InterruptedError', 'IsADirectoryError', 'KeyError', 'KeyboardInterrupt', 'LookupError', 'MemoryError', 'ModuleNotFoundError', 'NameError', 'None', 'NotADirectoryError', 'NotImplemented', 'NotImplementedError', 'OSError', 'OverflowError', 'PendingDeprecationWarning', 'PermissionError', 'ProcessLookupError', 'PythonFinalizationError', 'RecursionError', 'ReferenceError', 'ResourceWarning', 'RuntimeError', 'RuntimeWarning', 'StopAsyncIteration', 'StopIteration', 'SyntaxError', 'SyntaxWarning', 'SystemError', 'SystemExit', 'TabError', 'TimeoutError', 'True', 'TypeError', 'UnboundLocalError', 'UnicodeDecodeError', 'UnicodeEncodeError', 'UnicodeError', 'UnicodeTranslateError', 'UnicodeWarning', 'UserWarning', 'ValueError', 'Warning', 'ZeroDivisionError', '_', '_IncompleteInputError', '__build_class__', '__debug__', '__doc__', '__import__', '__loader__', '__name__', '__package__', '__spec__', 'abs', 'aiter', 'all', 'anext', 'any', 'ascii', 'bin', 'bool', 'breakpoint', 'bytearray', 'bytes', 'callable', 'chr', 'classmethod', 'compile', 'complex', 'copyright', 'credits', 'delattr', 'dict', 'dir', 'divmod', 'enumerate', 'eval', 'exec', 'exit', 'filter', 'float', 'format', 'frozenset', 'getattr', 'globals', 'hasattr', 'hash', 'help', 'hex', 'id', 'input', 'int', 'isinstance', 'issubclass', 'iter', 'len', 'license', 'list', 'locals', 'map', 'max', 'memoryview', 'min', 'next', 'object', 'oct', 'open', 'ord', 'pow', 'print', 'property', 'quit', 'range', 'repr', 'reversed', 'round', 'set', 'setattr', 'slice', 'sorted', 'staticmethod', 'str', 'sum', 'super', 'tuple', 'type', 'vars', 'zip']
