Zahlen
======

Die vier Zahlentypen von Python sind ganze Zahlen, Gleitkommazahlen, komplexe
Zahlen und Boolesche Zahlen:

+-----------------------+-----------------------------------------------+
| Typ                   | Beispiele                                     |
+=======================+===============================================+
| Ganzzahlen            | ``-1``, ``42``, ``90000000``                  |
+-----------------------+-----------------------------------------------+
| Gleitkommazahlen      | ``90000000.0``, ``-0.005``, ``9e7``, ``-5e-3``|
+-----------------------+-----------------------------------------------+
| Komplexe Zahlen       | ``3 + 2j``, ``-4- 2j``, ``4.2 + 6.3j``        |
+-----------------------+-----------------------------------------------+
| Boolesche Zahlen      | ``True``, ``False``                           |
+-----------------------+-----------------------------------------------+

Sie können mit den arithmetischen Operatoren manipuliert werden:

+-----------------------+-----------------------------------------------+
| Operator              | Beschreibung                                  |
+=======================+===============================================+
| ``+``                 | Addition                                      |
+-----------------------+-----------------------------------------------+
| ``-``                 | Subtraktion                                   |
+-----------------------+-----------------------------------------------+
| ``*``                 | Multiplikation                                |
+-----------------------+-----------------------------------------------+
| ``/``, ``//``         | Division [#]_                                 |
+-----------------------+-----------------------------------------------+
| ``**``                | Potenzierung                                  |
+-----------------------+-----------------------------------------------+
| ``%``                 | Modulus                                       |
+-----------------------+-----------------------------------------------+

