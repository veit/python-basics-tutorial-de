Stil
====

Einrückung und Blöcke
---------------------

Python unterscheidet sich von den meisten anderen Programmiersprachen, weil es
Einrückungen verwendet, um die Struktur zu bestimmen (:abbr:`d.h.(das heißt)` um
zu bestimmen, was die :doc:`while <control-flows/loops>`-Klausel einer Bedingung
:abbr:`usw. (und so weiter)` darstellt). Die meisten anderen Sprachen verwenden
dazu geschweifte Klammern. Im folgenden Beispiel wird durch die Einrückung der
Zeilen 3–6 festgelegt, dass sie zur ``while``-Anweisung gehören:

.. code-block:: python
    :linenos:

    >>> x, y = 6, 3
    >>> while x > y:
    ...     x -= 1
    ...     if x == 4:
    ...         break
    ...     print(x)

Einrückungen zur Strukturierung des Codes anstelle von geschweiften Klammern ist
zwar etwas gewöhnungsbedürftig, bietet aber erhebliche Vorteile:

* Ihr könnt weder fehlende oder zu viele Klammern haben. Auch müsst ihr nicht
  mehr nach der Klammer suchen, die zu früheren Klammern passen könnte.
* Die visuelle Struktur des Codes spiegelt seine tatsächliche Struktur wider,
  wodurch die Struktur des Codes sehr viel einfacher zu verstehen ist.
* Python Codierung-Styles sind meist einheitlich; :abbr. `m.a.W. (mit anderen
  Worten)`, euer Code wird meist sehr ähnlich aussehen, wie derjenige von
  anderen.

Kommentare
----------

Meist ist alles, was hinter ``#`` folgt ein Kommentar und wird bei der
Ausführung des Codes nicht beachtet. Die offensichtliche Ausnahme ist ``#`` in
einer :doc:`Zeichenkette <types/strings>`:

.. code-block:: python

    >>> x = "# Dies ist eine Zeichenkette und kein Kommentar"
