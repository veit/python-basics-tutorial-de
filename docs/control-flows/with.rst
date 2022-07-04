Kontextmanagement mit ``with`` 
==============================

Eine rationellere Art, das Muster ``try-except-finally`` zu kapseln, ist die
Verwendung des Schlüsselworts ``with`` und eines Kontextmanagers. Python
definiert Kontextmanager für Dinge wie den Zugriff auf :doc:`/types/files` und
eigene Kontextmanager. Ein Vorteil von Kontextmanagern ist, dass sie
standardmäßige Bereinigungsaktionen definieren können, die immer ausgeführt
werden, unabhängig davon, ob eine Ausnahme auftritt oder nicht.

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
