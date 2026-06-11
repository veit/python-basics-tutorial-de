Logging
=======

Das `logging
<https://docs.python.org/3/library/logging.html#module-logging>`_-Modul ist Teil
der Python-Standardbibliothek. Es ist beschrieben in :pep:`0282`. Eine erste
Einführung in das Modul erhaltet ihr in `Basic Logging Tutorial
<https://docs.python.org/3/howto/logging.html#logging-basic-tutorial>`_.

Logging erfüllt üblicherweise zwei verschiedene Zwecke:

* Diagnose:

  * Ihr könnt euch den Kontext von bestimmten Ereignissen anzeigen lassen.
  * Tools wie `Sentry <https://sentry.io/welcome/>`_ gruppieren
    zusammengehörende Ereignisse und erleichtern die Benutzeridentifikation
    :abbr:`etc. (et cetera)`, sodass die Fehlerursache schneller gefunden werden
    kann.

* Monitoring:

  * Das Logging zeichnet Ereignisse für benutzerdefinierten Heuristiken auf,
    :abbr:`z.B. (zum Beispiel)` für Geschäftsanalysen. Diese Aufzeichnungen
    können für Berichte oder Optimierungen der Geschäftsziele verwendet und
    :abbr:`ggf. (gegebenenfalls)` visualisiert werden.

Welche Vorteile bietet ``logging`` nun gegenüber ``print``?

* Die Logdatei enthält alle verfügbaren Diagnoseinformationen wie Dateiname,
  Pfad, Funktion und Zeilennummer
* Alle Ereignisse sind über den Root-Logger automatisch verfügbar, sofern sie
  nicht explizit herausgefiltert werden.
* Logging kann wahlweise durch eine der folgenden beiden Methoden
  stummgeschaltet werden: `logging.Logger.setLevel()
  <https://docs.python.org/3/library/logging.html#logging.Logger.setLevel>`_
  oder `logging.disabled
  <https://docs.python.org/3/library/logging.html#logging.disable>`_.


.. note::
   Auch bei agentischer Software-Entwicklung nutzen wir Logging um uns einen
   detaillierteren Einblick un Fehler zu geben:

   .. code-block:: md
      :caption: AGENTS.md

      # Logging
      - Use logging to provide insight into failures. Don’t use print for debugging. Don’t use logging to hide stack traces if you are going to fail anyway.
      - Don't hide exceptions. Let them propagate up to the caller. If you need to catch an exception, log it and re-raise it.

   .. seealso::
      * :ref:`agentic-software-engineering:logging`

.. seealso::

   * `loguru <https://github.com/Delgan/loguru>`_ macht das Protokollieren fast
     so einfach wie die Verwendung von ``print``-Anweisungen.
   * `structlog <https://www.structlog.org/en/stable/>`_ fügt euren
     Log-Einträgen Struktur hinzu.

.. toctree::
    :hidden:
    :titlesonly:
    :maxdepth: 0

    examples.ipynb
