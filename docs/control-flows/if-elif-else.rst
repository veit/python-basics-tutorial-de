``if``-``elif``-``else``-Anweisung
==================================

Der Codeblock nach der ersten wahren Bedingung einer ``if``- oder
``elif``-Anweisung wird ausgeführt. Wenn keine der Bedingungen wahr ist, wird
der Codeblock nach dem ``else`` ausgeführt:

.. code-block:: python
    :linenos:

    >>> x = 1
    >>> if x < 1:
    ...     x = 2
    ...     y = 3
    ... elif x > 1:
    ...     x = 4
    ...     y = 5
    ... else:
    ...     x = 6
    ...     y = 7
    ...
    >>> print(x, y)
    6 7

Zeilen 5 und 8
    Die ``elif``- und ``else``-Klauseln sind optional, und es kann eine
    beliebige Anzahl von ``elif``-Klauseln geben.
Zeilen 3, 4, 6, 7, 9 und 10
    Python verwendet Einrückungen, um Blöcke abzugrenzen. Es sind keine
    expliziten Begrenzungszeichen wie Klammern oder geschweifte Klammern
    erforderlich. Jeder Block besteht aus einer oder mehreren Anweisungen, die
    durch Zeilenumbrüche getrennt sind. Alle diese Anweisungen müssen auf der
    gleichen Einrückungsebene stehen.
