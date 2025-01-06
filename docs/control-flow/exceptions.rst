Exceptions
==========

In diesem Abschnitt geht es um :term:`Ausnahmen <Ausnahme>` (englisch
*exceptions*), :abbr:`d.h. (das heißt)` um Sprachfunktionen, die speziell
ungewöhnliche Umstände während der Ausführung eines Programms behandeln. Die
häufigste Ausnahme ist die Behandlung von Fehlern, aber sie können auch für
viele andere Zwecke effektiv eingesetzt werden. Python bietet einen umfassenden
Satz von Ausnahmen, und ihr könnt neue Ausnahmen für eure eigenen Zwecke
definieren.

Eine Exception ist ein Objekt, das automatisch von Python-Funktionen mit einer
:ref:`raise <python3:raise>`-Anweisung erzeugt wird, :abbr:`z.B. (zum Beispiel)`
mit:

.. literalinclude:: exceptions.py
   :language: python
   :linenos:
   :lines: 11-12
   :lineno-start: 11

Die :ref:`raise <python3:raise>`-Anweisung veranlasst die Ausführung des
Python-Programms auf eine andere Art und Weise, als üblicherweise vorgesehen:
Die aktuelle Aufrufkette wird nach einem Handler durchsucht, der die erzeugte
Ausnahme behandeln kann. Wenn ein solcher Handler gefunden wird, wird er
aufgerufen und kann auf das Ausnahmeobjekt zugreifen, um weitere Informationen
zu erhalten, wie in unserem :class:`EmptyFileError`-Beispiel:

.. literalinclude:: exceptions.py
   :language: python
   :linenos:
   :lines: 1-2

Dies definiert euren eigenen Ausnahmetyp, der vom Basistyp ``Exception`` erbt.

Eine Übersicht über die Klassenhierarchie eingebauter Exceptions erhaltet ihr
unter `Exception hierarchy
<https://docs.python.org/3/library/exceptions.html#exception-hierarchy>`_ in der
Python-Dokumentation. Jeder Ausnahmetyp ist eine Python-Klasse, die von ihrem
übergeordneten Exception-Typ erbt. So ist :abbr:`z.B. (zum Beispiel)` ein
``ZeroDivisionError`` durch Vererbung auch ein ``ArithmeticError``, eine
``Exception`` und auch eine ``BaseException``. Diese Hierarchie ist gewollt: Die
meisten Ausnahmen erben von ``Exception``, und es wird dringend empfohlen, dass
alle benutzerdefinierten Ausnahmen auch die Unterklasse von ``Exception``  und
nicht von ``BaseException`` bilden:

Es ist möglich, verschiedene Arten von Ausnahmen zu erzeugen, um die
tatsächliche Ursache des gemeldeten Fehlers oder außergewöhnlichen Umstandes zu
reflektieren.

.. literalinclude:: exceptions.py
   :language: python
   :linenos:
   :lines: 8-16
   :lineno-start: 8

Wenn während der Ausführung von :func:`open` im ``try``-Block ein ``OSError``
oder ein ``EmptyFileError`` auftritt, wird der jeweils zugehörige
``except``-Block ausgeführt.

Wird kein geeigneter Exception-Handler gefunden, bricht das Programm mit einer
Fehlermeldung ab. Daher ergänzen wir unsere  ``try``-``except``-Anweisungen um
``else`` und ``finally``:

.. literalinclude:: exceptions.py
   :language: python
   :linenos:
   :lines: 17-21
   :lineno-start: 17

Nun können wir noch eine Liste unterschiedlicher Datei-Arten definieren, sodass
unser vollständiger Code folgendermaßen aussieht:

.. literalinclude:: exceptions.py
   :language: python
   :linenos:
   :lines: 1-
   :lineno-start: 1

Zeile 7
    Wenn während der Ausführung der Anweisungen im ``try``-Block ein
    ``OSError`` oder ``EmptyFileError`` auftritt, wird der zugehörige
    ``except``-Block ausgeführt.
Zeile 9
    Hier könnte ein ``OSError`` ausgelöst werden.
Zeile 12
    Hier löst ihr den ``EmptyFileError`` aus.
Zeile 17
    Die ``else``-Klausel ist optional; sie wird ausgeführt, wenn im
    ``try``-Block keine Ausnahme auftritt.

Zeile 19
    Die ``finally``-Klausel ist ebenfalls optional und wird am Ende des Blocks
    ausgeführt, unabhängig davon, ob eine Ausnahme ausgelöst wurde oder nicht.

.. note::
   Die Art und Weise, wie Python Fehlersituationen im Allgemeinen behandelt,
   unterscheidet sich von manch anderen Sprachen, :abbr:`z.B. (zum Beispiel)`
   Java. Diese Sprachen prüfen mögliche Fehler so weit wie möglich, bevor sie
   auftreten, da die Behandlung von Exceptions nach ihrem Auftreten kostspielig
   ist. Dies wird manchmal als :term:`LBYL`-Ansatz bezeichnet.

   Bei Python hingegen verlässt man sich eher auf Exceptions, um Fehler zu
   behandeln, nachdem sie aufgetreten sind. Obwohl dieses Vertrauen riskant
   erscheinen mag, ist der Code weniger schwerfällig und leichter zu lesen, wenn
   Exceptions richtig eingesetzt werden, und Fehler werden nur dann behandelt,
   wenn sie auftreten. Diese pythonische Herangehensweise zur Behandlung von
   Fehlern wird oft als :term:`EAFP` beschrieben.

Checks
------

* Schreibt  Code, der zwei Zahlen erhält und die erste Zahl durch die zweite
  dividiert. Prüft, ob der :class:`python3:ZeroDivisionError` auftritt, wenn die
  zweite Zahl ``0`` ist, und fangt diese ab.

* Wenn :class:`MyError` von :class:`Exception` erbt, was ist dann der
  Unterschied zwischen ``except Exception as e`` und ``except MyError as e``?

* Schreibt ein einfaches Programm, das eine Zahl erhält und dann die Anweisung
  :func:`assert` verwendet, um eine :class:`python3:Exception` auszulösen, wenn
  die Zahl ``0`` ist.

* Schreibt eine benutzerdefinierte Ausnahme :class:`Outliers`, die eine
  :class:`Exception` auslöst, wenn die Variable ``x`` größer oder kleiner als
  ``3`` ist?

* Handelt es sich bei der Überprüfung, ob ein Objekt eine Liste ist
  (:ref:`Check: list <check-list>`) um eine Programmierung im Stil von
  :term:`LBYL` oder :term:`EAFP`?
