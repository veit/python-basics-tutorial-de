Aktivitätsdiagramm
==================

``(*)``
    Start- und Endknoten eines Aktivitätsdiagramms.

    ``(*top)``
        In einigen Fällen kann dies verwendet werden um den Startpunkt an den
        Anfang eines Diagramms zu verschieben.

``-->``
    definiert eine Aktivität

    ``-down->``
        Pfeil nach unten (Standardwert)
    ``-right-> or ->``
        Pfeil nach rechts
    ``-left->``
        Pfeil nach links
    ``-up->``
        Pfeil nach oben

``if``, ``then``, ``else``
    Schlüsselworte für die Definition von Verzweigungen.

    Beispiel:

    .. code-block:: rest

       .. uml::

           (*) --> "Initialisierung"
           if "ein Test" then
           -->[wahr] "Eine Aktivität"
           --> "Eine andere Aktivität"
           -right-> (*)
           else
           ->[falsch] "Etwas anderes"
           -->[Ende des Prozesses] (*)
           endif

    .. image:: activity-diagram.svg

``=== code ===``
    Synchronisationsbalken.

    Beispiel:

    .. code-block:: rest

       .. uml::
        
           (*) --> ===B1===
           --> "Erste parallele Aktivität"
           --> ===B2===
           ===B1=== --> "Parallele Aktivität 2"
           --> ===B2===
           --> (*)

    .. image:: activity-sync.svg
