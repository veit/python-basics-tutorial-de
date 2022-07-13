Exceptions
==========

In diesem Abschnitt geht es um Ausnahmen, :abbr:`d.h. (das heißt)` um
Sprachfunktionen, die speziell ungewöhnliche Umstände während der Ausführung
eines Programms behandeln. Die häufigste Ausnahme ist die Behandlung von
Fehlern, aber sie können auch für viele andere Zwecke effektiv eingesetzt
werden. Python bietet einen umfassenden Satz von Ausnahmen, und ihr könnt neue
Ausnahmen für eure eigenen Zwecke definieren.

Der gesamte Exception-Mechanismus in Python ist :doc:`objektorientiert
</oop/index>`: Eine Exception ist ein Objekt, das automatisch von
Python-Funktionen mit einer ``raise``-Anweisung erzeugt wird. Diese
``raise``-Anweisung veranlasst die Ausführung des Python-Programms auf eine
andere Art und Weise, als üblicherweise vorgesehen: Die aktuelle Aufrufkette
wird nach einem Handler durchsucht, der die erzeugte Ausnahme behandeln kann.
Wenn ein solcher Handler gefunden wird, wird er aufgerufen und kann auf das
Ausnahmeobjekt zugreifen, um weitere Informationen zu erhalten. Wird kein
geeigneter Exception-Handler gefunden, bricht das Programm mit einer
Fehlermeldung ab.

Es ist möglich, verschiedene Arten von Ausnahmen zu erzeugen, um die
tatsächliche Ursache des gemeldeten Fehlers oder außergewöhnlichen Umstandes zu
reflektieren. Eine Übersicht über die Klassenhierarchie eingebauter Exceptions
erhaltet ihr unter `Exception hierarchy
<https://docs.python.org/3/library/exceptions.html#exception-hierarchy>`_ in der
Python-Dokumentation. Jeder Ausnahmetyp ist eine Python-Klasse, die von ihrem
übergeordneten Exception-Typ erbt. So ist :abbr:`z.B. (zum Beispiel` ein
``ZeroDivisionError`` durch Vererbung auch ein ``ArithmeticError``, eine
``Exception`` und auch eine ``BaseException``. Diese Hierarchie ist gewollt: Die
meisten Ausnahmen erben von ``Exception``, und es wird dringend empfohlen, dass
alle benutzerdefinierten Ausnahmen auch die Unterklasse ``, Exception``,  und
nicht ``, BaseException``,  bilden:

.. literalinclude:: exceptions.py
   :language: python
   :lines: 1-2

Dies definiert ihr euren eigenen Ausnahmetyp, der vom Basistyp ``Exception``
erbt.

.. literalinclude:: exceptions.py
   :language: python
   :lines: 3

Eine Liste unterschiedlicher Datei-Arten wird definiert.

Schließlich werden Ausnahmen oder Fehler mit Hilfe der zusammengesetzten
Anweisung ``try``-``except``-``else``-``finally`` abgefangen und behandelt.
Jede Ausnahme, die nicht abgefangen wird, führt zur Beendigung des Programms.

.. literalinclude:: exceptions.py
   :language: python
   :linenos:
   :lines: 4-

Zeile 2
    Wenn während der Ausführung der Anweisungen im ``try``-Block ein
    ``IOError`` oder ``EmptyFileError`` auftritt, wird der zugehörige
    ``except``-Block ausgeführt.
Zeile 3
    Hier könnte ein ``IOError`` ausgelöst werden.
Zeile 7
    Hier löst ihr den ``EmptyFileError`` aus.
Zeile 12
    Die ``else``-Klausel ist optional; sie wird ausgeführt, wenn im
    ``try``-Block keine Ausnahme auftritt.

    .. note::
       In diesem Beispiel hätte stattdessen auch ``continue``-Anweisungen in
       den ``except``-Blöcken verwendet werden können.
      
Zeile 14
    Die ``finally``-Klausel ist optional; sie wird am Ende des Blocks
    ausgeführt, unabhängig davon, ob eine Ausnahme ausgelöst wurde oder nicht.
