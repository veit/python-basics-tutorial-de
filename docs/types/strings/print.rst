``print()``
===========

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

.. _f-strings:

F-Strings
---------

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
~~~~~~~~~~~~~~~~~~~~~~~~

In Python 3.8 wurde ein Spezifizierer eingeführt, der bei der Fehlersuche in
F-String-Variablen hilft. Durch Hinzufügen eines Gleichheitszeichens ``=`` wird der
Code innerhalb des F-Strings aufgenommen:

.. code-block:: pycon

   >>> uid = "veit"
   >>> print(f"My name is {uid.capitalize()=}")
   My name is uid.capitalize()='Veit'

Formatierung von Datums- und Zeitformaten
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

:py:mod:`datetime` unterstützt die Formatierung von Zeichenketten mit der
gleichen Syntax wie die :py:meth:`datetime.strftime
<datetime.datetime.strftime>`-Methode für diese Objekte.

.. code-block:: pycon

   >>> import datetime
   >>> import locale
   >>> locale.setlocale(locale.LC_TIME, "de_DE.utf-8")
   'de_DE.utf-8'
   >>> today = datetime.date.today()
   >>> print(f"Heute ist {today:%A, %d. %B %Y}.")
   Heute ist Freitag, 11. Juli 2025.

Umgekehrt könnt ihr mit :meth:`datetime.strptime <datetime.datetime.strptime>`
auch Zeichenketten in ``datetime``-Objekte umwandeln:

.. code-block:: pycon

   >>> today_string = "Fri, 11 Jul 2025 18:46:49"
   >>> today = datetime.datetime.strptime(today_string, "%A, %d. %B %Y")

.. code-block:: pycon

   >>> today_string = "Fri, 11 Jul 2025 18:46:49"
   >>> today = datetime.datetime.strptime(today_string, "%A, %d. %B %Y")

.. csv-table:: Häufige Formatierungen
   :header: "Beschreibung", "Beispiel", "Format"

   ISO 8601,              "2025-07-11T18:46:49",       "%Y-%m-%dT%H:%M:%S"
   ISO 8601 mit Zeitzone, "2025-07-11T18:46:49+0100",  "%Y-%m-%dT%H:%M:%S%z"
   RFC 2822,              "Fr, 11 Jul 2025 18:46:49",  "%a, %d %b %Y %H:%M:%S"
   RFC 3339 mit Zeitzone, "2025-07-11 18:46:49+0100",  "%Y-%m-%d %H:%M:%S%z"

Formatierung von IP-Adressen
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Das :py:mod:`ipaddress`-Modul von Python unterstützt auch die Formatierung von
``IPv4Address``- und ``IPv6Address``-Objekten.

Schließlich können Bibliotheken von Drittanbietern auch ihre eigene
Unterstützung für die Formatierung von Strings hinzufügen, indem sie eine
``__format__``-Methode zu ihren Objekten hinzufügen.

.. seealso::
   * :ref:`format-codes`
   * `Python strftime cheatsheet <https://strftime.org>`_
