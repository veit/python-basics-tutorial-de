Zeichenketten
=============

Die Verarbeitung von Zeichenketten ist eine der Stärken von Python. Es gibt
viele Optionen zur Begrenzung von Zeichenketten:

.. code-block:: python

   "Eine Zeichenfolge in doppelten Anführungszeichen kann 'einfache Anführungszeichen' enthalten."
   'Eine Zeichenfolge in einfachen Anführungszeichen kann "doppelte Anführungszeichen" enthalten.'
   """\tEine Zeichenkette, die mit einem Tabulator beginnt und mit einem Zeilenumbruchzeichen endet.\n"""
   """Dies ist eine Zeichenkette in dreifach doppelten Anführungszeichen, die
   einzige Zeichenkette, die echte Zeilenumbrüche enthält."""

Zeichenketten können durch einfache (``' '``), doppelte (``" "``), dreifache
einfache (``''' '''``) oder dreifache doppelte (``""" """``) Anführungszeichen
getrennt werden.

Eine normale Zeichenkette kann nicht auf mehrere Zeilen aufgeteilt werden. Der
folgende Code wird also nicht funktionieren:

.. code-block::

   "Dies ist ein fehlerhafter Versuch, einen einen Zeilenumbruch in
   eine Zeichenkette einzufügen, ohne \n zu verwenden."

Sie können auch Tabulator- (``\t``) und *Newline*-Zeichen (``\n``) enthalten.
Allgemein können Backslashes ``\`` als Escape-Zeichen verwendet werden. So kann
:abbr:`z.B. (zum Beispiel)` ``\\`` für einen einzelnen Backslash und ``\'`` für
ein einfaches Anführungszeichen verwendet werden, wodurch es die
Zeichenfolge nicht beendet:

.. blacken-docs:off

.. code-block:: python

   "You don't need a backslash here."
   'However, this wouldn\'t work without a backslash.'

.. blacken-docs:on

