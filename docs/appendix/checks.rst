Checks
======

:doc:`/variables-expressions`
-----------------------------

* Erstellt in der Python-Shell einige Variablen. Was passiert, wenn ihr
  Leerzeichen, Bindestriche oder andere Zeichen in den Variablennamen einfügt?

  .. blacken-docs:off

  .. code-block:: pycon

     >>> x = 3
     >>> var fünf = 5
       File "<stdin>", line 1
         var fünf = 5
             ^^^^
     SyntaxError: invalid syntax
     >>> var_fünf = 5

  .. blacken-docs:on

* Ändern sich die Ergebnisse, wenn ihr Klammern verwendet, um Zahlen auf
  verschiedene Weise zu gruppieren?

  .. code-block:: pycon

     >>> 2 + 3 * 4 - 5 / 6
     13.166666666666666
     >>> (2 + 3) * 4 - 5 / 6
     19.166666666666668
     >>> 2 + (3 * 4) - 5 / 6
     13.166666666666666
     >>> 2 + 3 * (4 - 5) / 6
     1.5

+ Welche der folgenden Variablen- und Funktionsnamen sind eurer Meinung nach
  kein guter Python-Stil, und warum?

  ``var*``
      ❌ enthält ein unzulässiges Zeichen (``*``)
  ``varname``
      ✅ ok, aber mit Unterstich einfacher lesbar
  ``func_name()``
      ✅
  ``varName``
      ❌ Gemischte Groß- und Kleinschreibung
  ``VARNAME``
      ❌ Nur Großbuchstaben, schlecht lesbar
  ``very_very_long_var_name``
      ✅ ok, aber sehr lang und daher nur zu empfehlen, wenn zwischen vielen
      sehr ähnlichen Variablen unterschieden werden soll

:doc:`/types/numbers/index`
---------------------------

* Erstellt einige Zahlenvariablen (Ganzzahlen, Gleitkommazahlen und komplexe
  Zahlen). Experimentiert ein wenig damit, was passiert, wenn ihr Operationen
  mit ihnen durchführt, auch Typ-übergreifend.

  .. blacken-docs:off

  .. code-block:: pycon

     >>> x = 3
     >>> import math
     >>> pi = math.pi
     >>> pi
     3.141592653589793
     >>> c = 3j4
       File "<stdin>", line 1
         c = 3j4
              ^
     SyntaxError: invalid imaginary literal
     >>> c = 3 +4j
     >>> c
     (3+4j)
     >>> x * c
     (9+12j)
     >>> x + c
     (6+4j)

  .. blacken-docs:on

:doc:`/types/numbers/complex`
-----------------------------

* Ladet das Modul :mod:`math` und probiert einige der Funktionen aus. Ladet dann
  auch das Modul :mod:`cmath` und macht dasselbe.

  .. code-block:: pycon

     >>> from math import sqrt
     >>> sqrt(3)
     1.7320508075688772
     >>> from cmath import sqrt
     >>> sqrt(3)
     (1.7320508075688772+0j)

* Wie könnt ihr die Funktionen des :mod:`math`-Moduls wiederherstellen?

  .. code-block:: pycon

     >>> from math import sqrt
     >>> sqrt(3)
     1.7320508075688772

:doc:`/types/numbers/bool`
--------------------------

* Entscheidet, ob die folgenden Aussagen wahr oder falsch sind:

  * ``1`` → True
  * ``0`` → False
  * ``-1`` → True
  * ``[0]`` → True (Liste mit einem Task)
  * ``1 and 0`` → False
  * ``1 > 0 or []`` → True

:doc:`/types/sequences-sets/lists`
----------------------------------

* Was gibt :func:`len` für jeden der folgenden Fälle zurück:

  .. code-block:: pycon

     >>> len([3])
     1
     >>> len([])
     0
     >>> len([[1, [2, 3], 4], "5 6"])
     2

