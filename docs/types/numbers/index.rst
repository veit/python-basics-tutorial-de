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

.. [#] Die Division ganzer Zahlen mit ``/`` führt zu einer Gleitkommazahl, und
       die Division ganzer Zahlen mit ``//`` führt zu einer Ganzzahl, die
       abgeschnitten wird.

.. note::
   Ganzzahlen können unbegrenzt groß sein, begrenzt nur durch den verfügbaren
   Speicher.

Beispiele:

.. code-block:: pycon

    >>> 8 + 3 - 5 * 3
    -4
    >>> 8 / 3
    2.6666666666666665
    >>> 8 // 3
    2
    >>> x = 4.2**3.4
    >>> x
    131.53689544409096
    >>> 9e7 * -5e-3
    -450000.0
    >>> -(5e-3**3)
    -1.2500000000000002e-07

.. seealso::
   * Julia Evans: `Examples of floating point problems
     <https://jvns.ca/blog/2023/01/13/examples-of-floating-point-problems/>`_
   * David Goldberg: `What Every Computer Scientist Should Know About
     Floating-Point Arithmetic
     <https://docs.oracle.com/cd/E19957-01/806-3568/ncg_goldberg.html>`_

.. toctree::
   :titlesonly:
   :hidden:

   complex
   bool

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

Erweiterte numerische Funktionen
--------------------------------

Fortgeschrittenere numerische Funktionen wie Trigonometrie sowie einige
nützliche Variablen sind nicht in Python integriert, sondern werden in einem
Standardmodul namens :doc:`math <python3:library/math>` bereitgestellt.
:doc:`Module </modules/index>` werden später noch ausführlicher erklärt. Für den
Moment genügt, dass ihr die mathematischen Funktionen in diesem Abschnitt
verfügbar machen müsst, indem ihr ``math`` importiert:

.. code-block:: python

    import math

Eingebaute Funktionen sind immer verfügbar und werden mit einer Standard-Syntax
für Funktionsaufrufe aufgerufen. Im folgenden Code wird ``round`` mit einem
Float als Eingangsargument aufgerufen.

.. code-block:: pycon

    >>> round(2.5)
    2

Mit ``ceil`` aus der Standardbibliothek ``math`` und der Attributschreibweise
:samp:`MODUL.FUNKTION(ARGUMENT)` wird aufgerundet:

.. code-block:: pycon

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
* und die Variablen :data:`python3:math.e` und :data:`python3:math.pi`.

Kaufmännisch runden
-------------------

Üblicherweise rechnet Python Gleitkommazahlen der `IEEE 754
<https://de.wikipedia.org/wiki/IEEE_754>`_-Norm entsprechend, wobei Zahlen in
der Mitte in der Hälfte der Fälle abgerundet werden und in der anderen Hälfte
aufgerundet werden um eine statistische Drift bei längeren Rechnungen zu
vermeiden. Für das `kaufmännische Runden werden
<https://de.wikipedia.org/wiki/Rundung#Kaufmännisches_Runden>`_ daher
:class:`Decimal <python3:decimal.Decimal>` und :data:`ROUND_HALF_UP
<python3:decimal.ROUND_HALF_UP>` aus dem :py:mod:`decimal`-Modul benötigt:

.. code-block:: pycon

    >>> import decimal
    >>> num = decimal.Decimal("2.5")
    >>> rounded = num.quantize(decimal.Decimal("0"), rounding=decimal.ROUND_HALF_UP)
    >>> rounded
    Decimal('3')

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

.. _end-number-modules:

.. seealso::
   * :doc:`Python4DataScience:workspace/numpy/index`

Checks
------

* Erstellt einige Zahlenvariablen (Ganzzahlen, Gleitkommazahlen und komplexe
  Zahlen). Experimentiert ein wenig damit, was passiert, wenn ihr Operationen
  mit ihnen durchführt, auch Typ-übergreifend.