Python bietet jedoch auch Zeichenketten in dreifachen Anführungszeichen
(``"""``), die dies ermöglichen und einfache und doppelte Anführungszeichen ohne
Backslashes ``\`` als Escape-Zeichen enthalten können.

Sonderzeichen und Escape-Sequenzen
----------------------------------

``\n`` steht für das *Newline*-Zeichen und ``\t`` für das Tabulator-Zeichen.
Zeichenfolgen, die mit einem Backslash beginnen und zur Darstellung anderer
Zeichen verwendet werden, werden Escape-Sequenzen genannt. Escape-Sequenzen
werden in der Regel verwendet, um Sonderzeichen darzustellen, :abbr:`d.h. (das
heißt)` Zeichen, für die es keine einstellige druckbare Darstellung gibt.

Hier sind weitere Zeichen, die ihr mit dem Escape-Zeichen erhalten könnt:

+--------------------------+--------------------------+--------------------------+
| Escape-Sequenz           | Ausgabe                  | Erläuterung              |
+==========================+==========================+==========================+
| ``\\``                   | ``\``                    | Backslash                |
+--------------------------+--------------------------+--------------------------+
| ``\'``                   | ``'``                    | einfaches                |
|                          |                          | Anführungszeichen        |
+--------------------------+--------------------------+--------------------------+
| ``\"``                   | ``"``                    | doppeltes                |
|                          |                          | Anführungszeichen        |
+--------------------------+--------------------------+--------------------------+
| ``\b``                   |                          | Backspace (``BS``)       |
+--------------------------+--------------------------+--------------------------+
| ``\n``                   |                          | ASCII Linefeed ``(LF``)  |
+--------------------------+--------------------------+--------------------------+
| ``\r``                   |                          | ASCII Carriage Return    |
|                          |                          | (``CR``)                 |
+--------------------------+--------------------------+--------------------------+
| ``\t``                   |                          | Tabulator (``TAB``)      |
+--------------------------+--------------------------+--------------------------+
| :samp:`\u{00B5}`         | ``µ``                    | Unicode 16 bit           |
+--------------------------+--------------------------+--------------------------+
| :samp:`\U{000000B5}`     | ``µ``                    | Unicode 32 bit           |
+--------------------------+--------------------------+--------------------------+
| :samp:`\N{{SNAKE}}`      | ``🐍``                   | Unicode Emoji name       |
+--------------------------+--------------------------+--------------------------+

Zeilen 1–7
    Der ASCII-Zeichensatz, der von Python verwendet wird und der
    Standard-Zeichensatz auf fast allen Computern ist, definiert eine ganze
    Reihe weiterer Sonderzeichen.
Zeilen 8–9
    Unicode-Escape-Sequenzen.
Zeile 10
    Unicode-Namen zur Angabe eines Unicode-Zeichens.

Operatoren und Funktionen
-------------------------

Die Operatoren und Funktionen, die mit Zeichenketten arbeiten, geben neue, vom
Original abgeleitete Zeichenketten zurück. Die Operatoren (``in``, ``+`` und
``*``) und eingebauten Funktionen (``len``, ``max`` und ``min``) arbeiten mit
Zeichenketten genauso wie mit Listen und Tupeln.

.. code-block:: pycon

   >>> welcome = "Hello pythonistas!\n"
   >>> 2 * welcome
   'Hello pythonistas!\nHello pythonistas!\n'
   >>> welcome + welcome
   'Hello pythonistas!\nHello pythonistas!\n'
   >>> "python" in welcome
   True
   >>> max(welcome)
   'y'
   >>> min(welcome)
   '\n'

Indizierung und Slicing
-----------------------

Die Index- und Slice-Notation funktioniert auf die gleiche Weise, um einzelne
Elemente oder Slices zu erhalten:

.. code-block:: pycon

   >>> welcome[0:5]
   'Hello'
   >>> welcome[6:-1]
   'pythonistas!'

Die Index- und Slice-Notation kann jedoch nicht verwendet werden, um Elemente
hinzuzufügen, zu entfernen oder zu ersetzen, da Zeichenketten unveränderlich
sind:

.. code-block:: pycon

   >>> welcome[6:-1] = "everybody!"
   Traceback (most recent call last):
     File "<stdin>", line 1, in <module>
   TypeError: 'str' object does not support item assignment

String-Methoden
---------------

Die meisten der Python-:ref:`String-Methoden <python3:string-methods>` sind im
:ref:`str <python3:textseq>`-Typ integriert, so dass alle ``str``-Objekte
automatisch über sie verfügen:

.. code-block:: pycon

   >>> welcome = "hello pythonistas!\n"
   >>> welcome.isupper()
   False
   >>> welcome.isalpha()
   False
   >>> welcome[0:5].isalpha()
   True
   >>> welcome.capitalize()
   'Hello pythonistas!\n'
   >>> welcome.title()
   'Hello Pythonistas!\n'
   >>> welcome.strip()
   'Hello pythonistas!'
   >>> welcome.split(" ")
   ['hello', 'pythonistas!\n']
   >>> chunks = [snippet.strip() for snippet in welcome.split(" ")]
   >>> chunks
   ['hello', 'pythonistas!']
   >>> " ".join(chunks)
   'hello pythonistas!'
   >>> welcome.replace("\n", "")
   'hello pythonistas!'

Im Folgenden findet ihr einen Überblick über die häufigsten
:ref:`String-Methoden <python3:string-methods>`:

+---------------------------+---------------------------------------------------------------+
| Methode                   | Beschreibung                                                  |
+===========================+===============================================================+
| :py:meth:`str.count`      | gibt die Anzahl der sich nicht überschneidenden Vorkommen der |
|                           | Zeichenkette zurück.                                          |
+---------------------------+---------------------------------------------------------------+
| :py:meth:`str.endswith`   | gibt ``True`` zurück, wenn die Zeichenkette mit dem Suffix    |
|                           | endet.                                                        |
+---------------------------+---------------------------------------------------------------+
| :py:meth:`str.startswith` | gibt ``True`` zurück, wenn die Zeichenkette mit dem Präfix    |
|                           | beginnt.                                                      |
+---------------------------+---------------------------------------------------------------+
| :py:meth:`str.join`       | verwendet die Zeichenkette als Begrenzer für die Verkettung   |
|                           | einer Folge anderer Zeichenketten.                            |
+---------------------------+---------------------------------------------------------------+
| :py:meth:`str.index`      | gibt die Position des ersten Zeichens in der Zeichenkette     |
|                           | zurück, wenn es in der Zeichenkette gefunden wurde; löst einen|
|                           | ``ValueError`` aus, wenn es nicht gefunden wurde.             |
+---------------------------+---------------------------------------------------------------+
| :py:meth:`str.find`       | gibt die Position des ersten Zeichens des ersten Vorkommens   |
|                           | der Teil-Zeichenkette in der Zeichenkette zurück; wie         |
|                           | ``index``, gibt aber ``-1`` zurück, wenn nichts gefunden      |
|                           | wurde.                                                        |
+---------------------------+---------------------------------------------------------------+
| :py:meth:`str.rfind`      | Rückgabe der Position des ersten Zeichens des letzten         |
|                           | Vorkommens der Teil-Zeichenkette in der Zeichenkette; gibt    |
|                           | ``-1`` zurück, wenn nichts gefunden wurde.                    |
+---------------------------+---------------------------------------------------------------+
| :py:meth:`str.replace`    | ersetzt Vorkommen einer Zeichenkette durch eine andere        |
|                           | Zeichenkette.                                                 |
+---------------------------+---------------------------------------------------------------+
| :py:meth:`str.strip`,     | schneiden Leerzeichen ab, einschließlich Zeilenumbrüchen.     |
| :py:meth:`str.rstrip`,    |                                                               |
| :py:meth:`str.lstrip`     |                                                               |
+---------------------------+---------------------------------------------------------------+
| :py:meth:`str.split`      | zerlegt eine Zeichenkette in eine Liste von Teil-Zeichenketten|
|                           | unter Verwendung des übergebenen Trennzeichens.               |
+---------------------------+---------------------------------------------------------------+
| :py:meth:`str.lower`      | konvertiert alphabetische Zeichen in Kleinbuchstaben.         |
+---------------------------+---------------------------------------------------------------+
| :py:meth:`str.upper`      | konvertiert alphabetische Zeichen in Großbuchstaben.          |
+---------------------------+---------------------------------------------------------------+
| :py:meth:`str.casefold`   | konvertiert Zeichen in Kleinbuchstaben und konvertiert alle   |
|                           | regionsspezifischen variablen Zeichenkombinationen in eine    |
|                           | gemeinsame vergleichbare Form.                                |
+---------------------------+---------------------------------------------------------------+
| :py:meth:`str.ljust`,     | linksbündig bzw. rechtsbündig; füllt die gegenüberliegende    |
| :py:meth:`str.rjust`      | Seite der Zeichenkette mit Leerzeichen (oder einem anderen    |
|                           | Füllzeichen) auf, um eine Zeichenkette mit einer Mindestbreite|
|                           | zu erhalten.                                                  |
+---------------------------+---------------------------------------------------------------+

``str.split`` und ``str.join``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Während :meth:`python3:str.split` eine Liste von Zeichenfolgen zurückgibt, nimmt
:meth:`python3:str.join` eine Liste von Zeichenketten und fügt sie zu einer
einzigen Zeichenkette zusammen. Normalerweise verwendet
:meth:`python3:str.split` Leerraum als Begrenzungszeichen für die aufzuteilenden
Zeichenketten, aber ihr könnt dieses Verhalten mit einem optionalen
:doc:`../../functions/params` ändern.

.. warning::
   Die Verkettung von Zeichenketten mit ``+`` ist zwar nützlich, aber nicht
   effizient, wenn es darum geht, eine große Anzahl von Zeichenketten zu einer
   einzigen Zeichenkette zusammenzufügen, da jedes Mal, wenn ``+`` angewendet
   wird, ein neues Zeichenketten-Objekt erstellt wird. :samp:`"Hello" +
   "Pythonistas!"` erzeugt zwei Objekte, von denen eines sofort wieder verworfen
   wird.

Wenn ihr mit :meth:`python3:str.join` Zeichenfolgen zusammenführt, könnt ihr
zwischen die Zeichenfolgen beliebige Zeichen einfügen:

.. code-block:: pycon

   >>> " :: ".join(["License", "OSI Approved"])
   'License :: OSI Approved'

Ihr könnt auch eine leere Zeichenkette, ``""``, verwenden, :abbr:`z.B. (zum
Beispiel)` für die CamelCase-Schreibweise von Python-Klassen:

.. code-block:: pycon

   >>> "".join(["My", "Class"])
   'MyClass'

:meth:`python3:str.split` wird meist verwendet um Zeichenketten an Leerräumen zu
trennen. Ihr könnt eine Zeichenkette jedoch auch an einer bestimmten anderen
Zeichenfolge trennen, indem ihr einen optionalen :doc:`../../functions/params`
übergebt:

.. code-block:: pycon

   >>> example = "1. You can have\n\twhitespaces, newlines\n   and tabs mixed in\n\tthe string."
   >>> example.split()
   ['1.', 'You', 'can', 'have', 'whitespaces,', 'newlines', 'and', 'tabs', 'mixed', 'in', 'the', 'string.']
   >>> license = "License :: OSI Approved"
   >>> license.split(" :: ")
   ['License', 'OSI Approved']

Manchmal ist es nützlich, dem letzten Feld in einer Zeichenkette zu erlauben,
beliebigen Text zu enthalten. Ihr könnt dies tun, indem ihr einen optionalen
zweiten :doc:`../../functions/params` angebt, wie viele Teilungen durchgeführt
werden sollen:

.. code-block:: pycon

   >>> example.split(" ", 1)
   ['1.', 'You can have\n\twhitespaces, newlines\n   and tabs mixed in\n\tthe string.']

Wenn ihr :meth:`python3:str.split` mit dem optionalen zweiten Argument verwenden
wollt, müsst ihr zunächst ein erstes Argument angeben. Um zu erreichen, dass bei
allen Leerzeichen geteilt wird, verwendet :doc:`../none` als erstes Argument:

.. code-block:: pycon

   >>> example.split(None, 8)
   ['1.', 'You', 'can', 'have', 'whitespaces,', 'newlines', 'and', 'tabs', 'mixed in\n\tthe string.']

.. tip::
   Ich verwende :meth:`python3:str.split` und :meth:`python3:str.join`
   ausgiebig, meist für Textdateien, die von anderen Programmen erzeugt wurden.
   Zum Schreiben von
   :doc:`Python4DataScience:data-processing/serialisation-formats/csv/index`-
   oder
   :doc:`Python4DataScience:data-processing/serialisation-formats/json/index`-Dateien
   verwende ich jedoch meist die zugehörigen Python-Bibliotheken.

Leerraum entfernen
~~~~~~~~~~~~~~~~~~

:py:meth:`str.strip` gibt eine neue Zeichenkette zurück, die sich von der
ursprünglichen Zeichenkette nur dadurch unterscheidet, dass alle Leerzeichen am
Anfang oder Ende der Zeichenkette entfernt wurden. :py:meth:`str.lstrip` und
:py:meth:`str.rstrip` arbeiten ähnlich, entfernen jedoch nur die Leerzeichen am
linken :abbr:`bzw. (beziehungsweise)` rechten Ende der ursprünglichen
Zeichenkette:

.. code-block:: pycon

   >>> example = "    whitespaces, newlines \n\tand tabs. \n"
   >>> example.strip()
   'whitespaces, newlines \n\tand tabs.'
   >>> example.lstrip()
   'whitespaces, newlines \n\tand tabs. \n'
   >>> example.rstrip()
   '    whitespaces, newlines \n\tand tabs.'

In diesem Beispiel werden die *Newlines* ``\n`` als Leerzeichen betrachtet. Die
genaue Zuordnung kann sich von Betriebssystem zu Betriebssystem unterscheiden.
Ihr könnt herausfinden, was Python als Leerzeichen betrachtet, indem ihr auf die
Konstante :py:data:`string.whitespace` zugreift. Bei mir wird das folgende
zurückgegeben:

.. code-block:: pycon

   >>> import string
   >>> string.whitespace
   ' \t\n\r\x0b\x0c'

Die im Hexadezimalformat (``\x0b``, ``\x0c``) angegebenen Zeichen stellen die
vertikalen Tabulator- und Vorschubzeichen dar.

.. tip::
   Ändert nicht den Wert dieser Variablen um die Funktionsweise von
   :py:meth:`str.strip` :abbr:`usw. (und so weiter)` zu beeinflussen. Welche
   Zeichen diese Methoden entfernen, könnt ihr Zeichen als zusätzlichen
   :doc:`../../functions/params` übergeben:

   .. code-block:: pycon

      >>> url = "https://www.cusy.io/"
      >>> url.strip("htps:/w.")
      'cusy.io'

Suche in Zeichenketten
~~~~~~~~~~~~~~~~~~~~~~

:ref:`str <python3:textseq>`-Objekte bieten mehrere Methoden für die einfache
Suche nach Zeichenketten: Die vier grundlegenden Methoden für die Suche nach
Zeichenketten sind :py:meth:`str.find`, :py:meth:`str.rfind`,
:py:meth:`str.index` und :py:meth:`str.rindex`. Eine verwandte Methode,
:py:meth:`str.count`, zählt, wie oft eine Zeichenfolge in einer anderen
Zeichenfolge gefunden werden kann.

:py:meth:`str.find` benötigt einen einzigen :doc:`../../functions/params`: die
gesuchte Teil-Zeichenkette; zurückgegeben wird dann die Position des ersten
Vorkommens oder ``-1``, wenn es kein Vorkommen gibt:

.. code-block:: pycon

   >>> hipy = "Hello Pythonistas!\n"
   >>> hipy.find("\n")
   18

:py:meth:`str.find` kann auch ein oder zwei zusätzliche
:doc:`../../functions/params` annehmen:

``start``
    Zahl, der Zeichen am Anfang der zu durchsuchenden Zeichenkette, die
    ignoriert werden soll.
``end``
    Zahl, der Zeichen am Ende der zu durchsuchenden Zeichenkette, die ignoriert
    werden soll.

Im Gegensatz zu :py:meth:`find` beginnt :py:meth:`rfind` die Suche am Ende der
Zeichenkette und gibt daher die Position des letzten Vorkommens zurück.

:py:meth:`index` und :py:meth:`rindex` unterscheiden sich von :py:meth:`find`
und :py:meth:`rfind` dadurch, dass statt dem Rückgabewert ``-1`` eine
:class:`python3:ValueError`-Ausnahme ausgelöst wird.

Ihr könnt zwei weitere :ref:`String-Methoden <python3:string-methods>`
verwenden, um Strings zu suchen: :py:meth:`str.startswith` und
:py:meth:`str.endswith`. Diese Methoden geben ``True``- oder ``False`` als
Ergebnis zurück, je nachdem, ob die Zeichenkette, auf die sie angewendet werden,
mit einer der als :doc:`../../functions/params` angegebenen Zeichenketten
beginnt oder endet:

.. code-block:: pycon

   >>> hipy.endswith("\n")
   True
   >>> hipy.endswith(("\n", "\r"))
   True

Darüber hinaus gibt es einige Methoden, mit denen die Eigenschaft einer
Zeichenkette überprüft werden kann:

+---------------------------+---------------+---------------+---------------+---------------+---------------+
| Methode                   | ``[!#$%…]``   | ``[a-zA-Z]``  | ``[¼½¾]``     | ``[¹²³]``     | ``[0-9]``     |
+===========================+===============+===============+===============+===============+===============+
| :py:meth:`str.isprintable`| ✅            | ✅            | ✅            | ✅            | ✅            |
+---------------------------+---------------+---------------+---------------+---------------+---------------+
| :py:meth:`str.isalnum`    | ❌            | ✅            | ✅            | ✅            | ✅            |
+---------------------------+---------------+---------------+---------------+---------------+---------------+
| :py:meth:`str.isnumeric`  | ❌            | ❌            | ✅            | ✅            | ✅            |
+---------------------------+---------------+---------------+---------------+---------------+---------------+
| :py:meth:`str.isdigit`    | ❌            | ❌            | ❌            | ✅            | ✅            |
+---------------------------+---------------+---------------+---------------+---------------+---------------+
| :py:meth:`str.isdecimal`  | ❌            | ❌            | ❌            | ❌            | ✅            |
+---------------------------+---------------+---------------+---------------+---------------+---------------+

:py:meth:`str.isspace` prüft auf Leerzeichen.

Zeichenketten ändern
~~~~~~~~~~~~~~~~~~~~

:ref:`str <python3:textseq>`-Objekte sind unveränderlich, aber sie verfügen über
mehrere Methoden, die eine modifizierte Version der ursprünglichen Zeichenkette
zurückgeben können.

:py:meth:`str.replace` könnt ihr verwenden, um Vorkommen des ersten
:doc:`../../functions/params` durch den zweiten zu ersetzen, :abbr:`z.B. (zum
Beispiel)`:

.. code-block:: pycon

   >>> hipy.replace("\n", "\n\r")
   'Hello Pythonistas!\n\r'

:py:meth:`str.maketrans` und :py:meth:`str.translate` können zusammen verwendet
werden, um Zeichen in Zeichenketten in andere Zeichen zu übersetzen, :abbr:`z.B.
(zum Beispiel)`:

.. code-block:: pycon
   :linenos:

   >>> hipy = "Hello Pythonistas!\n"
   >>> trans_map = hipy.maketrans(" ", "-", "!\n")
   >>> hipy.translate(trans_map)
   'Hello-Pythonistas'

Zeile 2
    :py:meth:`str.maketrans` wird verwendet, um eine Übersetzungstabelle aus den
    beiden Zeichenketten-Argumenten zu erstellen. Die beiden Argumente müssen
    jeweils die gleiche Anzahl von Zeichen enthalten. Als drittes Argument
    werden Zeichen übergeben, die nicht zurückgegeben werden sollen.
Zeile 3
    Die von :py:meth:`str.maketrans` erzeugte Tabelle wird an
    :py:meth:`str.translate` übergeben.

``re``
------

Die Python-Standard-Bibliothek :doc:`re <python3:library/re>` enthält ebenfalls
Funktionen für die Arbeit mit Zeichenketten. Dabei bietet ``re`` ausgefeiltere
Möglichkeiten zur Musterextraktion und -ersetzung als der
:ref:`str <python3:textseq>`-Typ.

.. code-block:: pycon

   >>> import re
   >>> re.sub("\n", "", welcome)
   'Hello pythonistas!'

Hier wird der reguläre Ausdruck zunächst kompiliert und dann seine
:py:meth:`re.Pattern.sub`-Methode für den übergebenen Text aufgerufen. Ihr könnt
den Ausdruck selbst mit :py:func:`re.compile` kompilieren und so ein
wiederverwendbares ``regex``-Objekt bilden, das auf unterschiedliche
Zeichenketten angewendet die CPU-Zyklen verringert:

.. code-block:: pycon

   >>> regex = re.compile("\n")
   >>> regex.sub("", welcome)
   'Hello pythonistas!'

Wenn ihr stattdessen eine Liste aller Muster erhalten möchtet, die dem
``regex``-Objekt entsprechen, könnt ihr die
:py:meth:`re.Pattern.findall`-Methode verwenden:

.. code-block:: pycon

   >>> regex.findall(welcome)
   ['\n']

.. note::
   Um das umständliche Escaping mit ``\`` in einem regulären Ausdruck zu
   vermeiden, könnt ihr rohe String-Literale wie ``r'C:\PATH\TO\FILE'``
   anstelle des  entsprechenden ``'C:\\PATH\\TO\\FILE'`` verwenden.

:py:meth:`re.Pattern.match` und :py:meth:`re.Pattern.search` sind eng mit
:py:meth:`re.Pattern.findall` verwandt. Während ``findall`` alle
Übereinstimmungen in einer Zeichenkette zurückgibt, gibt ``search`` nur die
erste Übereinstimmung und ``match`` nur Übereinstimmungen am Anfang der
Zeichenkette zurück. Als weniger triviales Beispiel betrachten wir einen
Textblock und einen regulären Ausdruck, der die meisten E-Mail-Adressen
identifizieren kann:

.. code-block:: pycon

   >>> addresses = """Veit <veit@cusy.io>
   ... Veit Schiele <veit.schiele@cusy.io>
   ... cusy GmbH <info@cusy.io>
   ... """
   >>> pattern = r"[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,4}"
   >>> regex = re.compile(pattern, flags=re.IGNORECASE)
   >>> regex.findall(addresses)
   ['veit@cusy.io', 'veit.schiele@cusy.io', 'info@cusy.io']
   >>> regex.search(addresses)
   <re.Match object; span=(6, 18), match='veit@cusy.io'>
   >>> print(regex.match(addresses))
   None

``regex.match`` gibt ``None`` zurück, da das Muster nur dann passt, wenn es am
Anfang der Zeichenkette steht.

Angenommen, ihr möchtet E-Mail-Adressen finden und gleichzeitig jede Adresse in
ihre drei Komponenten aufteilen:

#. Personen-Name
#. Domänen-Name
#. Domänen-Suffix

Dazu setzt ihr zunächst runde Klammern ``()`` um die zu segmentierenden Teile
des Musters:

.. code-block:: pycon

   >>> pattern = r"([A-Z0-9._%+-]+)@([A-Z0-9.-]+)\.([A-Z]{2,4})"
   >>> regex = re.compile(pattern, flags=re.IGNORECASE)
   >>> match = regex.match("veit@cusy.io")
   >>> match.groups()
   ('veit', 'cusy', 'io')

:py:meth:`re.Match.groups` gibt ein :doc:`../tuples` zurück, das alle
Untergruppen der Übereinstimmung enthält.

:py:meth:`re.Pattern.findall` gibt eine Liste von Tupeln zurück, wenn das Muster
Gruppen enthält:

.. code-block:: pycon

   >>> regex.findall(addresses)
   [('veit', 'cusy', 'io'), ('veit.schiele', 'cusy', 'io'), ('info', 'cusy', 'io')]

Auch in :py:meth:`re.Pattern.sub` können Gruppen verwendet werden wobei ``\1``
für die erste übereinstimmende Gruppe steht, ``\2`` für die zweite :abbr:`usw.
(und so weiter)`:

.. code-block:: pycon

   >>> regex.findall(addresses)
   [('veit', 'cusy', 'io'), ('veit.schiele', 'cusy', 'io'), ('info', 'cusy', 'io')]
   >>> print(regex.sub(r"Username: \1, Domain: \2, Suffix: \3", addresses))
   Veit <Username: veit, Domain: cusy, Suffix: io>
   Veit Schiele <Username: veit.schiele, Domain: cusy, Suffix: io>
   cusy GmbH <Username: info, Domain: cusy, Suffix: io>

Die folgende Tabelle enthält einen kurzen Überblick über Methoden für reguläre
Ausdrücke:

+-------------------------------+-------------------------------------------------------------------------------+
| Methode                       | Beschreibung                                                                  |
+===============================+===============================================================================+
| :py:func:`re.findall`         | gibt alle sich nicht überschneidenden übereinstimmenden Muster in einer       |
|                               | Zeichenkette als Liste zurück.                                                |
+-------------------------------+-------------------------------------------------------------------------------+
| :py:func:`re.finditer`        | wie ``findall``, gibt aber einen Iterator zurück.                             |
+-------------------------------+-------------------------------------------------------------------------------+
| :py:func:`re.match`           | entspricht dem Muster am Anfang der Zeichenkette und segmentiert optional die |
|                               | Musterkomponenten in Gruppen; wenn das Muster übereinstimmt, wird ein         |
|                               | ``match``-Objekt zurückgegeben, andernfalls keines.                           |
+-------------------------------+-------------------------------------------------------------------------------+
| :py:func:`re.search`          | durchsucht die Zeichenkette nach Übereinstimmungen mit dem Muster; gibt in    |
|                               | diesem Fall ein ``match``-Objekt zurück; im Gegensatz zu ``match`` kann die   |
|                               | Übereinstimmung an einer beliebigen Stelle der Zeichenkette und nicht nur am  |
|                               | Anfang stehen.                                                                |
+-------------------------------+-------------------------------------------------------------------------------+
| :py:func:`re.split`           | zerlegt die Zeichenkette bei jedem Auftreten des Musters in Teile.            |
+-------------------------------+-------------------------------------------------------------------------------+
| :py:func:`re.sub`,            | ersetzt alle (``sub``) oder die ersten ``n`` Vorkommen (``subn``) des Musters |
| :py:func:`re.subn`            | in der Zeichenkette durch einen Ersetzungsausdruck; verwendet die Symbole     |
|                               | ``\1``, ``\2``, …, um auf die Elemente der Übereinstimmungsgruppe zu          |
|                               | verweisen.                                                                    |
+-------------------------------+-------------------------------------------------------------------------------+
| :py:meth:`str.removeprefix`   | In Python 3.9 kann dies verwendet werden, um das Suffix oder den Dateinamen   |
| :py:meth:`str.removesuffix`   | zu extrahieren.                                                               |
+-------------------------------+-------------------------------------------------------------------------------+


.. seealso::
   * :doc:`python3:howto/regex`
   * :doc:`python3:library/re`

.. toctree::
   :titlesonly:
   :hidden:

   regex

Konvertieren von Zeichenketten in Zahlen
----------------------------------------

Ihr könnt die Funktionen :class:`python3:int` und :class:`python3:float`
verwenden, um Zeichenketten in Ganzzahl- bzw. Fließkommazahlen zu konvertieren.
Wenn eine Zeichenkette übergeben wird, die nicht als Zahl des angegebenen Typs
interpretiert werden kann, lösen diese Funktionen eine
:class:`python3:ValueError`-Ausnahme aus. Ausnahmen werden in
:doc:`../../control-flows/exceptions` ausführlicher erklärt. Darüber hinaus
könnt ihr :class:`python3:int` einen optionalen zweiten
:doc:`../../functions/params` übergeben, der die numerische Basis angibt, die
bei der Interpretation der Zeichenfolge verwendet werden soll:

.. code-block:: pycon
   :linenos:

   >>> float("12.34")
   12.34
   >>> float("12e3")
   12000.0
   >>> int("1000")
   1000
   >>> int("1000", base=10)
   1000
   >>> int("1000", 8)
   512
   >>> int("1000", 2)
   8
   >>> int("1234", 2)
   Traceback (most recent call last):
     File "<stdin>", line 1, in <module>
   ValueError: invalid literal for int() with base 2: '1234'

Zeilen 5–8
    Wird kein zweiter :doc:`../../functions/params` angegeben, rechnet
    :class:`python3:int` mit einer Basis von ``10``.
Zeilen 9, 10
    ``1000`` wird als `Oktalzahl <https://de.wikipedia.org/wiki/Oktalsystem>`_
    interpretiert.
Zeilen 11, 12
    ``1000`` wird als `Dualzahl <https://de.wikipedia.org/wiki/Dualsystem>`_
    interpretiert.
Zeilen 13–16
    ``1234`` kann nicht als Ganzzahl auf der Basis ``2`` angegeben werden. Daher
    wird eine :class:`python3:ValueError`-Ausnahme ausgelöst.

Ändern von Zeichenketten mit Listenmanipulationen
-------------------------------------------------

Da :ref:`str <python3:textseq>`-Objekte unveränderlich sind, gibt es keine
Möglichkeit, sie direkt zu verändern wie :doc:`../lists`. Ihr könnt sie jedoch
in Listen umwandeln:

.. code-block:: pycon

   >>> palindromes = "lol level gag"
   >>> palindromes_list = list(palindromes)
   >>> palindromes_list.reverse()
   >>> "".join(palindromes_list)
   'gag level lol'

Objekte in Zeichenketten konvertieren
-------------------------------------

In Python kann fast alles in eine Zeichenkette mit der eingebauten Funktion
:ref:`str <python3:textseq>` umgewandelt werden:

.. code-block:: pycon

   >>> data_types = [(7, "Data types", 19), (7.1, "Numbers", 19), (7.2, "Lists", 23)]
   >>> (
   ...     "The title of chapter "
   ...     + str(data_types[0][0])
   ...     + " is «"
   ...     + data_types[0][1]
   ...     + "»."
   ... )
   'The title of chapter 7 is «Data types».'

Das Beispiel verwendet :ref:`str <python3:textseq>`, um eine Ganzzahl aus der
Liste ``data_types`` in eine Zeichenkette umzuwandeln, die dann wieder
aneinanderhängt werden, um die endgültige Zeichenkette zu bilden.

.. note::
   Während :ref:`str <python3:textseq>` meist verwendet wird, um für Menschen
   lesbare Texte zu erzeugen, wird :func:`python3:repr` eher für
   Debugging-Ausgaben oder Statusberichte verwendet, :abbr:`z.B. (zum
   Beispiel)`, um Informationen über die eingebaute Python-Funktion
   :func:`python3:len` zu erhalten:

   .. code-block:: pycon

      >>> repr(len)
      '<built-in function len>'

``print()``
-----------

Die Funktion :func:`print` gibt Zeichenketten aus wobei andere Python-Datentypen
leicht in Strings umgewandelt und formatiert werden können, :abbr:`z.B. (zum
Beispiel)`:

.. code-block:: pycon

   >>> import math
   >>> pi = math.pi
   >>> d = 28
   >>> u = pi * d
   >>> print(
   ...     "Pi ist",
   ...     pi,
   ...     "und der Umfang bei einem Durchmesser von",
   ...     d,
   ...     "Zoll ist",
   ...     u,
   ...     "Zoll.",
   ... )
   Pi ist 3.141592653589793 und der Umfang bei einem Durchmesser von 28 Zoll ist 87.96459430051421 Zoll.

F-Strings
~~~~~~~~~

Mit F-Strings lassen sich die für einen Text zu detaillierten Zahlen kürzen:

.. code-block:: pycon

   >>> print(f"Der Wert von Pi ist {pi:.3f}.")
   Der Wert von Pi ist 3.142.

In ``{pi:.3f}`` wird die Format-Spezifikation ``f`` verwendet, um die Zahl Pi
auf drei Nachkommastellen zu kürzen.

In A/B-Testszenarien möchtet ihr oft die prozentuale Veränderung einer Kennzahl
darstellen. Mit F-Strings können sie verständlich formuliert werden:

.. code-block:: pycon

   >>> metrics = 0.814172
   >>> print(f"Die AUC hat sich vergrößert auf {metrics:=+7.2%}")
   Die AUC hat sich vergrößert auf +81.42%

In diesem Beispiel wird die Variable ``metrics`` formatiert, wobei ``=`` die
Inhalte der Variable nach dem ``+`` übernimmt, wobei insgesamt sieben Zeichen
einschließlich des Vorzeichen, ``metrics`` und des Prozentzeichens angezeigt
werden. ``.2`` sorgt für zwei Dezimalstellen, während das ``%``-Symbol den
Dezimalwert in eine Prozentzahl umwandelt. So wird ``0.514172`` in ``+51.42%``
umgewandelt.

Werte lassen sich auch in binäre und hexadezimale Werte umrechnen:

.. code-block:: pycon

   >>> block_size = 192
   >>> print(f"Binary block size: {block_size:b}")
   Binary block size: 11000000
   >>> print(f"Hex block size: {block_size:x}")
   Hex block size: c0

Es gibt auch Formatierungsangaben, die ideal geeignet sind für die :abbr:`CLI
(Command Line Interface)`-Ausgabe, :abbr:`z.B. (zum Beispiel)`:

.. code-block:: pycon

   >>> data_types = [(7, "Data types", 19), (7.1, "Numbers", 19), (7.2, "Lists", 23)]
   >>> for n, title, page in data_types:
   ...     print(f"{n:.1f} {title:.<25} {page: >3}")
   ...
   7.0 Data types...............  19
   7.1 Numbers..................  19
   7.2 Lists....................  23

Allgemein sieht das Format folgendermaßen aus, wobei alle Angaben in eckigen
Klammern optional sind:

:samp:`:[[FILL]ALIGN][SIGN][0b|0o|0x|d|n][0][WIDTH][GROUPING]["." PRECISION][TYPE]`

In der folgenden Tabelle sind die Felder für die Zeichenkettenformatierung und
ihre Bedeutung aufgeführt:

+-----------------------+-------------------------------------------------------+
| Feld                  | Bedeutung                                             |
+=======================+=======================================================+
| :samp:`FILL`          | Zeichen, das zum Ausfüllen von :samp:`ALIGN` verwendet|
|                       | wird. Der Standardwert ist ein Leerzeichen.           |
+-----------------------+-------------------------------------------------------+
| :samp:`ALIGN`         | Textausrichtung und Füllzeichen:                      |
|                       |                                                       |
|                       | | ``<``: linksbündig                                  |
|                       | | ``>``: rechtsbündig                                 |
|                       | | ``^``: zentriert                                    |
|                       | | ``=``: Füllzeichen nach :samp:`SIGN`                |
+-----------------------+-------------------------------------------------------+
| :samp:`SIGN`          | Vorzeichen anzeigen:                                  |
|                       |                                                       |
|                       | | ``+``: Vorzeichen bei positiven und negativen       |
|                       |    Zahlen anzeigen                                    |
|                       | | ``-``: Standardwert, ``-`` nur bei negativen Zahlen |
|                       |   oder Leerzeichen bei positiven Zahlen               |
+-----------------------+-------------------------------------------------------+
| :samp:`0b|0o|0x|d|n`  | Vorzeichen für ganze Zahlen:                          |
|                       |                                                       |
|                       | | ``0b``: Binärzahlen                                 |
|                       | | ``0o``: Oktalzahlen                                 |
|                       | | ``0x``: Hexadezimalzahlen                           |
|                       | | ``d``: Standardwert, dezimale Ganzzahl zur Basis 10 |
|                       | | ``n``: verwendet die aktuelle                       |
|                       |   ``locale``-Einstellung, um die entsprechenden       |
|                       |   Zahlentrennzeichen einzufügen                       |
+-----------------------+-------------------------------------------------------+
| :samp:`0`             | füllt mit Nullen auf                                  |
+-----------------------+-------------------------------------------------------+
| :samp:`WIDTH`         | Minimale Feldbreite                                   |
+-----------------------+-------------------------------------------------------+
| :samp:`GROUPING`      | Zahlentrennzeichen: [#]_                              |
|                       |                                                       |
|                       | | ``,``: Komma als Tausendertrennzeichen              |
|                       | | ``_``: Unterstrich für Tausendertrennzeichen        |
+-----------------------+-------------------------------------------------------+
| :samp:`.PRECISION`    | | Bei Fließkommazahlen die Anzahl der Ziffern nach    |
|                       |   dem Punkt                                           |
|                       | | bei nicht-numerischen Werten die maximale Länge     |
+-----------------------+-------------------------------------------------------+
| :samp:`TYPE`          | Ausgabeformat als Zahlentyp oder Zeichenkette         |
|                       |                                                       |
|                       | … für Ganzzahlen:                                     |
|                       |                                                       |
|                       | | ``b``: Binärformat                                  |
|                       | | ``c``: konvertiert die Ganzzahl in das              |
|                       |   entsprechende Unicode-Zeichen                       |
|                       | | ``d``: Standardwert, Dezimalzeichen                 |
|                       | | ``n``: dasselbe wie ``d``, mit dem Unterschied,     |
|                       |   dass es die aktuelle ``locale``-Einstellung         |
|                       |   verwendet, um die entsprechenden Zahlentrennzeichen |
|                       |   einzufügen                                          |
|                       | | ``o``: Oktalformat                                  |
|                       | | ``x``: Hexadezimalformat zur Basis 16, wobei für    |
|                       |   die Ziffern über 9 Kleinbuchstaben verwendet werden |
|                       | | ``X``: Hexadezimalformat zur Basis 16, wobei für    |
|                       |   die Ziffern über 9 Großbuchstaben verwendet werden  |
|                       |                                                       |
|                       | … für Fließkommazahlen:                               |
|                       |                                                       |
|                       | | ``e``: Exponent mit ``e`` als Trennzeichen zwischen |
|                       |   Koeffizient und Exponent                            |
|                       | | ``E``: Exponent mit ``E`` als Trennzeichen zwischen |
|                       |   Koeffizient und Exponent                            |
|                       | | ``g``: Standardwert für Fließkommazahlen, wobei der |
|                       |   Exponent eine feste Breite für große und            |
|                       |   kleine Zahlen erhält                                |
|                       | | ``G``: Wie ``g``, wechselt aber zu ``E``, wenn      |
|                       |   die Zahl zu groß wird. Die Darstellungen von        |
|                       |   Unendlich und NaN werden ebenfalls in Großbuchstaben|
|                       |   geschrieben                                         |
|                       | | ``n``: Wie ``g`` mit dem Unterschied, dass es die   |
|                       |   aktuelle ``locale``-Einstellung verwendet, um die   |
|                       |   die entsprechenden Zahlentrennzeichen einzufügen    |
|                       | | ``%``: Prozentsatz. Multipliziert die Zahl mit 100  |
|                       |   und zeigt sie im festen Format ``f`` an, gefolgt    |
|                       |   von einem Prozentzeichen                            |
+-----------------------+-------------------------------------------------------+

.. [#] Der Formatbezeichner ``n`` formatiert eine Zahl in einer lokal angepassten
    Weise, :abbr:`z.B. (zum Beispiel)`:

    .. code-block:: pycon

       >>> value = 635372
       >>> import locale
       >>> locale.setlocale(locale.LC_NUMERIC, "en_US.utf-8")
       'en_US.utf-8'
       >>> print(f"{value:n}")
       635,372

.. tip::
   Eine gute Quelle für F-Strings ist die Hilfe-Funktion:

   .. code-block:: pycon

      >>> help()
      help> FORMATTING
      ...

   Ihr könnt die Hilfe hier durchblättern und viele Beispiele finden.

   Mit :kbd:`:`–:kbd:`q` und :kbd:`⏎` könnt ihr die Hilfe-Funktion wieder
   verlassen.

.. seealso::
   * `PyFormat <https://pyformat.info>`_
   * :ref:`python3:f-strings`
   * :pep:`498`

Fehlersuche in F-Strings
::::::::::::::::::::::::

In Python 3.8 wurde ein Spezifizierer eingeführt, der bei der Fehlersuche in
F-String-Variablen hilft. Durch Hinzufügen eines Gleichheitszeichens ``=`` wird der
Code innerhalb des F-Strings aufgenommen:

.. code-block:: pycon

   >>> uid = "veit"
   >>> print(f"My name is {uid.capitalize()=}")
   My name is uid.capitalize()='Veit'

Formatierung von Datums-, Zeitformaten und IP-Adressen
::::::::::::::::::::::::::::::::::::::::::::::::::::::

:py:mod:`datetime` unterstützt die Formatierung von Zeichenketten mit der
gleichen Syntax wie die :py:meth:`strftime <datetime.datetime.strftime>`-Methode
für diese Objekte.

.. code-block:: pycon

   >>> import datetime
   >>> today = datetime.date.today()
   >>> print(f"Today is {today:%d %B %Y}.")
   Today is 26 November 2023.

Das :py:mod:`ipaddress`-Modul von Python unterstützt auch die Formatierung von
``IPv4Address``- und ``IPv6Address``-Objekten.

Schließlich können Bibliotheken von Drittanbietern auch ihre eigene
Unterstützung für die Formatierung von Strings hinzufügen, indem sie eine
``__format__``-Methode zu ihren Objekten hinzufügen.

.. seealso::
   * :ref:`format-codes`
   * `Python strftime cheatsheet <https://strftime.org>`_

Eingebaute Module für Zeichenketten
-----------------------------------

Die Python-Standardbibliothek enthält eine Reihe eingebauter Module, mit denen
ihr Zeichenketten managen könnt:

.. _string-modules:

+-----------------------+-------------------------------------------------------------------------------+
| Modul                 | Beschreibung                                                                  |
+=======================+===============================================================================+
| :py:mod:`string`      | vergleicht mit Konstanten wie :py:data:`string.digits` oder                   |
|                       | :py:data:`string.whitespace`                                                  |
+-----------------------+-------------------------------------------------------------------------------+
| :py:mod:`re`          | sucht und ersetzt Text mit regulären Ausdrücken                               |
+-----------------------+-------------------------------------------------------------------------------+
| :py:mod:`struct`      | konvertiert zwischen Python-Werten und C-Strukturen, die als                  |
|                       | Python-Bytes-Objekte dargestellt werden.                                      |
+-----------------------+-------------------------------------------------------------------------------+
| :py:mod:`difflib`     | hilft beim Berechnen von Deltas, beim Auffinden von Unterschieden zwischen    |
|                       | Zeichenketten oder Sequenzen und beim Erstellen von Patches und Diff-Dateien  |
+-----------------------+-------------------------------------------------------------------------------+
| :py:mod:`textwrap`    | umbricht und füllt Text, formatiert Text mit Zeilenumbrüchen oder Leerzeichen |
+-----------------------+-------------------------------------------------------------------------------+

.. _end-string-modules:

.. seealso::
   * :doc:`Manipulation von Zeichenketten mit pandas
     <Python4DataScience:workspace/pandas/string-manipulation>`
   * `humanize <https://humanize.readthedocs.io/en/stable/>`_

.. toctree::
   :titlesonly:
   :hidden:

   encodings

Checks
------

* Könnt ihr :abbr:`z.B. (zum Beispiel)` eine Zeichenkette mit einer ganzen Zahl
  addieren oder multiplizieren, oder mit einer Gleitkommazahl oder einer
  komplexen Zahl?

* Wie könnt ihr eine Überschrift wie ``variables and expressions`` so abändern,
  dass sie keine Leerzeichen mehr enthält und besser als Dateinamen verwendet
  werden kann?

* Welche der folgenden Zeichenketten können nicht in Zahlen umgewandelt werden
  und warum?

  * ``int("1e2")``
  * ``int(1e+2)``
  * ``int("1+2")``
  * ``int("+2")``

* Wenn ihr überprüfen wollt, ob eine Zeile mit ``.. note::`` beginnt, welche
  Methode würdet ihr verwenden? Gibt es auch noch andere Möglichkeiten?

* Angenommen, ihr habt eine Zeichenkette mit Ausrufezeichen, Anführungszeichen
  und Zeilenumbruch. Wie können diese aus der Zeichenkette entfernt werden?

* Wie könnt ihr **alle** Leerräume und Satzzeichen aus einer Zeichenfolge in
  einen Bindestrich (``-``) ändern?

* Welche Anwendungsfälle könnt ihr euch vorstellen, in denen das
  :mod:`python3:struct`-Modul für das Lesen oder Schreiben von Binärdaten
  nützlich wäre?

  * beim Lesen und Schreiben einer Binärdatei
  * beim Lesen von einer externen Schnittstelle, wobei die Daten genau so
    gespeichert werden sollen, wie sie übermittelt wurden

* Welchen regulären Ausdruck würdet ihr verwenden, um Zeichenfolgen zu finden,
  die die Zahlen zwischen -3 und +3 darstellen?

* Welchen regulären Ausdruck würdet ihr verwenden, um Hexadezimalwerte zu
  finden?
