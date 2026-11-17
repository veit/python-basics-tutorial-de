None
====

Zusätzlich zu den Standardtypen wie :doc:`strings/index` und
:doc:`numbers/index` verfügt Python über einen speziellen Datentyp, der ein
einziges spezielles Datenobjekt namens ``None`` definiert. Wie der Name schon
sagt, wird ``None`` verwendet, um einen leeren Wert darzustellen. Er taucht in
verschiedenen Formen in Python auf.

``None`` ist in der alltäglichen Python-Programmierung oft als Platzhalter
nützlich, um eine Datenstruktur zu kennzeichnen, in der irgendwann sinnvolle
Daten gefunden werden können, auch wenn diese Daten noch nicht berechnet wurden.

Das Vorhandensein von ``None`` lässt sich leicht überprüfen, da es in Python
nur eine Instanz von ``None`` gibt (alle Verweise auf ``None`` verweisen auf
dasselbe Objekt), und ``None`` ist nur mit sich selbst identisch:

.. code-block:: pycon

   >>> MyType = type(None)
   >>> MyType() is None
   True

:class:`None` ist ``False``
---------------------------

In Python verlassen wir uns oft darauf, dass :class:`None` ``False`` ist:

.. code-block:: pycon

   >>> bool(None)
   False

So können wir :abbr:`z. B. (zum Beispiel)` in einer :doc:`if-Anweisung
<../control-flow/conditional>` überprüfen, ob :doc:`../types/strings/index` leer
sind:

.. code-block:: pycon

   >>> myval = ""
   >>> if not myval:
   ...     print("No value was specified.")
   ...
   No value was specified.

.. tip::
   Bei Sequenzen wie :doc:`strings/index`, :doc:`sequences-sets/lists` und
   :doc:`sequences-sets/tuples` könnt ihr ausnutzen, dass leere Sequenzen
   ``False`` sind:

   .. code-block:: pycon
      :class: good

      >>> if seq:
      ...     pass
      ...

   Ihr müsst hierfür nicht umständlich :func:`len` verwenden:

   .. code-block:: pycon
      :class: bad

      >>> if len(seq):
      ...     pass
      ...

:class:`None` steht für Leere
-----------------------------

.. code-block:: pycon

   >>> titles = {7.0: "Data Types", 7.1: "Lists", 7.2: "Tuples"}
   >>> third_title = titles.get("7.3")
   >>> type(third_title)
   <class 'NoneType'>

Der Standard-Rückgabewert einer Funktion ist :class:`None`
----------------------------------------------------------

Eine Prozedur in Python ist beispielsweise nur eine Funktion, die nicht explizit
einen Wert zurückgibt, was bedeutet, dass sie standardmäßig ``None`` zurückgibt:

.. code-block:: pycon

   >>> def myfunc():
   ...     pass
   ...
   >>> type(myfunc())
   <class 'NoneType'>

``return None`` ist jedoch aussagekräftiger und wird auch von :pep:`PEP 8
<8#programming-recommendations>` empfohlen, also nicht

.. code-block::
   :class: bad

   def myfunc(x):
       if x >= 0:
           return x**2

sondern

.. code-block::
   :class: good
   :emphasize-lines: 4-

   def myfunc(x):
       if x >= 0:
           return x**2
       else:
           return None
