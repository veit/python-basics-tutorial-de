Sequenzdiagramm
===============

.. uml::

    Browser -> Server: Authentifizierungsanfrage
    Server --> Browser: Authentifizierungsantwort

    Browser -> Server: Eine andere Authentifizierungsanfrage
    Browser <-- Server: Eine andere Authentifizierungsantwort

.. code-block:: rest

   .. uml::

       Browser -> Server: Authentifizierungsanfrage
       Server --> Browser: Authentifizierungsantwort

       Browser -> Server: Eine andere Authentifizierungsanfrage
       Browser <-- Server: Eine andere Authentifizierungsantwort

``->``
    wird verwendet, um eine Nachricht zwischen zwei Akteuren zu zeichnen. Die
    Akteure müssen nicht explizit deklariert werden.
``-->``
     wird verwendet, um eine gepunktete Linie zu zeichnen.
``<- und <--``
    verändert die Zeichnung nicht, kann aber die Lesbarkeit erhöhen.
    
    .. note::
       Dies gilt nur für Sequenzdiagramme. In anderen Diagrammen können andere
       Regeln gelten.
