Komplexe Zahlen
===============

Komplexe Zahlen bestehen aus einem Realteil und einem
`Imaginärteil <https://de.wikipedia.org/wiki/Imagin%C3%A4re_Zahl>`_, der in
Python den Suffix ``j`` erhält.

.. code-block:: pycon

    >>> 7 + 2j
    (7+2j)

.. note::

    Python drückt die resultierende komplexe Zahl in Klammern aus, um
    anzuzeigen, dass die Ausgabe den Wert eines einzelnen Objekts darstellt:

.. code-block:: pycon

    >>> (7 + 2j) - (4 + 4j)
    (3-2j)

.. code-block:: pycon

    >>> 2j * 4j
    (-8+0j)

.. note::

    Die Berechnung von ``2j * 4j`` ergibt die erwartete Antwort von ``-8``, aber
    das Ergebnis bleibt ein Python-Objekt für komplexe Zahlen. Komplexe Zahlen
    werden nie automatisch in entsprechende reelle oder ganzzahlige Objekte
    umgewandelt. Ihr könnt aber leicht auf ihre realen und imaginären Teile mit
    ``real`` und ``imag`` zugreifen.

.. code-block:: pycon

    >>> x = 2j * 4j
    >>> x
    (-8+0j)
    >>> x.real
    -8.0
    >>> x.imag
    0.0

.. note::

    Die Real- und Imaginärteile einer komplexen Zahl werden immer als
    Fließkommazahlen zurückgegeben.

Erweiterte Funktionen
---------------------

Die Funktionen im Modul :doc:`math <python3:library/math>` sind nicht auf
komplexe Zahlen anwendbar; einer der Gründe hierfür dürfte sein, dass die
Quadratwurzel aus ``-1`` einen Fehler erzeugen soll. Daher wurden ähnliche
Funktionen für komplexe Zahlen im
:doc:`cmath <python3:library/cmath>`-Modul bereitgestellt:

:func:`python3:cmath.acos`, :func:`python3:cmath.acosh`, :func:`python3:cmath.asin`, :func:`python3:cmath.asinh`, :func:`python3:cmath.atan`, :func:`python3:cmath.atanh`, :func:`python3:cmath.cos`, :func:`python3:cmath.cosh`, :func:`python3:cmath.e`, :func:`python3:cmath.exp`, :func:`python3:cmath.log`, :func:`python3:cmath.log10`, :func:`python3:cmath.pi`, :func:`python3:cmath.sin`, :func:`python3:cmath.sinh`, :func:`python3:cmath.sqrt`, :func:`python3:cmath.tan`, :func:`python3:cmath.tanh`.

Um im Code deutlich zu machen, dass es sich bei diesen Funktionen um spezielle
Funktionen für komplexe Zahlen handelt, und um Namenskonflikte mit den
normaleren Äquivalenten zu vermeiden, empfiehlt sich der einfache Import des
Moduls um bei der Verwendung der Funktion ausdrücklich auf das ``cmath``-Modul
zu verweisen, :abbr:`z.B. (zum Beispiel)`:

.. code-block:: pycon

    >>> import cmath
    >>> cmath.sqrt(-2)
    1.4142135623730951j

.. warning::

    Nun wird auch verständlicher, weswegen wir nicht den Import aller Funktionen
    eines Moduls empfehlen mit :samp:`from {MODULE} import \*`. Wenn ihr damit
    zuerst das Modul ``math`` und dann das Modul ``cmath`` importieren würdet,
    hätten die Funktionen in ``cmath`` Vorrang vor denen von ``math``. Außerdem
    ist es beim Verstehen des Codes viel mühsamer, die Quelle der verwendeten
    Funktionen herauszufinden.

Checks
------

* Ladet das Modul :mod:`math` und probiert einige der Funktionen aus. Ladet dann
  auch das Modul :mod:`cmath` und macht dasselbe.

* Wie könnt ihr die Funktionen des :mod:`math`-Moduls wiederherstellen?
