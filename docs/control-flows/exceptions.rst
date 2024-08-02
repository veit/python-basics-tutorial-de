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

.. note::
   Die Art und Weise, wie Python Fehlersituationen im Allgemeinen behandelt,
   unterscheidet sich von manch anderen Sprachen, :abbr:`z.B. (zum Beispiel)`
   Java. Diese Sprachen prüfen mögliche Fehler so weit wie möglich, bevor sie
   auftreten, da die Behandlung von Exceptions nach ihrem Auftreten kostspielig
   ist. Dies wird manchmal als :abbr:`LBYL (Look before you leap, Erst schauen,
   dann springen)`-Ansatz bezeichnet.

   Bei Python hingegen verlässt man sich eher auf Exceptions, um Fehler zu
   behandeln, nachdem sie aufgetreten sind. Obwohl dieses Vertrauen riskant
   erscheinen mag, ist der Code weniger schwerfällig und leichter zu lesen, wenn
   Exceptions richtig eingesetzt werden, und Fehler werden nur dann behandelt,
   wenn sie auftreten. Diese pythonische Herangehensweise zur Behandlung von
   Fehlern wird oft als :abbr:`EAFP (easier to ask forgiveness than permission,
   engl.: leichter um Vergebung zu bitten als um Erlaubnis)` beschrieben.

Es ist möglich, verschiedene Arten von Ausnahmen zu erzeugen, um die
tatsächliche Ursache des gemeldeten Fehlers oder außergewöhnlichen Umstandes zu
reflektieren. Eine Übersicht über die Klassenhierarchie eingebauter Exceptions
erhaltet ihr unter `Exception hierarchy
<https://docs.python.org/3/library/exceptions.html#exception-hierarchy>`_ in der
Python-Dokumentation. Jeder Ausnahmetyp ist eine Python-Klasse, die von ihrem
übergeordneten Exception-Typ erbt. So ist :abbr:`z.B. (zum Beispiel)` ein
``ZeroDivisionError`` durch Vererbung auch ein ``ArithmeticError``, eine
``Exception`` und auch eine ``BaseException``. Diese Hierarchie ist gewollt: Die
meisten Ausnahmen erben von ``Exception``, und es wird dringend empfohlen, dass
alle benutzerdefinierten Ausnahmen auch die Unterklasse von ``Exception``  und
nicht von ``BaseException`` bilden:

.. literalinclude:: exceptions.py
   :language: python
   :linenos:
   :lines: 1-2

Dies definiert ihr euren eigenen Ausnahmetyp, der vom Basistyp ``Exception``
erbt.

.. literalinclude:: exceptions.py
   :language: python
   :linenos:
   :lines: 5
   :lineno-start: 5

Eine Liste unterschiedlicher Datei-Arten wird definiert.

Schließlich werden Ausnahmen oder Fehler mit Hilfe der zusammengesetzten
Anweisung ``try``-``except``-``else``-``finally`` abgefangen und behandelt.
Jede Ausnahme, die nicht abgefangen wird, führt zur Beendigung des Programms.

.. literalinclude:: exceptions.py
   :language: python
   :linenos:
   :lines: 7-
   :lineno-start: 7

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

    .. note::
       In diesem Beispiel hätte stattdessen auch ``continue``-Anweisungen in
       den ``except``-Blöcken verwendet werden können.

Zeile 19
    Die ``finally``-Klausel ist optional; sie wird am Ende des Blocks
    ausgeführt, unabhängig davon, ob eine Ausnahme ausgelöst wurde oder nicht.
