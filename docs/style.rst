Stil
====

Einrückung und Blöcke
---------------------

Python unterscheidet sich von den meisten anderen Programmiersprachen, weil es
Einrückungen verwendet, um die Struktur zu bestimmen (:abbr:`d.h.(das heißt)` um
zu bestimmen, was die :doc:`while <control-flow/loops>`-Klausel einer Bedingung
:abbr:`usw. (und so weiter)` darstellt). Die meisten anderen Sprachen verwenden
dazu geschweifte Klammern. Im folgenden Beispiel wird durch die Einrückung der
Zeilen 3–6 festgelegt, dass sie zur ``while``-Anweisung gehören:

.. code-block:: pycon
    :linenos:

    >>> x, y = 6, 3
    >>> while x > y:
    ...     x -= 1
    ...     if x == 4:
    ...         break
    ...     print(x)
    ...

Einrückungen zur Strukturierung des Codes anstelle von geschweiften Klammern ist
zwar etwas gewöhnungsbedürftig, bietet aber erhebliche Vorteile:

* Ihr könnt weder fehlende oder zu viele Klammern haben. Auch müsst ihr nicht
  mehr nach der Klammer suchen, die zu früheren Klammern passen könnte.
* Die visuelle Struktur des Codes spiegelt seine tatsächliche Struktur wider,
  wodurch die Struktur des Codes sehr viel einfacher zu verstehen ist.
* Python Codierung-Styles sind meist einheitlich; :abbr:`m.a.W. (mit anderen
  Worten)`, euer Code wird meist sehr ähnlich aussehen, wie derjenige von
  anderen.

Kommentare
----------

Meist ist alles, was hinter ``#`` folgt ein Kommentar und wird bei der
Ausführung des Codes nicht beachtet. Die offensichtliche Ausnahme ist ``#`` in
einer :doc:`Zeichenkette <types/strings/index>`:

.. code-block:: pycon

    >>> x = "# Dies ist eine Zeichenkette und kein Kommentar"

Grundlegender Python-Stil
-------------------------

In Python gibt es nur relativ wenige Einschränkungen für den Codierungsstil, mit
der offensichtlichen Ausnahme, dass der Code durch Einrücken in Blöcke
gegliedert werden muss. Selbst in diesem Fall ist nicht vorgeschrieben, wie
(Tabulatoren oder Leerzeichen) und wie weit eingerückt wird.  Es gibt jedoch
bevorzugte stilistische Konventionen für Python, die im *Python Enhancement
Proposal* (PEP) 8 enthalten sind. Eine Auswahl von Python-Konventionen findet
ihr in der folgenden Tabelle:

+-----------------------+-------------------------------+-------------------------------+
| Kontext               | Empfehlung                    | Beispiel                      |
+=======================+===============================+===============================+
| Modul- und Paketnamen | kurz, Kleinbuchstaben,        | ``math``, ``sys``             |
|                       | Unterstriche nur bei Bedarf   |                               |
+-----------------------+-------------------------------+-------------------------------+
| Funktionsnamen        | Kleinbuchstaben, :abbr:`ggf.` | ``my_func()``                 |
|                       | (gegebenenfalls)` mit         |                               |
|                       | Unterstrichen                 |                               |
+-----------------------+-------------------------------+-------------------------------+
| Variablennamen        | Kleinbuchstaben, :abbr:`ggf.` | ``my_var``                    |
|                       | (gegebenenfalls)` mit         |                               |
|                       | Unterstrichen                 |                               |
+-----------------------+-------------------------------+-------------------------------+
| Klassennamen          | CamelCase-Schreibweise        | ``MyClass``                   |
+-----------------------+-------------------------------+-------------------------------+
| Namen für             | Versalien mit Unterstrichen   | ``PI``                        |
| :term:`Konstanten     |                               |                               |
| <Konstante>`          |                               |                               |
+-----------------------+-------------------------------+-------------------------------+
| Einrückung            | Vier Leerzeichen pro Ebene,   |                               |
|                       | keine Tabs                    |                               |
+-----------------------+-------------------------------+-------------------------------+
| Vergleiche            | nicht explizit mit ``True``   | ``if my_var:``,               |
|                       | oder ``False``, siehe auch    | ``if not my_var:``            |
|                       | :doc:`control-flow/boolean`   |                               |
+-----------------------+-------------------------------+-------------------------------+

.. seealso::

    * :pep:`8`
    * `Google Python Style Guide
      <https://google.github.io/styleguide/pyguide.html>`_

Ich empfehle dringend, die Konventionen von PEP 8 zu befolgen. Sie sind bewährt,
und machen euren Code für euch selbst und andere leichter verständlich.
