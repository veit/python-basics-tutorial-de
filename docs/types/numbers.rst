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

Die folgenden Beispiele verwenden komplexe Zahlen:

.. code-block:: python

    >>> (5+3j) ** (3+5j)
    (-7.04464115622119-11.276062812695923j)

.. code-block:: python

    >>> x = (5+3j) * (6+8j)
    >>> x
    (6+58j)
    >>> x.real
    6.0
    >>> x.imag
    58.0

Komplexe Zahlen bestehen aus einem Realteil und einem Imaginärteil mit dem
Suffix ``j``. Im vorangehenden Code ist die Variable ``x`` einer komplexen Zahl
zugeordnet. Ihr könnt ihren „realen“ Teil mit der Attributschreibweise
``x.real`` und den „imaginären“ Teil mit ``x.imag`` erhalten.

Built-in numerische Funktionen
------------------------------

Mehrere eingebaute Funktionen können mit Zahlen arbeiten:

:func:`python3:abs`
    gibt den absoluten Wert einer Zahl zurück. Dabei kann as Argument kann eine
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

Erweiterte numerische Funktionen
--------------------------------

Fortgeschrittenere numerische Funktionen wie Trigonometrie sowie einige
nützliche Konstanten sind nicht in Python integriert, sondern werden in einem
Standardmodul namens :doc:`math <python3:library/math>` bereitgestellt.
:doc:`Module </modules/index>` werden später noch ausführlicher erklärt. Für den
Moment genügt, dass ihr die mathematischen Funktionen in diesem Abschnitt
verfügbar machen müsst, indem ihr alles von ``math`` importiert:

.. code-block:: python

    from math import *

Eingebaute Funktionen sind immer verfügbar und werden mit einer Standard-Syntax
für Funktionsaufrufe aufgerufen. Im folgenden Code wird ``round`` mit einem
Float als Eingangsargument aufgerufen.

.. code-block:: python

    >>> round(1.49)
    1

Mit ``ceil`` aus der Standardbibliothek ``math`` und der Attributschreibweise
:samp:`MODUL.FUNKTION(ARGUMENT)` wird aufgerundet:

.. code-block:: python

    >>> import math
    >>> math.ceil(1.49)
    2

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

Außerdem gibt es das Bibliotheksmodul :doc:`cmath <python3:library/cmath>` (das
Funktionen für komplexe Zahlen enthält).

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
