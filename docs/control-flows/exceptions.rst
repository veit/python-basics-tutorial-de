Ausnahmebehandlung (``exception``)
==================================

Ausnahmen oder Fehler können mit Hilfe der zusammengesetzten Anweisung
``try``-``except``-``else``-``finally`` abgefangen und behandelt werden. Diese
Anweisung kann auch Ausnahmen abfangen und behandeln, die ihr selbst definieren
und auslösen könnt. Jede Ausnahme, die nicht abgefangen wird, führt zur
Beendigung des Programms. Die folgenden Beispiele zeigen die grundlegende
Behandlung von Ausnahmen.

.. literalinclude:: exceptions.py
   :linenos:

Zeile 1
    Hier definiert ihr euren eigenen Ausnahmetyp, der vom Basistyp
    ``Exception`` erbt.
Zeile 5
    Wenn während der Ausführung der Anweisungen im ``try``-Block ein
    ``IOError`` oder ``EmptyFileError`` auftritt, wird der zugehörige
    ``except``-Block ausgeführt.
Zeile 7
    Hier könnte ein ``IOError`` ausgelöst werden.
Zeile 10
    Hier löst ihr den ``EmptyFileError`` aus.
Zeile 15
    Die ``else``-Klausel ist optional; sie wird ausgeführt, wenn im
    ``try``-Block keine Ausnahme auftritt.

    .. note::
       In diesem Beispiel hätte stattdessen auch ``continue``-Anweisungen in
       den ``except``-Blöcken verwendet werden können.
      
Zeile 17
    Die ``finally``-Klausel ist optional; sie wird am Ende des Blocks
    ausgeführt, unabhängig davon, ob eine Ausnahme ausgelöst wurde oder nicht.
