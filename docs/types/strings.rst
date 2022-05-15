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

    >>> welcome = "Hello pythonistas!\n"
    >>> welcome.strip()
    'Hello pythonistas!'
    >>> welcome.split(' ')
    ['Hello', 'pythonistas!\n']
    >>> chunks = [x.strip() for x in welcome.split(' ')]
    >>> chunks
    ['Hello', 'pythonistas!']
    >>> ' '.join(chunks)
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

``re``
------

Die Python-Standard-Bibliothek :doc:`re <python3:library/re>` enthält ebenfalls
Funktionen für die Arbeit mit Zeichenketten. Dabei bietet ``re`` ausgefeiltere
Möglichkeiten zur Musterextraktion und -ersetzung als ``string``.

.. code-block:: python

    >>> import re
    >>> re.sub('\n', '', x)
    'Hello pythonistas!'

Hier wird der reguläre Ausdruck zunächst kompiliert und dann seine
:py:meth:`re.Pattern.sub`-Methode für den übergebenen Text aufgerufen. Ihr könnt den
Auddruck selbst mit :py:func:`re.compile` kompilieren und so ein wiederverwendbares
``regex``-Objekt bilden, das auf unterschiedliche Zeichenketten angewendet die
CPU-Zyklen verringert:

.. code-block:: python

    >>> regex = re.compile('\n')
    >>> regex.sub('', x)
    'Hello pythonistas!'

Wenn ihr stattdessen eine Liste aller Muster erhalten möchtet, die dem
``regex``-Objekt entsprechen, könnt ihr die :py:meth:`re.Pattern.findall`-Methode
verwenden:

.. code-block:: python

    >>> regex.findall(x)
    ['\n']

.. note::
   Um das umständliche Escaping mit ``\`` in einem regulären Ausdruck zu vermeiden,
   könnt ihr rohe String-Literale wie ``r'C:\PATH\TO\FILE'`` anstelle des
   entsprechenden ``'C:\\PATH\\TO\\FILE'`` verwenden.

:py:meth:`re.Pattern.match` und :py:meth:`re.Pattern.search` sind eng mit
:py:meth:`re.Pattern.findall` verwandt. Während ``findall`` alle Übereinstimmungen in
einer Zeichenkette zurückgibt, gibt ``search`` nur die erste Übereinstimmung und
``match`` nur Übereinstimmungen am Anfang der Zeichenkette zurück. Als weniger
triviales Beispiel betrachten wir einen Textblock und einen regulären Ausdruck, der
die meisten E-Mail-Adressen identifizieren kann:

.. code-block:: python

    >>> addresses = """Veit <veit@cusy.io>
    ... Veit Schiele <veit.schiele@cusy.io>
    ... cusy GmbH <info@cusy.io>
    ... """
    >>> pattern = r'[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,4}'
    >>> regex = re.compile(pattern, flags=re.IGNORECASE)
    >>> regex.findall(addresses)
    ['veit@cusy.io', 'veit.schiele@cusy.io', 'info@cusy.io']
    >>> regex.search(addresses)
    <re.Match object; span=(6, 18), match='veit@cusy.io'>
    >>> print(regex.match(addresses))
    None

``regex.match`` gibt ``None`` zurück, da das Muster nur dann passt, wenn es am Anfang
der Zeichenkette steht.

Angenommen, ihr möchtet E-Mail-Adressen finden und gleichzeitig jede Adresse in ihre
drei Komponenten aufteilen:

#. Personenname
#. Domänenname
#. Domänensuffix

Dazu setzt ihr zunächst runde Klammern ``()`` um die zu segmentierenden Teile
des Musters:

.. code-block:: python

    >>> pattern = r'([A-Z0-9._%+-]+)@([A-Z0-9.-]+)\.([A-Z]{2,4})'
    >>> regex = re.compile(pattern, flags=re.IGNORECASE)
    >>> match = regex.match('veit@cusy.io')
    >>> match.groups()
    ('veit', 'cusy', 'io')

:py:meth:`re.Match.groups` gibt ein :doc:`tuples` zurück, das alle Untergruppen der
Übereinstimmung enthält. 

:py:meth:`re.Pattern.findall` gibt eine Liste von Tupeln zurück, wenn das Muster Gruppen
enthält:

.. code-block:: python

    >>> regex.findall(addresses)
    [('veit', 'cusy', 'io'), ('veit.schiele', 'cusy', 'io'), ('info', 'cusy', 'io')]

Auch in :py:meth:`re.Pattern.sub` können Gruppen verwendet werden wobei ``\1`` für die erste
übereinstimmende Gruppe steht, ``\2`` für die zweite :abbr:`usw. (und so weiter)`:

.. code-block:: python

    >>> regex.findall(addresses)
    [('veit', 'cusy', 'io'), ('veit.schiele', 'cusy', 'io'), ('info', 'cusy', 'io')]
    >>> print(regex.sub(r'Username: \1, Domain: \2, Suffix: \3', addresses))
    Veit <Username: veit, Domain: cusy, Suffix: io>
    Veit Schiele <Username: veit.schiele, Domain: cusy, Suffix: io>
    cusy GmbH <Username: info, Domain: cusy, Suffix: io>

Die folgende Tabelle enthält einen kurzen Überblick über Methoden für reguläre Ausdrücke:

+---------------+-------------------------------------------------------------------------------+
| Methode       | Beschreibung                                                                  |
+===============+===============================================================================+
| ``findall``   | gibt alle sich nicht überschneidenden übereinstimmenden Muster in einer       |
|               | Zeichenkette als Liste zurück.                                                |
+---------------+-------------------------------------------------------------------------------+
| ``finditer``  | wie ``findall``, gibt aber einen Iterator zurück.                             |
+---------------+-------------------------------------------------------------------------------+
| ``match``     | entspricht dem Muster am Anfang der Zeichenkette und segmentiert optional die |
|               | Musterkomponenten in Gruppen; wenn das Muster übereinstimmt, wird ein         |
|               | ``match``-Objekt zurückgegeben, andernfalls keines.                           |
+---------------+-------------------------------------------------------------------------------+
| ``search``    | durchsucht die Zeichenkette nach Übereinstimmungen mit dem Muster; gibt in    |
|               | diesem Fall ein ``match``-Objekt zurück; im Gegensatz zu ``match`` kann die   |
|               | Übereinstimmung an einer beliebigen Stelle der Zeichenkette und nicht nur am  |
|               | Anfang stehen.                                                                |
+---------------+-------------------------------------------------------------------------------+
| ``split``     | zerlegt die Zeichenkette bei jedem Auftreten des Musters in Teile.            |
+---------------+-------------------------------------------------------------------------------+
| ``sub``,      | ersetzt alle (``sub``) oder die ersten ``n`` Vorkommen (``subn``) des Musters |
| ``subn``      | in der Zeichenkette durch einen Ersetzungsausdruck; verwendet die Symbole     |
|               | ``\1``, ``\2``, …, um auf die Elemente der Übereinstimmungsgruppe in der      |
|               | zu verweisen.                                                                 |
+---------------+-------------------------------------------------------------------------------+

.. seealso::
   * :doc:`../appendix/regex`

``print()``
-----------

Die Funktion :func:`print` gibt Zeichenketten aus wobei andere Python-Datentypen
leicht in Strings umgewandelt und formatiert werden können, :abbr:`z.B. (zum
Beispiel)`:

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
