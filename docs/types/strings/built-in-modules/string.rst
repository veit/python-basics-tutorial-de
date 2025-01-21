``string``
==========

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
------------------------------

Während :meth:`python3:str.split` eine Liste von Zeichenfolgen zurückgibt, nimmt
:meth:`python3:str.join` eine Liste von Zeichenketten und fügt sie zu einer
einzigen Zeichenkette zusammen. Normalerweise verwendet
:meth:`python3:str.split` Leerraum als Begrenzungszeichen für die aufzuteilenden
Zeichenketten, aber ihr könnt dieses Verhalten mit einem optionalen
:doc:`../../../functions/params` ändern.

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
Zeichenfolge trennen, indem ihr einen optionalen
:doc:`../../../functions/params` übergebt:

.. code-block:: pycon

   >>> example = "1. You can have\n\twhitespaces, newlines\n   and tabs mixed in\n\tthe string."
   >>> example.split()
   ['1.', 'You', 'can', 'have', 'whitespaces,', 'newlines', 'and', 'tabs', 'mixed', 'in', 'the', 'string.']
   >>> license = "License :: OSI Approved"
   >>> license.split(" :: ")
   ['License', 'OSI Approved']

Manchmal ist es nützlich, dem letzten Feld in einer Zeichenkette zu erlauben,
beliebigen Text zu enthalten. Ihr könnt dies tun, indem ihr einen optionalen
zweiten :doc:`../../../functions/params` angebt, wie viele Teilungen
durchgeführt werden sollen:

.. code-block:: pycon

   >>> example.split(" ", 1)
   ['1.', 'You can have\n\twhitespaces, newlines\n   and tabs mixed in\n\tthe string.']

Wenn ihr :meth:`python3:str.split` mit dem optionalen zweiten Argument verwenden
wollt, müsst ihr zunächst ein erstes Argument angeben. Um zu erreichen, dass bei
allen Leerzeichen geteilt wird, verwendet :doc:`../../none` als erstes Argument:

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
------------------

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
Variable :py:data:`string.whitespace` zugreift. Bei mir wird das folgende
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
   :doc:`../../../functions/params` übergeben:

   .. code-block:: pycon

      >>> url = "https://www.cusy.io/"
      >>> url.strip("htps:/w.")
      'cusy.io'

Suche in Zeichenketten
----------------------

:ref:`str <python3:textseq>`-Objekte bieten mehrere Methoden für die einfache
Suche nach Zeichenketten: Die vier grundlegenden Methoden für die Suche nach
Zeichenketten sind :py:meth:`str.find`, :py:meth:`str.rfind`,
:py:meth:`str.index` und :py:meth:`str.rindex`. Eine verwandte Methode,
:py:meth:`str.count`, zählt, wie oft eine Zeichenfolge in einer anderen
Zeichenfolge gefunden werden kann.

:py:meth:`str.find` benötigt einen einzigen :doc:`../../../functions/params`:
die gesuchte Teil-Zeichenkette; zurückgegeben wird dann die Position des ersten
Vorkommens oder ``-1``, wenn es kein Vorkommen gibt:

.. code-block:: pycon

   >>> hipy = "Hello Pythonistas!\n"
   >>> hipy.find("\n")
   18

:py:meth:`str.find` kann auch ein oder zwei zusätzliche
:doc:`../../../functions/params` annehmen:

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
mit einer der als :doc:`../../../functions/params` angegebenen Zeichenketten
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
--------------------

:ref:`str <python3:textseq>`-Objekte sind :term:`unveränderlich
<Unveränderlich>`, aber sie verfügen über mehrere Methoden, die eine
modifizierte Version der ursprünglichen Zeichenkette zurückgeben können.

:py:meth:`str.replace` könnt ihr verwenden, um Vorkommen des ersten
:doc:`../../../functions/params` durch den zweiten zu ersetzen, :abbr:`z.B. (zum
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

Checks
------

* Wie könnt ihr eine Überschrift wie ``variables and expressions`` so abändern,
  dass sie keine Leerzeichen mehr enthält und besser als Dateinamen verwendet
  werden kann?

* Wenn ihr überprüfen wollt, ob eine Zeile mit ``.. note::`` beginnt, welche
  Methode würdet ihr verwenden? Gibt es auch noch andere Möglichkeiten?

* Angenommen, ihr habt eine Zeichenkette mit Ausrufezeichen, Anführungszeichen
  und Zeilenumbruch. Wie können diese aus der Zeichenkette entfernt werden?

* Wie könnt ihr **alle** Leerräume und Satzzeichen aus einer Zeichenfolge in
  einen Bindestrich (``-``) ändern?
