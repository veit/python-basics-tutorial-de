Boolesche Werte
===============

Boolesche Werte sind in Python Ganzzahlen:

.. code-block:: pycon

   >>> issubclass(bool, int)
   True

.. include:: ../../oop/types.rst
   :start-after: start-issubclass
   :end-before: end-issubclass

``True`` verhält sich wie eine ``1`` und ``False`` wie eine ``0``. Es gibt
tatsächlich bestimmte Situationen, in denen dies sehr nützlich ist.

Die Funktionen :py:func:`any` und :py:func:`all` prüfen, ob mindestens ein oder
alle Elemente in einer iterierbaren Struktur die jeweilige Bedingung erfüllen:

.. code-block:: pycon

   >>> numbers = [0, 1, -1, 2, 3]
   >>> any(n > 0 for n in numbers)
   True
   >>> any(n < 0 for n in numbers)
   True
   >>> all(n > 0 for n in numbers)
   False
   >>> all(n < 0 for n in numbers)
   False

Aber ``any`` oder ``all`` können euch nicht sagen, wie viele Elemente
übereinstimmen. Um die Anzahl der Übereinstimmungen zu zählen, könnt ihr
stattdessen :py:func:`sum` verwenden:

.. code-block:: pycon

   >>> sum(n > 0 for n in numbers)
   3

Dabei werden durch die Summierung von ``True`` und ``False`` die Anzahl der
``True``-Werte gezählt.

Checks
------

* Entscheidet, ob die folgenden Aussagen wahr oder falsch sind:

  * ``1``
  * ``0``
  * ``-1``
  * ``[0]``
  * ``[]``
  * ``1 and 0``
  * ``1 > 0 or []``
