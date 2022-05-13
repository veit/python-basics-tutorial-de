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

``string``
----------

Für Zeichenketten gibt es in der Standard-Python-Bibliothek :doc:`string
<python3:library/string>` mehrere Methoden, um mit ihrem Inhalt zu arbeiten
:abbr:`u.a. (unter anderem)` :py:meth:`str.split`, :py:meth:`str.replace` und
:py:meth:`str.strip`:

.. code-block:: python

    >>> x = "Hello pythonistas!\n"
    >>> x.split()
    ['Hello', 'pythonistas!']
    >>> x.replace('Hello', 'Hi')
    'Hi pythonistas!\n'
    >>> x.strip()
    'Hello pythonistas!'

Im folgenden ein Überblick über alle ``string``-Methoden:

+---------------+---------------------------------------------------------------+
| Methode       | Beschreibung                                                  |
+===============+===============================================================+
| ``count``     | gibt die Anzahl der sich nicht überschneidenden Vorkommen der |
|               | Zeichenkette zurück.                                          |
+---------------+---------------------------------------------------------------+
| ``endswith``  | gibt ``True`` zurück, wenn die Zeichenkette mit dem Suffix    |
|               | endet.                                                        |
+---------------+---------------------------------------------------------------+
| ``startswith``| gibt ``True`` zurück, wenn die Zeichenkette mit dem Präfix    |
|               | beginnt.                                                      |
+---------------+---------------------------------------------------------------+
| ``join``      | verwendet die Zeichenkette als Begrenzer für die Verkettung   |
|               | einer Folge anderer Zeichenketten.                            |
+---------------+---------------------------------------------------------------+
| ``index``     | gibt die Position des ersten Zeichens in der Zeichenkette     |
|               | zurück, wenn es in der Zeichenkette gefunden wurde; löst einen|
|               | ``ValueError`` aus, wenn es nicht gefunden wurde.             |
+---------------+---------------------------------------------------------------+
| ``find``      | gibt die Position des ersten Zeichens des ersten Vorkommens   |
|               | der Teilzeichenkette in der Zeichenkette zurück; wie          |
|               | ``index``, gibt aber ``-1`` zurück, wenn nichts gefunden      |
|               | wurde.                                                        |
+---------------+---------------------------------------------------------------+
| ``rfind``     | Rückgabe der Position des ersten Zeichens des letzten         |
|               | Vorkommens der Teilzeichenkette in der Zeichenkette; gibt     |
|               | ``-1`` zurück, wenn nichts gefunden wurde.                    |
+---------------+---------------------------------------------------------------+
| ``replace``   | ersetzt Vorkommen einer Zeichenkette durch eine andere        |
|               | Zeichenkette.                                                 |
+---------------+---------------------------------------------------------------+
| ``strip``,    | schneiden Leerzeichen ab, einschließlich Zeilenumbrüchen.     |
| ``rstrip``,   |                                                               |
| ``lstrip``    |                                                               |
+---------------+---------------------------------------------------------------+
| ``split``     | zerlegt eine Zeichenkette in eine Liste von Teilzeichenketten |
|               | unter Verwendung des übergebenen Trennzeichens.               |
+---------------+---------------------------------------------------------------+
| ``lower``     | konvertiert alphabetische Zeichen in Kleinbuchstaben.         |
+---------------+---------------------------------------------------------------+
| ``upper``     | konvertiert alphabetische Zeichen in Großbuchstaben.          |
+---------------+---------------------------------------------------------------+
| ``casefold``  | konvertiert Zeichen in Kleinbuchstaben und konvertiert alle   |
|               | regionsspezifischen variablen Zeichenkombinationen in eine    |
|               | gemeinsame vergleichbare Form.                                |
+---------------+---------------------------------------------------------------+
| ``ljust``,    | linksbündig bzw. rechtsbündig; füllt die gegenüberliegende    |
| ``rjust``     | Seite der Zeichenkette mit Leerzeichen (oder einem anderen    |
|               | Füllzeichen) auf, um eine Zeichenkette mit einer Mindestbreite|
|               | zu erhalten.                                                  |
+---------------+---------------------------------------------------------------+

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
    >>> d = 28
    >>> u = pi * d
    >>> print("Pi ist", pi, "und der Umfang bei einem Durchmesser von", d, "Zoll ist", u, "Zoll.")
    Pi ist 3.141592653589793 und der Umfang bei einem Durchmesser von 28 Zoll ist 87.96459430051421 Zoll.
    >>> print(f"Der Wert von Pi ist {pi:.3f}.")
    Der Wert von Pi ist 3.142.

Objekte werden automatisch in Zeichenketten umgewandelt, um sie auszudrucken,
wobei die mit vorangestelltem ``f`` formatierten String-Literale zusätzliche
Formatierungsmöglichkeiten bieten.

.. seealso::
   * :ref:`python3:f-strings`
   * `PEP 498 – Literal String Interpolation
     <https://peps.python.org/pep-0498/>`_
