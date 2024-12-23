Kontrollflüsse
==============

Python verfügt über eine ganze Reihe von Strukturen zur Kontrolle der
Code-Ausführung und des Programmablaufs, einschließlich gängiger Verzweigungen
und Schleifen:

:doc:`boolean`
    überprüfen Werte und Identität und ermöglichen Verknüpfungen zwischen
    beiden.
:doc:`conditional`
    führen den Codeblock nach der ersten wahren Bedingung einer ``if``- oder
    ``elif``-Anweisung aus; wenn keine der Bedingungen wahr ist, wird der
    Codeblock nach dem ``else`` ausgeführt.
:doc:`loops`
    Während ``while``-Schleifen so lange ausgeführt werden, wie die Bedingung
    wahr ist, iterieren ``for``-Schleifen über
    :doc:`../types/sequences-sets/lists`, :doc:`../types/sequences-sets/tuples`
    und :doc:`../types/sequences-sets/sets`.
:doc:`exceptions`
    behandeln meist Fehler, die während der Ausführung von Programmen passieren.
:doc:`with`
    regelt :abbr:`u.a. (unter anderem)` den Zugriff auf Dateien, das Locking von
    Threads und die Unterdrückung von :doc:`exceptions`.

.. toctree::
   :titlesonly:
   :hidden:

   boolean
   conditional
   loops
   exceptions
   with
