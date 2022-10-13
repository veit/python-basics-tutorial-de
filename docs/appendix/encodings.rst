Unicode und Zeichenkodierungen
==============================

Es gibt Dutzende von Zeichenkodierungen. Einen Überblick über die Encodings von
Python erhaltet ihr in :ref:`python3:encodings-overview`.

Das ``string``-Modul
--------------------

Das :doc:`string <python3:library/string>`-Modul von Python unterscheidet die
folgenden String-Konstanten, die alle in den ASCII-Zeichensatz fallen:

.. code-block:: python

    # Some strings for ctype-style character classification
    whitespace = ' \t\n\r\v\f'
    ascii_lowercase = 'abcdefghijklmnopqrstuvwxyz'
    ascii_uppercase = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ'
    ascii_letters = ascii_lowercase + ascii_uppercase
    digits = '0123456789'
    hexdigits = digits + 'abcdef' + 'ABCDEF'
    octdigits = '01234567'
    punctuation = r"""!"#$%&'()*+,-./:;<=>?@[\]^_`{|}~"""
    printable = digits + ascii_letters + punctuation + whitespace

Die meisten dieser Konstanten sollten in ihrem Bezeichnernamen selbsterklärend
sein. ``hexdigits`` und ``octdigits`` beziehen sich auf die Hexadezimal-
:abbr:`bzw. (beziehungsweise)` Oktalwerte. Ihr könnt diese Konstanten für
alltägliche String-Manipulation verwenden:

.. code-block:: python

    >>> import string
    >>> hepy = "Hello Pythonistas!"
    >>> hepy.rstrip(string.punctuation)
    'Hello Pythonistas'

Das :doc:`string <python3:library/string>`-Modul arbeitet jedoch standardmäßig
mit Unicode, der als Binärdaten (Bytes) dargestellt wird.

Unicode
-------

Es ist offensichtlich, dass der ASCII-Zeichensatz nicht annähernd groß genug
ist, um alle Sprachen, Dialekte, Symbole und Glyphen zu erfassen; er ist nicht
einmal groß genug für das Englische.

ASCII ist zwar eine vollständige Untermenge von Unicode – die ersten 128 Zeichen
in der Unicode-Tabelle entsprechen genau den ASCII-Zeichen – Unicode umfasst
jedoch eine viel größere Menge von Zeichen. Dabei ist Unicode selbst keine
Kodierung sondern wird durch verschiedene Zeichenkodierungen implementiert wobei
UTF-8 das vermutlich am häufigsten verwendete Kodierungsschema ist.

.. seealso::
    * `Unicode HOWTO
      <https://docs.python.org/3/howto/unicode.html#unicode-howto>`_
    * `What’s New In Python 3.0: Text Vs. Data Instead Of Unicode Vs. 8-bit
      <https://docs.python.org/3.0/whatsnew/3.0.html#text-vs-data-instead-of-unicode-vs-8-bit>`_

Unicode und UTF-8
~~~~~~~~~~~~~~~~~

Während Unicode ein abstrakter Kodierungsstandard ist, ist UTF-8 ein konkretes
Kodierungsschema. Der Unicode-Standard ist eine Zuordnung von Zeichen zu
Codepunkten und definiert mehrere verschiedene Kodierungen aus einem einzigen
Zeichensatz. UTF-8 ist ein Kodierungsschema für die Darstellung von
Unicode-Zeichen als Binärdaten mit einem oder mehreren Bytes pro Zeichen.

Kodierung und Dekodierung in Python 3
-------------------------------------

Der :ref:`str <python3:textseq>`-Typ ist für die Darstellung von
menschenlesbarem Text gedacht und kann alle Unicode-Zeichen enthalten. Der
:ref:`bytes-Typ <python3:typebytes>` hingegen repräsentiert Binärdaten, die
nicht von vornherein mit einer Kodierung versehen sind.
:meth:`python3:str.encode` und :meth:`python3:bytes.decode` sind die Methoden
des Übergangs vom einen zum anderen:

.. code-block:: python

    >>> "schön".encode("utf-8")
    b'sch\xc3\xb6n'
    >>> b"sch\xc3\xb6n".decode("utf-8")
    'schön'

Das Ergebnis von ``str.encode()`` ist ein :ref:`Bytes-Objekt
<python3:typebytes>`. Sowohl Bytes-Literale (wie ``b'sch\xc3\xb6n'``) als auch
die Darstellungen von Bytes lassen nur ASCII-Zeichen zu. Aus diesem Grund darf
beim Aufruf von ``"schön".encode("utf-8")`` das ASCII-kompatible ``"sch"`` so
dargestellt werden, wie es ist, das `ö <https://unicode-table.com/en/00F6/>`_
wird jedoch zu ``"\xc3\xb6"``. Diese chaotisch aussehende Sequenz repräsentiert
zwei Bytes, ``c3`` und ``b6`` als Hexadezimalwerte.

.. tipp::
    In ``.encode()`` und ``.decode()`` ist der Kodierungsparameter standardmäßig
    ``"utf-8"``; dennoch empfiehlt sich, ihn explizit anzugeben.

Mit :meth:`python3:bytes.fromhex` könnt ihr die Hexadezimalwerte in Bytes
umwandeln:

.. code-block:: python

    >>> bytes.fromhex('c3 b6')
    b'\xc3\xb6'

