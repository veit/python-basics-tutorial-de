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
    z.B. für Geschäftsanalysen. Diese Aufzeichnungen können für Berichte oder
    Optimierungen der Geschäftsziele verwendet und ggf. visualisiert werden.

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
