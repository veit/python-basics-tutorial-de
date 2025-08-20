:orphan:

Reguläre Ausdrücke
==================

.. seealso::
   * `www.regular-expressions.info <https://www.regular-expressions.info/>`_
   * `regex101 <https://regex101.com>`_
   * `AutoRegex <https://www.autoregex.xyz>`_

``[]``
------

Eckige Klammern definieren eine Liste oder einen Bereich von zu suchenden
Zeichen:

``[abc]``
    entspricht a, b oder c

``[a-z]``
    entspricht jedem Kleinbuchstaben
``[A-Za-z]``
    entspricht jedem Buchstaben
``[A-Za-z0-9]``
    entspricht einem beliebigen Buchstaben oder einer beliebigen Ziffer

Anzahl
------

``.``
    entspricht einem einzelnen Zeichen
``*``
    entspricht null oder mehr Mal dem vorhergehenden Element, :abbr:`z.B. (zum
    Beispiele)` ``colou*r``
    passt zu ``color``, ``colour``, ``colouur``, :abbr:`usw (und so weiter)`.
``?``
    entspricht null oder einmal dem vorhergehenden Element. ``colou?r`` passt
    zu ``color`` und ``colour``
``+``
    entspricht ein- oder mehr Mal dem vorhergehenden Element, :abbr:`z.B. (zum
    Beispiel)` ``.+`` passt
    zu ``.,`` ``..``, ``...`` :abbr:`usw (und so weiter)`.
``{N}``
    entspricht ``N`` Mal dem vorhergehenden Element.
``{N,}``
    entspricht ``N`` oder mehr Mal dem vorhergehenden Element.
``{N,M}``
    entspricht mindestens ``N`` mal dem vorhergehenden Element, aber nicht mehr
    als ``M`` mal.

Position
--------

``^``
    setzt die Position an den Anfang der Zeile.
``$``
    setzt die Position an das Ende der Zeile.

Verknüpfung
-----------

``|``
    logisches *oder*.

Escape-Zeichen und Literale
---------------------------

``\``
    wird verwendet, um nach einem Sonderzeichen zu suchen,
    :abbr:`z.B.(zum Beispiel)` um ``.org`` zu finden, müsst ihr den regulären
    Ausdruck ``\.org`` verwenden, da ``.`` das Sonderzeichen ist, das auf jedes
    Zeichen passt.
``[\b]``
    Backspace-Zeichen
``\d``
    passt zu jeder einzelnen Ziffer.
``\D``
    passt zu jedem Zeichen außer Ziffern.
``\w``
    passt auf jeden Teil eines Wortzeichens und ist äquivalent zu
    ``[A-Za-z0-9]``.
``\W``
    passt auf jeden Teil eines Nicht-Wort-Zeichens.
``\s``
    passt zu jedem Leerzeichen, Tabulator oder Zeilenumbruch.
``\S``
    passt zu jedem Zeichen außer Leerzeichen, Tabulator oder Zeilenumbruch.
``\A``
    Anfang einer Zeichenkette
``\Z``
    Ende einer Zeichenkette
``\b``
    passt zu einem Muster an einer Wortgrenze.
``\B``
    Nicht-Wort-Grenze
