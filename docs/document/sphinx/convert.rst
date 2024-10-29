Konvertieren
============

Andere Dateiformate können mit pandoc in :doc:`rest` konvertiert werden.

Installation von pandoc
-----------------------

`Pandoc <https://pandoc.org/installing.html>`_ ist ein leistungsfähiges
Dienstprogramm zur Dokumentumwandlung. Wir verwenden es für einfache
Konvertierungen, aber es ist zu viel mehr in der Lage.

Installation
------------

Pandoc könnt ihr für die verschiedenen Plattformen installieren:

.. tab:: Debian/Ubuntu

   .. code-block:: console

      $  sudo apt install pandoc

.. tab:: macOS

   .. code-block:: console

      $  brew install pandoc

.. tab:: Windows

   .. code-block:: ps1

      $  choco install pandoc

.. seealso::
   * `Installing pandoc <https://pandoc.org/installing.html>`_

Konvertieren
------------

Navigiert im Terminal zu dem Verzeichnis, das die zu konvertierenden Dokumente
enthält. Gebt dann für jede Datei, die ihr konvertieren möchtet, den  Befehl
:samp:`pandoc -s --toc -f {INPUT_FORMAT} -t rst {MYDOC}.{SUFFIX}` ein:

``-s``
    erzeugt ein eigenständiges Dokument
``--toc``
    erstellt ein Inhaltsverzeichnis (optional)
``-t``
    erzeugt eine reStructuredText-Ausgabe
``-f``
    teilt pandoc das Eingabeformat mit. Einen Überblick über die verfügbaren
    Eingabeformate erhaltet ihr in `General options
    <https://pandoc.org/MANUAL.html#general-options>`_.

Korrigieren des konvertierten Dokuments
---------------------------------------

Wie umfangreich die Korrektur für das konvertierte Dokument ausfällt, hängt
davon ab, aus welchem Dateiformat ihr konvertiert. Hier sind einige Dinge, auf
die ihr achten solltet:

* Mehrzeilige Titel müssen in einzeilige konvertiert werden
* Eigenständige ``**``-Zeichen
* :samp:`***FETT***` sollte :samp:`**FETT**` sein
* Fehlerhafte Tabellen
