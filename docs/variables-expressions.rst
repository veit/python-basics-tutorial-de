Variablen und Ausdrücke
=======================

Variablen
---------

Der am häufigsten verwendete Befehl in Python ist die Zuweisung. Der Python-Code
um eine Variable namens ``x`` zu erstellen, die den Wert ``π`` erhalten soll,
lautet:

.. code-block:: pycon

    >>> pi = 3.14159

In Python ist, anders als in vielen anderen Programmiersprachen, weder eine
Variablendeklaration noch ein Zeilenende-Begrenzer notwendig. Die Zeile wird
durch das Ende der Zeile abgeschlossen. Variablen werden automatisch erstellt,
wenn sie zum ersten Mal zugewiesen werden.

.. note::
   In Python sind Variablen Label, die auf Objekte verweisen. Eine beliebige
   Anzahl von Label können sich auf dasselbe Objekt beziehen, und wenn sich
   dieses Objekt ändert, ändert sich auch der Wert, auf den sich all diese
   Variablen beziehen. Um besser zu verstehen, was das bedeutet, seht euch das
   folgende Beispiel an:

   .. code-block:: pycon

      >>> x = [1, 2, 3]
      >>> y = x
      >>> y[0] = 4
      >>> print(x)
      [4, 2, 3]

   Variablen können sich jedoch auch auf Konstanten beziehen:

   .. code-block:: pycon

      >>> x = 1
      >>> y = x
      >>> z = y
      >>> y = 4
      >>> print(x, y, z)
      1 4 1

   In diesem Fall verweisen nach der dritten Zeile ``x``, ``y`` und ``z`` alle
   auf dasselbe unveränderliche Integer-Objekt mit dem Wert ``1``. Die nächste
   Zeile, ``y = 4``, bewirkt, dass ``y`` auf das Integer-Objekt ``4`` verweist,
   dies ändert jedoch nicht die Referenzen von ``x`` oder ``z``.

Python-Variablen können auf jedes beliebige Objekt gesetzt werden, während in
vielen anderen Sprachen Variablen nur im deklarierten Typ gespeichert werden
können.

Variablennamen unterscheiden Groß- und Kleinschreibung und können jedes
alphanumerische Zeichen sowie Unterstriche enthalten, müssen aber mit einem
Buchstaben oder Unterstrich beginnen.

.. note::
   Wenn ihr einen ``SyntaxError`` erhaltet, prüft, ob der Variablenname ein
   Schlüsselwort ist. Schlüsselwörter sind für die Verwendung in
   Python-Sprachkonstrukten reserviert, so dass ihr sie nicht zu Variablen
   machen könnt. Nach dem Aufruf von :ref:`help` könnt ihr ``keywords``
   eingeben, um die Schlüsselworte zu erhalten:

   .. code-block:: pycon

      >>> help()
      help> keywords

      Here is a list of the Python keywords.  Enter any keyword to get more help.

      False               class               from                or
      None                continue            global              pass
      True                def                 if                  raise
      and                 del                 import              return
      as                  elif                in                  try
      assert              else                is                  while
      async               except              lambda              with
      await               finally             nonlocal            yield
      break               for                 not

