Lambda-Funktionen
=================

In Python ist eine Lambda-Funktion eine anonyme Funktion, also eine Funktion,
die ohne Namen deklariert wird. Es handelt sich um eine kleine und
eingeschränkte Funktion, die nicht länger als eine Zeile ist. Wie eine normale
Funktion kann eine Lambda-Funktion mehrere Argumente haben, aber nur einen
Ausdruck, der ausgewertet und zurückgegeben wird.

Die Syntax einer Lambda-Funktion lautet :samp:`lambda {ARGUMENTS}:
{EXPRESSION}`.

.. note::
   Es gibt in der Lambda-Funktion keine ``return``-Anweisung. Der einzelne
   Ausdruck nach dem Doppelpunkt ist der Rückgabewert.

.. tip::
   Verwendet jedoch stets eine ``def``-Anweisung anstelle einer Zuweisung, die
   einen Lambda-Ausdruck direkt an einen Identifikator bindet, also nicht

   .. code-block:: pycon
      :class: bad

      >>> add = lambda x, y: x + y
      >>> add(2, 3)
      5

   sondern

   .. code-block:: pycon
      :class: good

      >>> def add(x, y):
      ...     return x + y
      ...
      >>> add(2, 3)
      5

Dies ist im Allgemeinen für Tracebacks und String-Repräsentationen nützlicher.
Der einzige Vorteil von lambda-Funktionen ist, dass sie in einen größeren
Ausdruck eingebettet werden können. Im nächsten Beispiel sehen wir, wie eine
lambda-Funktion innerhalb eines Funktionsaufrufs erstellt wird. Dabei gibt es
keine globale Variable, um die Werte der lambda-Funktion zu speichern:

.. code-block:: pycon
   :linenos:

   >>> count = ["1", "123", "1000"]
   >>> max(count)
   '123'
   >>> max(count, key=lambda val: int(val))
   '1000'

In diesem Fall akzeptiert die Funktion :py:func:`max` das Argument ``key``, das
definiert, wie die Größe jedes Eintrags bestimmt werden soll. Mithilfe einer
Lambda-Funktion, die jede Zeichenkette in eine ganze Zahl umwandelt, kann
``max`` die numerischen Werte vergleichen und so das erwartete Ergebnis
ermitteln.

.. tip::
   `cusy Seminar: Fortgeschrittenes Python
   <https://cusy.io/de/our-training-courses/advanced-python.html>`_
