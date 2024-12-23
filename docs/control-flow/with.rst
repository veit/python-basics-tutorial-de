Kontextmanagement mit ``with``
==============================

Eine rationellere Art, das Muster ``try``-``except``-``finally`` zu kapseln, ist
die Verwendung des Schlüsselworts ``with`` und eines Kontextmanagers. Python
definiert Kontextmanager für Dinge wie den Zugriff auf :doc:`/save-data/files`
und eigene Kontextmanager. Ein Vorteil von Kontextmanagern ist, dass sie
Bereinigungsaktionen definieren können, die immer ausgeführt werden, unabhängig
davon, ob eine Ausnahme auftritt oder nicht.

.. seealso::
   :doc:`python3:library/contextlib`

Öffnen und Schließen von Dateien
--------------------------------

Die folgende Auflistung zeigt das Öffnen und Lesen einer Datei unter Verwendung
von ``with`` und einem Kontextmanager.

.. literalinclude:: with.py
   :linenos:

Hier wird ein Kontextmanager eingerichtet, der die Funktion ``open`` und den
darauf folgenden Block umschließt. Die vordefinierte Aufräumaktion des
Kontextmanagers schließt die Datei, auch wenn eine Ausnahme auftritt. Solange
der Ausdruck in der ersten Zeile ausgeführt wird, ohne eine Ausnahme
auszulösen, wird die Datei immer geschlossen. Dieser Code ist äquivalent zu
diesem Code:

.. literalinclude:: with_alt.py
   :linenos:

.. seealso::
   * :doc:`../save-data/files`

Locking
-------

:class:`threading.Lock` kann mit ``try``-``finally`` verwendet werden:

.. code-block:: Python

   lock = threading.Lock()

   try:
       print("a Job")
       print("another Job")
   finally:
       lock.release()

Eleganter ist jedoch die Verwendung mit dem Kontextmanager:

.. code-block:: Python

   with lock:
      print("a Job")
      print("another Job")

.. seealso::
   `Sorgfältiges Threading mit Locks
   <https://www.python4data.science/de/latest/performance/threading-example.html#Sorgf%C3%A4ltiges-Threading-mit-Locks>`_

Exceptions unterdrücken
-----------------------

Kontextmanager können auch verwendet werden um die Ausgabe von :doc:`exceptions`
zu unterdrücken und die Ausführung fortzusetzen.

.. code-block:: Python

   try:
       os.remove("somefile.tmp")
   except FileNotFoundError:
       pass

Dies kann eleganter geschrieben werden mit:

.. code-block:: Python

   from contextlib import suppress

   with suppress(FileNotFoundError):
       os.remove("somefile.tmp")
