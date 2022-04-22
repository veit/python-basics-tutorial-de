Zeichenketten
=============

Die Verarbeitung von Zeichenketten ist eine der Stärken von Python. Es gibt
viele Optionen zur Begrenzung von Zeichenketten:

.. code-block:: python

    "Eine Zeichenfolge in doppelten Anführungszeichen kann 'einfache Anführungszeichen' enthalten."
    'Eine Zeichenfolge in einfachen Anführungszeichen kann "doppelte Anführungszeichen" enthalten.'
    '''\tEine Zeichenkette, die mit einem Tabulator beginnt und mit einem Zeilenumbruchzeichen endet.\n'''
    """Dies ist eine Zeichenkette in dreifach doppelten Anführungszeichen, die
    einzige Zeichenkette, die echte Zeilenumbrüche enthält."""

Zeichenketten können durch einfache (``' '``), doppelte (``" "``), dreifache
einfache (``''' '''``) oder dreifache doppelte (``""" """``) Anführungszeichen
getrennt werden und können Tabulator- (``\t``) und Zeilenumbruchzeichen (``\n``)
enthalten.
    
Zeichenketten sind außerdem unveränderlich. Die Operatoren und Funktionen, die
mit ihnen arbeiten, geben neue, vom Original abgeleitete Zeichenketten zurück.
Die Operatoren (``in``, ``+`` und ``*``) und eingebauten Funktionen (``len``,
``max`` und ``min``) arbeiten mit Zeichenketten genauso wie mit Listen und
Tupeln. Die Index- und Slice-Notation funktioniert auf die gleiche Weise, um
Elemente oder Slices zu erhalten, kann aber nicht verwendet werden, um Elemente
hinzuzufügen, zu entfernen oder zu ersetzen.

Für Zeichenketten gibt es im Modul :doc:`string <python3:library/string>`
mehrere Methoden, um mit ihrem Inhalt zu arbeiten :abbr:`u.a. (unter anderem)`
``split`` und ``replace``:

.. code-block:: python

    >>> x = "\tHello pythonistas!"
    >>> x.split()
    ['Hello', 'pythonistas!']
    >>> x.replace("\tHello", "Hi")
    'Hi pythonistas!'

.. seealso::
   Eine vollständige Übersicht über die ``str``-Methoden findet ihr in der
   :ref:`Python-Dokumentation <python3:string-methods>`.

Das Bibliotheksmodul :doc:`python3:library/re` enthält ebenfalls Funktionen für
die Arbeit mit Zeichenketten:

.. code-block:: python

    >>> import re
    >>> regex = re.compile(r"[\t]+")
    >>> regex.sub("", x)
    'Hello pythonistas!'

Das Modul ``re`` bietet Funktionen für reguläre Ausdrücke. Es bietet
ausgefeiltere Möglichkeiten zur Musterextraktion und -ersetzung als das Modul
``string``.

Die Funktion ``print`` gibt Zeichenketten aus wobei andere Python-Datentypen
leicht in Strings umgewandelt und formatiert werden können, :abbr:`z.B. (zum
Beispiel`:

.. code-block:: python

    >>> import math
    >>> pi = math.pi
    >>> r = 28
    >>> u = 2 * pi * r
    >>> print("Pi ist ", pi, "und der Umfang u ist ", u, "Zoll.")
    Pi ist  3.141592653589793 und der Umfang u ist  175.92918860102841 Zoll.
    >>> print("Der Wert von %s ist %.2f." % ("Pi", pi))
    Der Wert von Pi ist 3.14.

Objekte werden automatisch in Zeichenketten umgewandelt, um sie auszudrucken,
wobei der ``%``-Operator bietet zusätzliche Formatierungsmöglichkeiten.