.. [#] Die Division ganzer Zahlen mit ``/`` führt zu einem Float, und die
       Division ganzer Zahlen mit ``//`` führt zu einer Ganzzahl, die
       abgeschnitten wird.

.. note::
   Ganzzahlen können unbegrenzt groß sein, begrenzt nur durch den verfügbaren
   Speicher.

Beispiele:

.. code-block:: python

    >>> 8 + 3 - 5 * 3
    -4
    >>> 8 / 3
    2.6666666666666665
    >>> 8 // 3
    2
    >>> x = 4.2 ** 3.4
    >>> x
    131.53689544409096
    >>> 9e7 * -5e-3
    -450000.0
    >>> -5e-3 ** 3
    -1.2500000000000002e-07

Komplexe Zahlen
---------------

Komplexe Zahlen bestehen aus einem Realteil und einem
`Imaginärteil <https://de.wikipedia.org/wiki/Imagin%C3%A4re_Zahl>`_, der in
Python den Suffix ``j`` erhält.

.. code-block:: python

    >>> 7 + 2j
    (7+2j)

.. note::

    Python drückt die resultierende komplexe Zahl in Klammern aus, um
    anzuzeigen, dass die Ausgabe den Wert eines einzelnen Objekts darstellt:

.. code-block:: python

    >>> (7+2j) - (4+4j)
    (3-2j)

.. code-block:: python

    >>> 2j * 4j
    (-8+0j)

.. note::

    Die Berechnung von ``2j * 4j`` ergibt die erwartete Antwort von ``-8``, aber
    das Ergebnis bleibt ein Python-Objekt für komplexe Zahlen. Komplexe Zahlen
    werden nie automatisch in entsprechende reelle oder ganzzahlige Objekte
    umgewandelt. Ihr könnt aber leicht auf ihre realen und imaginären Teile mit
    ``real`` und ``imag`` zugreifen.

.. code-block:: python

    >>> x = 2j * 4j
    >>> x
    (-8+0j)
    >>> x.real
    -8.0
    >>> x.imag
    0.0

.. note::

    Die Real- und Imaginärteile einer komplexen Zahl werden immer als
    Fließkommazahlen zurückgegeben.

Built-in numerische Funktionen
------------------------------

Mehrere eingebaute Funktionen können mit Zahlen arbeiten:

:func:`python3:abs`
    gibt den absoluten Wert einer Zahl zurück. Dabei kann das Argument eine
    Ganzzahl, eine Fließkommazahl oder ein Objekt sein, das ``__abs__()``
    implementiert. Bei komplexen Zahlen als Argument wird ihr Betrag
    zurückgegeben.
:func:`python3:divmod`
    nimmt zwei (nicht-komplexe) Zahlen als Argumente und gibt ein Zahlenpaar
    zurück, das aus ihrem Quotienten und dem Rest besteht, wenn eine ganzzahlige
    Division verwendet wird.
:class:`python3:float`
    Gibt eine Fließkommazahl zurück, die aus einer Zahl oder Zeichenkette ``x``
    gebildet wird.
:func:`python3:hex`
    konvertiert eine Integer-Zahl in eine klein geschriebene hexadezimale
    Zeichenkette mit dem Präfix ``0x``.
:class:`python3:int`
    gibt ein Integer-Objekt zurück, das aus einer Zahl oder Zeichenkette ``x``
    konstruiert wurde, oder ``0``, wenn keine Argumente angegeben werden.
:func:`python3:max`
    gibt das größte Element in einem :term:`python3:iterable` oder das größte
    von zwei oder mehr Argumenten zurück.
:func:`python3:min`
    gibt das kleinste Element in einem Iterable oder das kleinste von zwei oder
    mehr Argumenten zurück.
:func:`python3:oct`
    konvertiert eine Integer-Zahl in eine oktale Zeichenkette mit dem Präfix
    ``0o``. Das Ergebnis ist ein gültiger Python-Ausdruck. Wenn ``x`` kein
    Python :func:`int`-Objekt ist, muss es eine ``__index__()``-Methode
    definieren, die eine ganze Zahl zurückgibt.
:func:`python3:pow`
    gibt *base* als Potenz von *exp* zurück.
:func:`python3:round`
    gibt eine Zahl zurück, die auf *ndigits* nach dem Dezimalpunkt gerundet ist.
    Wird *ndigits* weggelassen oder ist *None*, wird die nächstgelegene Ganzzahl
    zur Eingabe zurückgegeben.

Boolsche Werte
--------------

In den folgenden Beispielen werden Boolesche Werte verwendet:

.. code-block:: python

    >>> x = False
    >>> x
    False
    >>> not x
    True

.. code-block:: python

    >>> y = True * 2
    >>> y
    2

Abgesehen von ihrer Darstellung als ``True`` und ``False`` verhalten sich
Boolesche Werte wie die Zahlen ``1`` (``True``) und ``0`` (``False``).

Erweiterte numerische Funktionen
--------------------------------

Fortgeschrittenere numerische Funktionen wie Trigonometrie sowie einige
nützliche Konstanten sind nicht in Python integriert, sondern werden in einem
Standardmodul namens :doc:`math <python3:library/math>` bereitgestellt.
:doc:`Module </modules/index>` werden später noch ausführlicher erklärt. Für den
Moment genügt, dass ihr die mathematischen Funktionen in diesem Abschnitt
verfügbar machen müsst, indem ihr ``math`` importiert:

.. code-block:: python

    import math

Eingebaute Funktionen sind immer verfügbar und werden mit einer Standard-Syntax
für Funktionsaufrufe aufgerufen. Im folgenden Code wird ``round`` mit einem
Float als Eingangsargument aufgerufen.

.. code-block:: python

    >>> round(2.5)
    2

Mit ``ceil`` aus der Standardbibliothek ``math`` und der Attributschreibweise
:samp:`MODUL.FUNKTION(ARGUMENT)` wird aufgerundet:

.. code-block:: python

    >>> math.ceil(2.5)
    3

Das ``math``-Modul bietet :abbr:`u.a. (unter anderem)`

* die zahlentheoretischen und Darstellungsfunktionen :func:`python3:math.ceil`,
  :func:`python3:math.modf`, :func:`python3:math.frexp` und
  :func:`python3:math.ldexp`,
* die Potenz- und logarithmische Funktionen :func:`python3:math.exp`,
  :func:`python3:math.log`, :func:`python3:math.log10`, :func:`python3:math.pow`
  und :func:`python3:math.sqrt`,
* die trigonometrischen Funktionen :func:`python3:math.acos`,
  :func:`python3:math.asin`, :func:`python3:math.atan`,
  :func:`python3:math.atan2`, :func:`python3:math.ceil`,
  :func:`python3:math.cos`, :func:`python3:math.hypot` und
  :func:`python3:math.sin`,
* die hyperbolischen Funktionen :func:`python3:math.cosh`,
  :func:`python3:math.sinh` und :func:`python3:math.tanh`
* und die Konstanten :data:`python3:math.e` und :data:`python3:math.pi`.

Erweiterte Funktionen für komplexe Zahlen
-----------------------------------------

Die Funktionen im Modul :doc:`math <python3:library/math>` sind nicht auf
komplexe Zahlen anwendbar; einer der Gründe hierfür dürfte sein, dass die
Quadratwurzel aus ``-1`` einen Fehler erzeugen soll. Daher wurden ähnliche
Funktionen für komplexe Zahlen arbeiten im
:doc:`cmath <python3:library/cmath>`-Modul bereitgestellt:

:func:`python3:cmath.acos`, :func:`python3:cmath.acosh`, :func:`python3:cmath.asin`, :func:`python3:cmath.asinh`, :func:`python3:cmath.atan`, :func:`python3:cmath.atanh`, :func:`python3:cmath.cos`, :func:`python3:cmath.cosh`, :func:`python3:cmath.e`, :func:`python3:cmath.exp`, :func:`python3:cmath.log`, :func:`python3:cmath.log10`, :func:`python3:cmath.pi`, :func:`python3:cmath.sin`, :func:`python3:cmath.sinh`, :func:`python3:cmath.sqrt`, :func:`python3:cmath.tan`, :func:`python3:cmath.tanh`.

Um im Code deutlich zu machen, dass es sich bei diesen Funktionen um spezielle
Funktionen für komplexe Zahlen handelt, und um Namenskonflikte mit den
normaleren Äquivalenten zu vermeiden, empfiehlt sich der einfache Import des
Moduls um bei der Verwendung der Funktion ausdrücklich auf das ``cmath``-Paket
zu verweisen, :abbr:`z.B. (zum Beispiel)`:

.. code-block:: python

    >>> import cmath
    >>> cmath.sqrt(-2)
    1.4142135623730951j

.. warning::

    Nun wird auch verständlicher, weswegen wir nicht den Import aller Funktionen
    eines Moduls empfehlen mit :samp:`from {MODULE} import \*`. Wenn ihr damit
    zuerst das Modul ``math`` und dann das Modul ``cmath`` importieren würdet,
    hätten die Funktionen in ``cmath`` Vorrang vor denen von ``math``. Außerdem
    ist es beim Verstehen des Codes viel mühsamer, die Quelle der verwendeten
    Funktionen herauszufinden.

Kaufmännisch runden
-------------------

Für das kaufmännische Runden werden :class:`Decimal <python3:decimal.Decimal>` und
:data:`ROUND_HALF_UP <python3:decimal.ROUND_HALF_UP>` aus dem :py:mod:`decimal`-Modul benötigt:

.. code-block:: python

    >>> import decimal
    >>> num = decimal.Decimal('2.5')
    >>> rounded = num.quantize(decimal.Decimal('0'), rounding = decimal.ROUND_HALF_UP)
    >>> rounded
    Decimal('3')

Numerische Berechnungen
-----------------------

Die Python-Standardinstallation eignet sich aufgrund von
Geschwindigkeitseinschränkungen nicht gut für intensive numerische Berechnungen.
Aber die leistungsstarke Python-Erweiterung
:doc:`jupyter-tutorial:workspace/numpy/index`  bieten hocheffiziente
Implementierungen vieler fortgeschrittener numerischer Operationen. Der Schwerpunkt liegt dabei auf Array-Operationen, einschließlich mehrdimensionaler Matrizen
und fortgeschrittener Funktionen wie der schnellen Fourier-Transformation.

Eingebaute Module für Zahlen
----------------------------

Die Python-Standardbibliothek enthält eine Reihe eingebauter Module, mit denen
ihr Zahlen managen könnt:

.. _number-modules:

+-----------------------+-------------------------------------------------------------------------------+
| Modul                 | Beschreibung                                                                  |
+=======================+===============================================================================+
| :py:mod:`numbers`     | für numerische abstrakte Basisklassen                                         |
+-----------------------+-------------------------------------------------------------------------------+
| :py:mod:`math`,       | für mathematische Funktionen für reelle und komplexe Zahlen                   |
| :py:mod:`cmath`       |                                                                               |
+-----------------------+-------------------------------------------------------------------------------+
| :py:mod:`decimal`     | für dezimale Festkomma- und Gleitkomma-Arithmetik                             |
+-----------------------+-------------------------------------------------------------------------------+
| :py:mod:`statistics`  | für Funktionen zur Berechnung von mathematischen Statistiken                  |
+-----------------------+-------------------------------------------------------------------------------+
| :py:mod:`fractions`   | für rationale Zahlen                                                          |
+-----------------------+-------------------------------------------------------------------------------+
| :py:mod:`random`      | zum Erzeugen von Pseudozufallszahlen und -auswahlen sowie zum Mischen von     |
|                       | Sequenzen                                                                     |
+-----------------------+-------------------------------------------------------------------------------+
| :py:mod:`itertools`   | für Funktionen, die Iteratoren für effiziente Schleifen erzeugen              |
+-----------------------+-------------------------------------------------------------------------------+
| :py:mod:`functools`   | für Funktionen höherer Ordnung und Operationen auf aufrufbaren Objekten       |
+-----------------------+-------------------------------------------------------------------------------+
| :py:mod:`operator`    | für Standardoperatoren als Funktionen                                         |
+-----------------------+-------------------------------------------------------------------------------+
