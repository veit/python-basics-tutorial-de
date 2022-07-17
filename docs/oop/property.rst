@property-Dekorator
===================

In Python könnt ihr direkt auf Instanzvariablen zugreifen, ohne zusätzliche
Getter- und Setter-Methoden, die häufig in Java und anderen objektorientierten
Sprachen verwendet werden. Dies macht das Schreiben von Python-Klassen sauberer
und einfacher, aber in manchen Situationen kann die Verwendung von Getter- und
Setter-Methoden auch nützlich sein. Nehmen wir an, dass ihr einen Wert benötigt,
bevor ihr ihn in eine Instanzvariable setzt, oder ihr einfach den Wert eines
Attributs herausfinden möchtet. In beiden Fällen würden Getter- und
Setter-Methoden die Aufgabe erfüllen, allerdings um den Preis, dass der einfache
Zugriff auf Instanzvariablen in Python verloren ginge.

Die Antwort ist die Verwendung einer *Property*. Diese kombiniert die
Möglichkeit, den Zugriff auf eine Instanzvariable über Methoden wie Getter und
Setter zu übergeben, mit dem einfachen Zugriff auf Instanzvariablen über die
Punktnotation.  Um eine *Property* zu erstellen, wird der
:class:`python3:property`-Dekorator mit einer Methode verwendet, die den Namen
der Eigenschaft hat:

.. literalinclude:: form_pr.py
    :language: python
    :linenos:
    :lines: 11-18
    :lineno-start: 11

Ohne Setter ist die *Property* ``length`` jedoch schreibgeschützt:

.. code-block:: python

    >>> s1 = form.Square()
    >>> s1.length = 2
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    AttributeError: can't set attribute

Um dies zu ändern, müsst ihr einen Setter hinzufügen:

.. literalinclude:: form_pr.py
    :language: python
    :linenos:
    :lines: 19-21
    :lineno-start: 19

Jetzt könnt ihr die Punkt-Notation verwenden, um die Eigenschaft ``length``
sowohl zu erhalten als auch zu setzen. Beachtet, dass der Name der Methode
derselbe bleibt, aber der Dekorator ändert sich in den *Property*-Namen, in
unserem Fall in ``length.setter``:

.. code-block:: python

    >>> s1 = form.Square()
    >>> s1.length = 2
    >>> s1.circumference()
    8

Ein großer Vorteil von Pythons Fähigkeit, Eigenschaften hinzuzufügen, besteht
darin, dass ihr zu Beginn der Entwicklung mit einfachen alten Instanzvariablen
arbeiten und dann nahtlos zu *Property*-Variablen wechseln könnt, wann immer
und wo immer ihr dies benötigt, ohne den Client-Code zu ändern. Der Zugriff ist
immer noch derselbe, unter Verwendung der Punktnotation.
