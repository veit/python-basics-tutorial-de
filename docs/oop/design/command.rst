Kommando-Entwurfsmuster
=======================

`Kommando <https://de.m.wikipedia.org/wiki/Kommando_(Entwurfsmuster)>`_ ist ein
weiteres Entwurfsmuster, das durch die Verwendung von Funktionen, die als
Argumente übergeben werden, vereinfacht werden kann:

.. uml::

    title UML-Klassendiagramm für das Kommando-Entwurfsmuster

    together {
        abstract class  Caller
        abstract class  Command {
            {method}    execute()
        }
    }

    together {
        abstract class  Client
        abstract class  Receiver {
            {method}    action()
        }
        class           ConcreteCommand {
            state
            {method}    execute()
        }
    }

    Caller *-> Command
    Client -> Receiver
    Client -> ConcreteCommand
    Receiver <- ConcreteCommand
    ConcreteCommand -u-|> Command

Das Ziel des Kommando-Entwurfsmusters ist es, ein Objekt, das eine Operation
aufruft vom :obj:`Receiver`-Objekt zu entkoppeln, das die Operation
implementiert. Im Beispiel aus dem `Entwurfsmuster
<https://de.wikipedia.org/wiki/Entwurfsmuster_(Buch)>`_-Buch ist jedes
:obj:`Caller`-Objekt ein Menüpunkt in einer grafischen Anwendung, und die
:obj:`Receiver`-Objekte sind das zu bearbeitende Dokument oder die Anwendung
selbst. Hierzu wird ein :obj:`Command`-Objekt zwischen die beiden gesetzt, das
eine Schnittstelle mit einer einzigen Methode implementiert, die eine Methode im
:class:`Receiver` aufruft, um die gewünschte Operation durchzuführen. Auf diese
Weise muss das :obj:`Caller`-Objekt das Interface des :class:`Receiver` nicht
kennen, und verschiedene Empfänger können durch verschiedene
:class:`Command`-Unterklassen angepasst werden. :class:`Caller` wird mit einem
:class:`ConcreteCommand`-Befehl konfiguriert und ruft dessen
:meth:`execute`-Methode auf, um ihn auszuführen.

    Command sind ein objektorientierter Ersatz für Callbacks. [#]_

Die Frage ist nun, ob wir in Python wirklich einen solchen objektorientierten
Ersatz für Callbacks brauchen? Können wir stattdessen nicht dem :obj:`Caller`
einfach eine Funktion geben? Anstatt also :func:`Command.execute` aufzurufen,
könnte der :class:`Caller` einfach :func:`command()` aufrufen. Dabei kann
:class:`Command` eine Klasse sein, die :func:`__call__()` implementiert und
Instanzen von :class:`Command` wären dann :abbr:`sog. (sogenannte)` *callables*,
die jeweils eine Liste von Funktionen für zukünftige Aufrufe enthalten,
:abbr:`z.B. (zum Beispiel)`:

.. literalinclude:: caller.py
   :language: python
   :linenos:

Zeilen 4–5
    erstellt eine Liste aus den Befehlsargumenten und stellt sicher, dass sie
    iterierbar ist
Zeile 7–8
    erstellt eine lokale Kopie der Befehlsreferenzen in jeder
    :class:`MacroCommand`-Instanz. Wenn eine Instanz von :class:`MacroCommand`
    aufgerufen wird, wird jeder Befehl in ``self.commands`` nacheinander
    aufgerufen.

----

.. [#] `Entwurfsmuster (Buch)
       <https://de.wikipedia.org/wiki/Entwurfsmuster_(Buch)>`_