.. note::
   Ihr könnt mit einem Variablennamen eingebaute (engl.: *built-in*) Funktionen,
   Typen und andere Objekte überschreiben, sodass der Zugriff anschließend nur
   noch über das :doc:`builtins <python3:library/builtins>`-Modul erfolgen kann.
   Daher sollten diese Variablennamen nie verwendet werden. Eine Liste der
   :mod:`__builtins__`-Objekte erhaltet ihr mit:

   .. code-block:: pycon

      >>> dir(__builtins__)
      ['ArithmeticError', 'AssertionError', 'AttributeError', 'BaseException', 'BaseExceptionGroup', 'BlockingIOError', 'BrokenPipeError', 'BufferError', 'BytesWarning', 'ChildProcessError', 'ConnectionAbortedError', 'ConnectionError', 'ConnectionRefusedError', 'ConnectionResetError', 'DeprecationWarning', 'EOFError', 'Ellipsis', 'EncodingWarning', 'EnvironmentError', 'Exception', 'ExceptionGroup', 'False', 'FileExistsError', 'FileNotFoundError', 'FloatingPointError', 'FutureWarning', 'GeneratorExit', 'IOError', 'ImportError', 'ImportWarning', 'IndentationError', 'IndexError', 'InterruptedError', 'IsADirectoryError', 'KeyError', 'KeyboardInterrupt', 'LookupError', 'MemoryError', 'ModuleNotFoundError', 'NameError', 'None', 'NotADirectoryError', 'NotImplemented', 'NotImplementedError', 'OSError', 'OverflowError', 'PendingDeprecationWarning', 'PermissionError', 'ProcessLookupError', 'RecursionError', 'ReferenceError', 'ResourceWarning', 'RuntimeError', 'RuntimeWarning', 'StopAsyncIteration', 'StopIteration', 'SyntaxError', 'SyntaxWarning', 'SystemError', 'SystemExit', 'TabError', 'TimeoutError', 'True', 'TypeError', 'UnboundLocalError', 'UnicodeDecodeError', 'UnicodeEncodeError', 'UnicodeError', 'UnicodeTranslateError', 'UnicodeWarning', 'UserWarning', 'ValueError', 'Warning', 'ZeroDivisionError', '__build_class__', '__debug__', '__doc__', '__import__', '__loader__', '__name__', '__package__', '__spec__', 'abs', 'aiter', 'all', 'anext', 'any', 'ascii', 'bin', 'bool', 'breakpoint', 'bytearray', 'bytes', 'callable', 'chr', 'classmethod', 'compile', 'complex', 'copyright', 'credits', 'delattr', 'dict', 'dir', 'divmod', 'enumerate', 'eval', 'exec', 'exit', 'filter', 'float', 'format', 'frozenset', 'getattr', 'globals', 'hasattr', 'hash', 'help', 'hex', 'id', 'input', 'int', 'isinstance', 'issubclass', 'iter', 'len', 'license', 'list', 'locals', 'map', 'max', 'memoryview', 'min', 'next', 'object', 'oct', 'open', 'ord', 'pow', 'print', 'property', 'quit', 'range', 'repr', 'reversed', 'round', 'set', 'setattr', 'slice', 'sorted', 'staticmethod', 'str', 'sum', 'super', 'tuple', 'type', 'vars', 'zip']

Ausdrücke
---------

Python unterstützt arithmetische und ähnliche Ausdrücke. Der folgende Code
berechnet den Durchschnitt von ``x`` und ``y`` und speichert das Ergebnis in der
Variablen ``z``:

.. code-block:: pycon

    >>> x = 1
    >>> y = 2
    >>> z = (x + y) / 2

.. note::
   Arithmetische Operatoren, die nur ganze Zahlen verwenden, geben nicht immer
   eine ganze Zahl zurück. Ab Python 3 gibt die Division eine Fließkommazahl
   zurück. Wenn die traditionelle Ganzzahldivision mit einer Ganzzahl
   zurückgegeben werden soll, könnt ihr stattdessen ``//`` verwenden.

Checks
------

* Erstellt in der Python-Shell einige Variablen. Was passiert, wenn ihr
  Leerzeichen, Bindestriche oder andere Zeichen in den Variablennamen einfügt?

* Ändern sich die Ergebnisse, wenn ihr Klammern verwendet, um Zahlen auf
  verschiedene Weise zu gruppieren?

* Welche der folgenden Variablen- und Funktionsnamen sind eurer Meinung nach
  kein guter Python-Stil, und warum?

  ``var*``, ``varname``, ``func_name()``, ``varName``, ``VARNAME``,
  ``very_very__long_var_name``