UTF-16 und UTF-32
~~~~~~~~~~~~~~~~~

Der Unterschied zwischen diesen und UTF-8 ist in der Praxis erheblich. Im
Folgenden möchte ich euch nur kurz an einem Beispiel zeigen, dass hier eine
eine Round-Trip-Konvertierung einfach fehlschlagen kann:

.. code-block:: python

    >>> hepy = "Hello Pythonistas!"
    >>> hepy.encode("utf-8")
    b'Hello Pythonistas!'
    >>> len(hepy.encode("utf-8"))
    18
    >>> hepy.encode("utf-8").decode("utf-16")
    '效汬\u206f祐桴湯獩慴ⅳ'
    >>> len(hepy.encode("utf-8").decode("utf-16"))
    9

Die Kodierung von lateinischen Buchstaben in UTF-8 und die anschließende
Dekodierung in UTF-16 führte zu einem Text, der auch Zeichen aus dem
chinesischen, japanischen oder koreanischen Sprachraum sowie römische Ziffern
enthält. Die Dekodierung desselben Byte-Objekts kann zu Ergebnissen führen, die
nicht einmal in derselben Sprache sind oder gleich viele Zeichen enthalten.

Python 3 und Unicode
--------------------

Python 3 setzt voll und ganz auf Unicode und speziell auf UTF-8:

* Der Quellcode von Python 3 wird standardmäßig in UTF-8 angenommen.
* Texte (:ref:`str <python3:textseq>`) sind standardmäßig Unicode. Kodierter
  Unicode-Text wird als Binärdaten (:ref:`Bytes <python3:typebytes>`)
  dargestellt.
* Python 3 akzeptiert viele Unicode-Codepunkte in :ref:`Bezeichnern
  <identifiers>`.
* Pythons :doc:`re-Modul <python3:library/re>` verwendet standardmäßig das
  ``re.UNICODE``-Flag und nicht ``re.ASCII``. Das bedeutet, dass :abbr:`z.B.
  (zum Beispiel)` ``r"\w"`` auf Unicode-Wortzeichen passt, nicht nur auf
  ASCII-Buchstaben.
* Die Standardkodierung in ``str.encode()`` und ``bytes.decode()`` ist UTF-8.

Die einzige Ausnahme könnte :func:`open() <python3:open>` sein, das
plattformabhängig ist und daher vom Wert von
:func:`python3:locale.getpreferredencoding` abhängt:

.. code-block:: python

    >>> import locale
    >>> locale.getpreferredencoding()
    'UTF-8'

Built-In Python-Funktionen
--------------------------

Python verfügt über eine Reihe von eingebauten Funktionen, die sich in
irgendeiner Weise auf Zeichenkodierungen beziehen:

``ascii()``, ``bin()``, ``hex()``, ``oct()``
    geben einen String aus.
``bytes()``, ``str()``, ``int()``
    sind Klassenkonstruktoren für ihre jeweiligen Typen, die die Eingabe in den
    gewünschten Typ konvertiert.
``ord()``, ``chr()``
    sind insofern invers zueinander, als die Python-Funktion ``ord()`` ein
    ``str``-Zeichen in seinen ``base=10``-Codepunkt umwandelt, während ``chr()``
    das Gegenteil tut.

Im Folgenden findet ihr einen detaillierteren Blick auf jede dieser neun
Funktionen:

+-----------------------+---------------+---------------------------------------+
| Funktion              | Rückgabetyp   | Beschreibung                          |
+=======================+===============+=======================================+
| :func:`python3:ascii` | ``str``       | ASCII-Darstellung eines Objekts, wobei|
|                       |               | nicht-ASCII-Zeichen escaped werden    |
+-----------------------+---------------+---------------------------------------+
| :func:`python3:bin`   | ``str``       | binäre Darstellung einer ganzen Zahl  |
|                       |               | mit dem Präfix ``0b``                 |
+-----------------------+---------------+---------------------------------------+
| :func:`python3:hex`   | ``str``       | hexadezimale Darstellung einer ganzen |
|                       |               | Zahl mit dem Präfix ``0x``            |
+-----------------------+---------------+---------------------------------------+
| :func:`python3:oct`   | ``str``       | Oktaldarstellung einer ganzen Zahl    |
|                       |               | mit dem Präfix ``0o``                 |
+-----------------------+---------------+---------------------------------------+
| :class:`python3:bytes`| ``bytes``     | konvertiert die Eingabe in            |
|                       |               | :ref:`bytes-Typ <python3:typebytes>`  |
+-----------------------+---------------+---------------------------------------+
| :class:`python3:str`  | ``str``       | konvertiert die Eingabe in            |
|                       |               | :ref:`str-Typ <python3:textseq>`      |
+-----------------------+---------------+---------------------------------------+
| :class:`python3:int`  | ``int``       | konvertiert die Eingabe in ``int``    |
+-----------------------+---------------+---------------------------------------+
| :func:`python3:ord`   | ``int``       | konvertiert ein einzelnes             |
|                       |               | Unicode-Zeichen in seinen             |
|                       |               | Integer-Codepunkt                     |
+-----------------------+---------------+---------------------------------------+
| :func:`python3:chr`   | ``str``       | wandelt einen Integer-Codepunkt in    |
|                       |               | ein einzelnes Unicode-Zeichen um      |
+-----------------------+---------------+---------------------------------------+