* Wie würdet ihr mit :func:`len` und Slices die zweite Hälfte einer Liste
  ermitteln, wenn ihr nicht wisst, wie groß sie ist?

  .. code-block:: pycon

     >>> l = [[1, [2, 3], 4], "5 6"]
     >>> l[len(l) // 2 :]
     ['5 6']

* Wie könntet ihr die letzten zwei Einträge einer Liste an den Anfang
  verschieben, ohne die Reihenfolge der beiden zu ändern?

  .. code-block:: pycon

     >>> l[-2:] + l[:2]
     ['5 6', 7, [1, [2, 3], 4], '5 6']

* Welcher der folgenden Fälle löst eine Exception aus?

  * ``min(["1", "2", "3"])``
  * ``max([1, 2, "3"])``
  * ``[1,2,3].count("1")``

  ``max([1, 2, "3"])``, da Strings und Ganzzahlen nicht verglichen werden
  können; daher ist es unmöglich, einen Maximalwert zu erhalten.

* Wenn ihr eine Liste ``l`` habt, wie könnt ihr daraus einen bestimmten Wert
  ``i`` entfernen?

  .. code-block:: pycon

     >>> if i in l:
     ...     l.remove(i)
     ...

  .. note::
     Mit diesem Code wird nur das erste Vorkommen von ``i`` entfernt. Um alle
     Vorkommen von ``i`` aus der Liste zu entfernen, könnte die Liste
     :abbr:`z.B. (zum Beispiel)` in den :doc:`Set
     </types/sequences-sets/sets>`-Typ umgewandelt werden:

     .. code-block:: pycon

        >>> l = set(l)
        >>> if i in l:
        ...     l.remove(i)
        ...
        >>> l = list(l)

     Dies ändert jedoch auch die Reihenfolge der Elemente.

* Wenn ihr eine verschachtelte Liste ``ll`` habt, wie könnt ihr eine Kopie
  ``nll`` dieser Liste erhalten, in der ihr die Elemente ändern könnt, ohne den
  Inhalt von ``ll`` zu verändern?

  .. code-block:: pycon

     >>> import copy
     >>> nll = copy.deepcopy(ll)

* Stellt sicher, dass das Objekt ``my_collection`` eine Liste ist, bevor ihr
  versucht, daran Daten anzuhängen.

  .. code-block:: pycon

     >>> my_collection = []
     >>> if isinstance(my_collection, list):
     ...     print(f"my_collection is a list")
     ...
     my_collection is a list

* Welche anderen Optionen könntet ihr neben der expliziten Überprüfung des Typs
  haben?

:doc:`/types/sequences-sets/tuples`
-----------------------------------

* Erläutert, warum die folgenden Operationen nicht auf das Tuple ``t``
  angewendet werden können:

  * ``t.append(1)``
  * ``t[2] = 2``
  * ``del t[3]``

  Alle Operation versuchen, das Tuple ``t`` zu ändern. Tuples können jedoch
  nicht verändert werden.

* Wie könnt ihr die Elemente eines Tuple sortieren?

  .. code-block:: pycon

     >>> sorted(t)

:doc:`/types/sequences-sets/sets`
---------------------------------

* Wieviele Elemente hat ein Set, wenn es aus der folgenden Liste
  ``[4, 2, 3, 2, 1]`` gebildet wird?

  Vier unterschiedliche Elemente.

:doc:`/types/dicts`
-------------------

* Angenommen, ihr habt die beiden Dictionaries ``x = {"a":1, "b":2, "c":3,
  "d":4}`` und ``y = {"a":5, "e":6, "f":7}``. Was wäre der Inhalt von ``x``,
  nachdem die folgenden Codeschnipsel ausgeführt wurden?

  .. code-block:: pycon

     >>> del x["b"]
     >>> z = x.setdefault("e", 8)
     >>> x.update(y)

  .. code-block:: pycon

     >>> x = {"a": 1, "b": 2, "c": 3, "d": 4}
     >>> y = {"a": 5, "e": 6, "f": 7}
     >>> del x["b"]
     >>> z = x.setdefault("e", 8)
     >>> x.update(y)
     >>> x
     {'a': 5, 'c': 3, 'd': 4, 'e': 6, 'f': 7}

* Welcher der folgenden Ausdrücke kann ein Schlüssel eines Dictionary sein:
  ``1``; ``"Veit"``; ``("Veit", [1])``; ``[("Veit", [1])]``; ``["Veit"]``;
  ``("Veit", "Tim", "Monique")``

  .. code-block:: pycon

     >>> d = {}
     >>> d[1] = None
     >>> d["Veit"] = None
     >>> d[("Veit", [1])]
     Traceback (most recent call last):
       File "<stdin>", line 1, in <module>
     TypeError: unhashable type: 'list'
     >>> d[["Veit"]] = None
     Traceback (most recent call last):
       File "<stdin>", line 1, in <module>
     TypeError: unhashable type: 'list'
     >>> d[("Veit", "Tim", "Monique")] = None

* Ihr könnt ein :doc:`Dictionary </types/dicts>` verwenden, und das wie ein
  Tabelle einer Tabellenkalkulation verwenden, indem ihr
  :doc:`/types/sequences-sets/tuples` als Schlüssel Zeilen- und Spaltenwerte
  verwendet. Schreibt Beispielcode, um Werte hinzuzufügen und wieder abzufragen.

  .. code-block:: pycon

     >>> tabular = {}
     >>> tabular[("A", 0)] = 1
     >>> tabular[("A", 1)] = 2
     >>> tabular[("B", 0)] = 3
     >>> tabular[("B", 1)] = 4
     >>> print(tabular[("A", 1)])
     2

* Wie könnt ihr alle Dubletten aus einer Liste entfernen **ohne** die
  Reihenfolge der Elemente in der Liste zu ändern?

  Hierfür können die Schlüssel eines :doc:`/types/dicts` verwendet werden:

  .. code-block:: pycon

     >>> list(dict.fromkeys(l))

:doc:`/types/strings/index`
---------------------------

* Könnt ihr :abbr:`z.B. (zum Beispiel)` eine Zeichenkette mit einer ganzen Zahl
  addieren oder multiplizieren, oder mit einer Gleitkommazahl oder einer
  komplexen Zahl?

  .. code-block:: pycon

     >>> x = 3
     >>> c = 3 + 4j
     >>> snake = "🐍"
     >>> x + snake
     Traceback (most recent call last):
       File "<stdin>", line 1, in <module>
     TypeError: unsupported operand type(s) for +: 'int' and 'str'
     >>> x * snake
     '🐍🐍🐍'
     >>> c + snake
     Traceback (most recent call last):
       File "<stdin>", line 1, in <module>
     TypeError: unsupported operand type(s) for +: 'complex' and 'str'
     >>> c * snake
     Traceback (most recent call last):
       File "<stdin>", line 1, in <module>
     TypeError: can't multiply sequence by non-int of type 'complex'

:doc:`/types/strings/operators-functions`
-----------------------------------------

* Welche der folgenden Zeichenketten können nicht in Zahlen umgewandelt werden
  und warum?

  .. blacken-docs:off

  .. code-block:: pycon

     >>> int("1e2")
     Traceback (most recent call last):
       File "<stdin>", line 1, in <module>
     ValueError: invalid literal for int() with base 10: '1e2'
     >>> int(1e+2)
     100
     >>> int("1+2")
     Traceback (most recent call last):
       File "<stdin>", line 1, in <module>
     ValueError: invalid literal for int() with base 10: '1+2'
     >>> int("+2")
     2

  .. blacken-docs:on

:doc:`/types/strings/built-in-modules/string`
---------------------------------------------

* Wie könnt ihr eine Überschrift wie ``variables and expressions`` so abändern,
  dass sie statt Leerzeichen Bindestriche  enthält und so besser als Dateinamen
  verwendet werden kann?

  .. code-block:: pycon

     >>> ve = "variables and expressions"
     >>> "-".join(ve.split())
     'variables-and-expressions'

* Wenn ihr überprüfen wollt, ob eine Zeile mit ``.. note::`` beginnt, welche
  Methode würdet ihr verwenden? Gibt es auch noch andere Möglichkeiten?

  .. code-block:: pycon

     >>> x.startswith(".. note::")
     True
     >>> x[:9] == ".. note::"
     True

* Angenommen, ihr habt eine Zeichenkette mit Ausrufezeichen, Anführungszeichen
  und Zeilenumbruch. Wie können diese aus der Zeichenkette entfernt werden?

  .. code-block:: pycon

     >>> hipy = "„Hello Pythonistas!“\n"
     >>> hipy.strip("„“!\n")
     'Hello Pythonistas'

* Wie könnt ihr **alle** Leerräume und Satzzeichen aus einer Zeichenfolge in
  einen Bindestrich (``-``) ändern?

  .. code-block:: pycon

     >>> from string import punctuation, whitespace
     >>> chars = punctuation + whitespace
     >>> subs = str.maketrans(chars, len(chars) * "-")
     >>> hipy = "Hello Pythonistas!\n"
     >>> hipy.translate(subs)
     'Hello-Pythonistas--'

:doc:`/types/strings/built-in-modules/re`
-----------------------------------------

* Welchen regulären Ausdruck würdet ihr verwenden, um Zeichenfolgen zu finden,
  die die Zahlen zwischen -3 und +3 darstellen?

  ``r"-?[0-3]"`` oder ``r"-{0,1}[0-3]"``

  ``?``
      ist ein Quantifizierer für ein oder kein Vorkommen.

* Welchen regulären Ausdruck würdet ihr verwenden, um Hexadezimalwerte zu
  finden?

  ``r"0[xX][0-9a-fA-F]+"``
      entspricht einem Ausdruck, der mit ``0`` beginnt, gefolgt von einem
      kleinen oder großen ``x``, gefolgt von einem oder mehreren Zeichen in den
      Bereichen ``0-9``, ``a-f`` oder ``A-F``.

:doc:`/types/strings/input`
---------------------------

* Wie könnt ihr mit der :func:`input`-Funktion String- und Integer-Werte
  erhalten?

  .. code-block:: pycon

     >>> year_birth = input("Geburtsjahr: ")
     Geburtsjahr: 1964
     >>> type(year_birth)
     <class 'str'>
     >>> year_birth = int(input("Geburtsjahr: "))
     Geburtsjahr: 1964
     >>> type(year_birth)
     <class 'int'>

* Wie wirkt es sich aus, wenn ihr :func:`int` nicht verwendet um den Aufruf von
  :func:`input` für Integer-Eingaben zu verwenden?

  .. code-block:: pycon

     >>> import datetime
     >>> current = datetime.datetime.now()
     >>> year = current.year
     >>> year_birth = input("Geburtsjahr? ")
     Geburtsjahr? 1964
     >>> age = year - year_birth
     Traceback (most recent call last):
       File "<stdin>", line 1, in <module>
     TypeError: unsupported operand type(s) for -: 'int' and 'str'

* Könnt ihr den Code so abändern, dass er eine Fließkommazahl akzeptiert?

  .. code-block:: pycon

     >>> import datetime
     >>> current = datetime.datetime.now()
     >>> year = current.year
     >>> year_birth = float(input("Geburtsjahr: "))
     Geburtsjahr: 1964
     >>> type(year_birth)
     <class 'float'>

* Was passiert, wenn ihr einen *falschen* Werttyp eingebt?

  .. code-block:: pycon

     >>> import datetime
     >>> current = datetime.datetime.now()
     >>> year = current.year
     >>> year_birth = int(input("Geburtsjahr: "))
     Geburtsjahr: Schaltjahr
     Traceback (most recent call last):
       File "<stdin>", line 1, in <module>
     ValueError: invalid literal for int() with base 10: 'Schaltjahr'

* Schreibt den Code, um für drei User jeweils nach Namen und Alter zu fragen.
  Nachdem die Werte eingegeben wurden, fragt nach einem der Namen und gebt das
  zugehörige Alter aus.

  .. code-block:: pycon

     >>> personal_data = {}
     >>> for i in range(3):
     ...     name = input("Name? ")
     ...     age = int(input("Age? "))
     ...     personal_data[name] = age
     ...
     Name? Veit
     Age? 60
     Name? Tim
     Age? 35
     Name? Monique
     Age? 37
     >>> who = input("Who? ")
     Who? Veit
     >>> print(personal_data[who])
     60

:doc:`/control-flow/loops`
--------------------------

* Entfernt aus der Liste ``x = [ -2, -1, 0, 1, 2, 3]``, alle negativen Zahlen.

  .. code-block:: pycon

     >>> x = [-2, -1, 0, 1, 2, 3]
     >>> pos = []
     >>> for i in x:
     ...     if i >= 0:
     ...         pos.append(i)
     ...
     >>> pos
     [0, 1, 2, 3]

* Welche List-Comprehension würdet ihr verwenden, um zum selben Ergebnis zu
  kommen?

  .. code-block:: pycon

     >>> x = [-2, -1, 0, 1, 2, 3]
     >>> pos = [i for i in x if i >= 0]
     >>> pos
     [0, 1, 2, 3]

* Wie würdet ihr die Gesamtzahl der negativen Zahlen in der Liste ``[[-1, 0, 1],
  [-1, 1, 3], [-2, 0, 2]]`` zählen?

  .. code-block:: pycon

     >>> x = [[-1, 0, 1], [-1, 1, 3], [-2, 0, 2]]
     >>> neg = 0
     >>> for row in x:
     ...     for col in row:
     ...         if col < 0:
     ...             neg += 1
     ...
     >>> neg
     3

* Erstellt einen Generator, der nur ungerade Zahlen von 1 bis 10 liefert.

  .. tip::
     Eine Zahl ist ungerade, wenn bei der Division durch 2 ein Rest übrig
     bleibt; also wenn ``% 2`` wahr ist.

  .. code-block:: pycon

     >>> x = (x for x in range(10) if x % 2)
     >>> for i in x:
     ...     print(i)
     ...
     1
     3
     5
     7
     9

* Schreibt ein :doc:`Dict </types/dicts>` mit den Kantenlängen und Volumen von
  Würfeln.

  .. code-block:: pycon

     >>> {x: x**3 for x in range(1, 5)}
     {1: 1, 2: 8, 3: 27, 4: 64}

:doc:`/control-flow/exceptions`
-------------------------------

* Schreibt  Code, der zwei Zahlen erhält und die erste Zahl durch die zweite
  dividiert. Prüft, ob der :class:`python3:ZeroDivisionError` auftritt, wenn die
  zweite Zahl ``0`` ist, und fangt diese ab.

  .. code-block:: pycon

     >>> x = int(input("Please enter an integer: "))
     Please enter an integer: 7
     >>> y = int(input("Please enter an integer: "))
     Please enter an integer: 6
     >>> try:
     ...     z = x / y
     ... except ZeroDivisionError as e:
     ...     print("It cannot be divided by 0!")
     ...
     >>> z
     1.1666666666666667
     >>> y = int(input("Please enter an integer: "))
     Please enter an integer: 0
     >>> try:
     ...     print("It cannot be divided by 0!")
     ... except ZeroDivisionError as e:
     ...     print("It cannot be divided by 0!")
     ...
     It cannot be divided by 0!

* Wenn :class:`MyError` von :class:`Exception` erbt, was ist dann der
  Unterschied zwischen ``except Exception as e`` und ``except MyError as e``?

  Die erste fängt jede Ausnahme ab, die von :class:`Exception` erbt, während die
  zweite nur :class:`MyError`-Ausnahmen abfängt.

* Schreibt ein einfaches Programm, das eine Zahl erhält und dann die Anweisung
  :func:`assert` verwendet, um eine :class:`python3:Exception` auszulösen, wenn
  die Zahl ``0`` ist.

  .. code-block:: pycon

     >>> x = int(input("Please enter an integer that is not zero: "))
     Please enter an integer that is not zero: 0
     >>> assert x != 0, "The integer must not be zero."
     Traceback (most recent call last):
       File "<stdin>", line 1, in <module>
     AssertionError: The integer must not be zero.

* Schreibt eine benutzerdefinierte Ausnahme :class:`Outliers`, die eine
  :class:`Exception` auslöst, wenn die Variable ``x`` größer oder kleiner als
  ``3`` ist?

  .. code-block:: pycon

     >>> class Outliers(Exception):
     ...     pass
     ...
     >>> x = -4
     >>> if abs(x) > 3:
     ...     raise Outliers(f"The value {x} is an outlier")
     ...
     Traceback (most recent call last):
       File "<stdin>", line 2, in <module>
     Outliers: The value -4 is an outlier

* Handelt es sich bei der Überprüfung, ob ein Objekt eine Liste ist
  (:ref:`Check: Listen <check-list>`) um eine Programmierung im Stil von
  :abbr:`LBYL (look before you leap)` oder :abbr:`EAFP (easier to ask
  forgiveness than permission)`?

  Das ist :abbr:`LBYL (look before you leap)`-Programmierung. Erst wenn ihr
  :func:`append` einem ``try... except``-Block packt und
  :class:`TypeError`-Exceptions abfangt, wird es etwas mehr :abbr:`EAFP (easier
  to ask forgiveness than permission)`.

:doc:`/functions/params`
------------------------

* Schreibt eine Funktion, die eine beliebige Anzahl von unbenannten Argumenten
  annehmen und deren Werte in umgekehrter Reihenfolge ausgeben kann?

  .. code-block:: pycon

     >>> def reverse(*args):
     ...     reverse_list = []
     ...     for i in reversed(*args):
     ...         reverse_list.append(i)
     ...     return reverse_list
     ...
     >>> values = input("Values separated by commas: ")
     Values separated by commas: 1,3,2,4
     >>> value_list = values.split(",")
     >>> reverse(value_list)
     ['4', '3', '2', '1']

:doc:`/functions/variables`
---------------------------

* Angenommen, ``x = 1``, :func:`func` setze die lokale Variable ``x`` auf ``2``
  und :func:`gfunc` die globale Variable ``x`` auf ``3``, welchen Wert nimmt
  ``x`` an, nachdem :func:`func` und :func:`gfunc` durchlaufen wurden?

  .. code-block:: pycon

     >>> x = 1
     >>> def func():
     ...     x = 2
     ...
     >>> def gfunc():
     ...     global x
     ...     x = 3
     ...
     >>> func()
     >>> x
     1
     >>> gfunc()
     >>> x
     3

:doc:`/modules/index`
---------------------

* Wenn ihr ein Modul :mod:`my_math` erstellt habt, das eine Funktion
  :func:`divide` enthält, welche Möglichkeiten gibt es, diese Funktion zu
  importieren und dann zu verwenden? Was sind die Vor- und Nachteile der
  einzelnen Möglichkeiten?

  .. code-block:: pycon

     >>> import my_math
     >>> my_math.divide(..., ...)

  .. code-block:: pycon

     >>> from my_math import divide
     >>> divide(..., ...)

  Die erste Lösung wird oft bevorzugt, da es keinen Konflikt zwischen den
  Bezeichnern in :mod:`my_math` und dem importierenden Namespace geben wird.
  Diese Lösung ist jedoch ein wenig aufwändiger.

* Eine Variable ``min`` ist im Modul :mod:`scope.py` enthalten. In welchem der
  folgenden Kontexte kann ``min`` verwendet werden?

  #. Mit dem Modul selbst
  #. Innerhalb der Funktion :func:`scope` des Moduls
  #. Innerhalb eines Skripts, das das Modul :mod:`scope.py` importiert hat

  1. und 2., aber nicht 3.

* Packt die Funktionen, die ihr am Ende von :doc:`/functions/decorators`
  erstellt habt, als eigenständiges Modul. Dabei soll die Funktionen zunächst
  lediglich von einem anderen Skript aus vollständig nutzbar sein.

  .. literalinclude:: example_mod.py
     :caption: example_mod.py
     :name: example_mod.py
     :language: python

  .. literalinclude:: my_script.py
     :caption: my_script.py
     :name: my_script.py
     :language: python

* Macht euer Modul ausführbar.

  .. literalinclude:: example_mod2.py
     :diff: example_mod.py
     :language: python

.. _wcargv_stdin:

* Schreibt eure Version des :mod:`wc`-Dienstprogramms so um, dass es sowohl die
  Unterscheidung zwischen Bytes und Zeichen als auch die Möglichkeit, aus
  Dateien und von der Standardeingabe zu lesen, implementiert.

  .. literalinclude:: /modules/wcargv_stdin.py
     :diff: /modules/wcargv.py

:doc:`/oop/classes`
-------------------

* Schreibt eine :class:`Triangle`-Klasse, die auch die Fläche berechnen kann.

  .. code-block:: python

     class Triangle:
         def __init__(self, width, height):
             self.width = width
             self.height = height

         def area(self):
             return 0.5 * self.width * self.height

:doc:`/oop/methods`
-------------------

* Schreibt eine Klassenmethode, die ähnlich wie :func:`circumferences` ist, aber
  die Gesamtfläche aller Kreise zurückgibt.

  .. code-block:: python

     def area(self):
         return self.diameter**2 / 4 * self.__class__.pi


     @classmethod
     def areas(cls):
         """Class method to sum all areas."""
         careasum = 0
         for c in cls.circles:
             careasum = careasum + c.area()
         return careasum

:doc:`/oop/inheritance`
-----------------------

* Schreibt den Code für eine :class:`Triangle`-Klasse um, sodass sie von
  :class:`Form` erbt.

  .. code-block:: pycon

     >>> class Form:
     ...     def __init__(self, x=0, y=0):
     ...         self.x = x
     ...         self.y = y
     ...
     >>> class Triangle(Form):
     ...     def __init__(self, width=1, height=1, x=0, y=0):
     ...         super().__init__(x, y)
     ...         self.length = length
     ...         self.height = height
     ...

* Wie würdet ihr den Code schreiben, um eine Methode :func:`area` für die Klasse
  :class:`Triangle` hinzuzufügen? Sollte die Methode :func:`area` in die
  Basisklasse :class:`Form` verschoben und an :class:`Circle`, :class:`Square`
  und :class:`Triangle` vererbt werden? Welche Probleme würde diese Änderung
  verursachen?

  Es ist sinnvoll, die :func:`area`-Methode in eine :class:`Triangle`-Klasse zu
  packen; aber sie in :class:`Form` zu packen, wäre nicht sehr hilfreich, weil
  verschiedene Typen von :class:`Form` ihre eigenen Berechnungen der Fläche
  haben. Jede abgeleitete Form würde ohnehin die Basismethode :func:`area`
  überschreiben.

:doc:`/oop/types`
-----------------

* Was wäre der Unterschied zwischen der Verwendung von :func:`type` und
  :func:`isinstance` in :ref:`Check: Listen <check-list>`?

  Mit :func:`type` würdet ihr nur Listen erhalten, nicht aber Instanzen von
  Listen.

:doc:`/oop/private`
-------------------

* Ändert den Code der Klasse :class:`Triangle`, um die Dimensionsvariablen
  privat zu machen. Welche Einschränkung wird diese Änderung für die Verwendung
  der Klasse mit sich bringen?

  .. code-block:: pycon

     >>> class Triangle:
     ...     def __init__(self, x, y):
     ...         self.__x = x
     ...         self.__y = y
     ...

  Die Dimensionsvariablen sind außerhalb der Klasse nicht mehr über ``.x`` und
  ``.y`` verfügbar.

* Aktualisiert die Dimensionen der Klasse :class:`Triangle`, damit sie
  Eigenschaften mit Getter- und Setter-Methoden sind, die keine negativen Größen
  zulassen.

  .. code-block:: pycon

     >>> class Triangle:
     ...     def __init__(self, x, y):
     ...         self.__x = x
     ...         self.__y = y
     ...     @property
     ...     def x(self):
     ...         return self.__x
     ...     @x.setter
     ...     def x(self, new_x):
     ...         if new_x >= 0:
     ...             self.__x = new_x
     ...     @property
     ...     def y(self):
     ...         return self.__y
     ...     @y.setter
     ...     def y(self, new_y):
     ...         if new_y >= 0:
     ...             self.__y = new_y
     ...
     >>> t1 = Triangle(-2, 2)
     Traceback (most recent call last):
       File "<stdin>", line 1, in <module>
       File "<stdin>", line 6, in __init__
     ValueError: The number must be greater or equal to zero.
     >>> t1 = Triangle(2, 2)
     >>> t1.x = -2
     Traceback (most recent call last):
       File "<stdin>", line 1, in <module>
       File "<stdin>", line 13, in x
     ValueError: The number must be greater or equal to zero.
     >>> t1.x = 3
     >>> t1.x
     3

:doc:`/packs/distribution`
--------------------------

* Wenn ihr ein Paket für eine Aufgabenverwaltung erstellen wollt, das die
  Aufgaben in eine Datenbank schreibt und über ein Python-:abbr:`API (engl.:
  Application Programming Interface)` und eine Befehlszeilenschnittstelle
  (:abbr:`CLI (engl.: Command-Line Interface)` bereitstellt, wie würdet ihr die
  Dateien strukturieren?

  Das Paket führt drei Arten von Aktionen durch:

  * Zugriffe auf die Datenbank
  * Bereitstellen einer Python-API
  * Bereitstellen einer Befehlszeilenschnittstelle

  .. code-block:: console

     ├── README.rst
     ├── pyproject.toml
     └── src
         └── cusy
             └── tasks
                 ├── __init__.py
                 ├── api.py
                 ├── cli.py
                 └── db.py

* Überlegt euch, wie ihr die oben genannten Aufgaben erledigen wollt. Welche
  Bibliotheken und Module fallen euch ein, die diese Aufgabe erfüllen könnten?
  Skizziert den Code für die Module der Python-API, der
  Befehlszeilenschnittstelle und der Datenbankanbindung.

  Ich würde in :file:`src/cusy/tasks/db.py` eine :class:`DB`-Klasse für die
  Kommunikation mit der Datenbank erstellen, im folgenden Beispiel zu `tinydb
  <https://tinydb.readthedocs.io/en/latest/>`_:

  .. code-block:: python

     import tinydb


     class DB:
         def __init__(self, db_path, db_file_prefix):
             self._db = tinydb.TinyDB(
                 db_path / f"{db_file_prefix}.json", create_dirs=True
             )

         def create(self, task: dict):
             """Create a task

             Returns:
                 id: The tasks id.
             """

             return id

         def read(self, id: int):
             """Reads a task.

             Args:
                 id (int): The task id of a task.
             Returns:
                 task: The task object."""
             return task

         def update(self, id: int, mods):
             """Update a task in the database.

             Args:
                 id (int): The task id of a task.
                 mods (Task): The modifications to be made to this task.
             """
             self._db.update(changes, doc_ids=[id])

         def delete(self, id: int):
             """Deletes a task in the database.

             Args:
                 id (int): The task id of a task.
             """
             self._db.remove(doc_ids=[id])

         def close(self):
             """Closes the database connection."""
             self._db.close()

  Dann würde ich in :file:`src/cusy/tasks/api` :func:`dataclass` verwenden, um
  eine :class:`Task`-Klasse zu erstellen:

  .. code-block:: python

     from dataclasses import dataclass, field


     @dataclass
     class Task:
         summary: str = None
         owner: str = None
         state: str = "todo"
         id: int = field(default=None, compare=False)


     class TasksException(Exception):
         pass


     class TasksDB:
         def __init__(self, db_path):
             self._db_path = db_path
             self._db = DB(db_path, ".tasks_db")

         def add_task(self, task: Task):
             return

         def get_task(self, task: Task):
             return

         def update_task(self, task: Task):
             return

         def delete_task(self, task: Task):
             return

         def close(self):
             self._db.close()

         def path(self):
             return self._db_path

  In :file:`src/cusy/tasks/__init__.py` werden dann :class:`TasksException`
  :class:`Task` und  :class:`TasksDB` bereitgestellt:

  .. code-block:: python

     from .api import TasksException, Task, TasksDB

  .. seealso::
     Ein vollständiges Beispiel findet ihr in `github.com/cusyio/cusy.tasks
     <https://github.com/cusyio/cusy.tasks/>`_.

:doc:`/save-data/files-directories`
-----------------------------------

* Verwendet die Funktionen des :mod:`python3:pathlib`-Moduls, um einen Pfad zu
  einer Datei namens :file:`instance.log` zu nehmen und einen neuen Dateipfad im
  selben Verzeichnis für eine Datei namens :file:`instance.log1` zu erstellen.

  .. code-block:: pycon

     >>> from pathlib import Path
     >>> l = Path("logs", "instance.log")
     >>> l1 = Path("logs", "instance.log1")
     >>> l.rename(l1)
     PosixPath('logs/instance.log1')

* Öffnet eine Datei :file:`my_file.txt` und fügt zusätzlichen Text am Ende der
  Datei ein. Welchen Befehl würdet ihr verwenden, um :file:`my_file.txt` zu
  öffnen? Welchen Befehl würdet ihr verwenden, um die Datei erneut zu öffnen und
  von Anfang an zu lesen?

  .. code-block:: pycon

     >>> from pathlib import Path
     >>> p = Path("docs", "save-data", "myfile.txt")
     >>> p.write_text("Hi, Pythonistas!\n")
     17
     >>> p.read_text()
     'Hi, Pythonistas!\n'

* Wenn ihr euch die `Manpage für das wc-Dienstprogramm
  <https://linux.die.net/man/1/wc>`_ anseht, seht ihr zwei
  Befehlszeilenoptionen:

  ``-c``
      zählt die Bytes in der Datei
  ``-m``
      zählt die Zeichen, die im Falle einiger Unicode-Zeichen zwei oder mehr
      Bytes lang sein können

  Außerdem sollte unser Modul, wenn eine Datei angegeben wird, aus dieser Datei
  lesen und sie verarbeiten, aber wenn keine Datei angegeben wird, sollte es aus
  ``stdin`` lesen und verarbeiten.

  .. seealso::
     :ref:`_wcargv_stdin.py <wcargv_stdin>`

* Wenn ein Kontext-Manager in einem Skript verwendet wird, das mehrere Dateien
  liest und/oder schreibt, welche der folgenden Ansätze wäre eurer Meinung nach
  am besten?

  #. Legt das gesamte Skript in einen Block, der von einer ``with``-Anweisung
     verwaltet wird.
  #. Verwendet eine ``with``-Anweisung für alle Lesevorgänge und eine weitere
     für alle Schreibvorgänge.
  #. Verwendet jedes Mal eine ``with``-Anweisung, wenn ihr eine Datei lest oder
     schreibt, :abbr:`d.h. (das heißt)` für jede Zeile.
  #. Verwendet für jede Datei, die ihr lest oder schreibt, eine
     ``with``-Anweisung.

  Wahrscheinlich ist 4. der beste Ansatz, da ein Teil der Aufgabe des
  Kontextmanagers beim Dateizugriff darin besteht, sicherzustellen, dass eine
  Datei geschlossen ist.

* Archiviert :file:`*.txt`-Dateien aus dem aktuellen Verzeichnis im Verzeichnis
  :file:`archive` als :file:`*.zip`-Dateien mit dem aktuellen Datum als
  Dateiname.

  * Welche Module benötigt ihr hierfür?

    :mod:`python3:datetime`, :mod:`python3:pathlib` und :mod:`python3:zipfile`.

  * Schreibt eine mögliche Lösung.

    .. code-block:: pycon
       :linenos:

       >>> import datetime
       >>> import pathlib
       >>> import zipfile
       >>> file_pattern = "*.txt"
       >>> archive_path = "archive"
       >>> today = f"{datetime.date.today():%Y-%m-%d}"
       >>> cur_path = pathlib.Path(".")
       >>> paths = cur_path.glob(file_pattern)
       >>> zip_path = cur_path.joinpath(archive_path, today + ".zip")
       >>> zip_file = zipfile.ZipFile(str(zip_path), "w")
       >>> for path in paths:
       ...     zip_file.write(str(path))
       ...     path.unlink()
       ...

    Zeile 9
        erstellt den Pfad zur ZIP-Datei im Archivverzeichnis.
    Zeile 10
        öffnet das neue ZIP-Dateiobjekt zum Schreiben; :func:`str` wird
        benötigt, um einen Pfad in eine Zeichenkette umzuwandeln.
    Zeile 12
        schreibt die aktuelle Datei in die Zip-Datei.
    Zeile 13
        entfernt die aktuelle Datei aus dem Arbeitsverzeichnis.

:doc:`/save-data/modules`
-------------------------

* Welche Anwendungsfälle könnt ihr euch vorstellen, in denen das
  :mod:`python3:struct`-Modul für das Lesen oder Schreiben von Binärdaten
  nützlich wäre?

  * beim Lesen und Schreiben einer Binärdatei
  * beim Lesen von einer externen Schnittstelle, wobei die Daten genau so
    gespeichert werden sollen, wie sie übermittelt wurden

* Warum könnte :doc:`pickle <python3:library/pickle>` für die folgenden
  Anwendungsfälle geeignet sein oder auch nicht:

  #. Speichern einiger Zustandsvariablen von einem Durchlauf zum nächsten ✅
  #. Aufbewahren von Auswertungsergebnissen ❌, da Pickle abhängig von der
     jeweiligen Python-Version sind
  #. Speichern von Benutzernamen und Passwörtern ❌, da Pickle nicht sicher sind
  #. Speichern eines großen Wörterbuchs mit englischen Begriffen ❌, da der
     gesamte Pickle in den Speicher geladen werden müsste
