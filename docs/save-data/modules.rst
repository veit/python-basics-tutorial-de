Module für Dateien
==================

.. _builtin-file-modules:

Eingebaute Module
-----------------

Die Python-Standardbibliothek enthält eine Reihe eingebauter Module, mit denen
ihr Dateien managen könnt:

.. _file-modules:

+-----------------------------------+-------------------------------------------------------------------------------+
| Modul                             | Beschreibung                                                                  |
+===================================+===============================================================================+
| :py:mod:`os.path`                 | führt allgemeine Pfadnamenmanipulationen durch                                |
+-----------------------------------+-------------------------------------------------------------------------------+
| :py:mod:`pathlib`                 | manipuliert Pfadnamen                                                         |
+-----------------------------------+-------------------------------------------------------------------------------+
| :py:mod:`fileinput`               | iteriert über mehrere Eingabedateien                                          |
+-----------------------------------+-------------------------------------------------------------------------------+
| :py:mod:`filecmp`                 | vergleicht Dateien und Verzeichnisse                                          |
+-----------------------------------+-------------------------------------------------------------------------------+
| :py:mod:`tempfile`                | erzeugt temporäre Dateien und Verzeichnisse                                   |
+-----------------------------------+-------------------------------------------------------------------------------+
| :py:mod:`glob`,                   | verwenden UNIX-ähnlicher Pfad- und Dateinamensmuster                          |
| :py:mod:`fnmatch`                 |                                                                               |
+-----------------------------------+-------------------------------------------------------------------------------+
| :py:mod:`linecache`               | greift zufällig auf Textzeilen zu                                             |
+-----------------------------------+-------------------------------------------------------------------------------+
| :py:mod:`shutil`                  | führt Dateioperationen auf höherer Ebene aus                                  |
+-----------------------------------+-------------------------------------------------------------------------------+
| :py:mod:`mimetypes`               | Zuordnung von Dateinamen zu MIME-Typen                                        |
+-----------------------------------+-------------------------------------------------------------------------------+
| :py:mod:`pickle`,                 | aktivieren von Python-Objektserialisierung und -persistenz, :abbr:`s.a. (siehe|
| :py:mod:`shelve`                  | auch)` :doc:`../save-data/pickle`                                             |
+-----------------------------------+-------------------------------------------------------------------------------+
| :py:mod:`csv`                     | liest und schreibt CSV-Dateien                                                |
+-----------------------------------+-------------------------------------------------------------------------------+
| :py:mod:`json`                    | JSON-Kodierer und -Dekodierer                                                 |
+-----------------------------------+-------------------------------------------------------------------------------+
| :py:mod:`sqlite3`                 | bietet eine DB-API 2.0-Schnittstelle für SQLite-Datenbanken, :abbr:`s.a.      |
|                                   | (siehe auch)` :doc:`../save-data/sqlite/index`                                |
+-----------------------------------+-------------------------------------------------------------------------------+
| :py:mod:`xml`,                    | liest und schreibt XML-Dateien, :abbr:`s.a. (siehe auch)`                     |
| :py:mod:`xml.parsers.expat`,      | :doc:`../save-data/xml`                                                       |
| :py:mod:`xml.dom`,                |                                                                               |
| :py:mod:`xml.sax`,                |                                                                               |
| :py:mod:`xml.etree.ElementTree`   |                                                                               |
+-----------------------------------+-------------------------------------------------------------------------------+
| :py:mod:`html.parser`,            | Parsen von HTML und XHTML                                                     |
| :py:mod:`html.entities`           |                                                                               |
+-----------------------------------+-------------------------------------------------------------------------------+
| :py:mod:`configparser`            | liest und schreibt Windows-ähnliche Konfigurationsdateien (``.ini``)          |
+-----------------------------------+-------------------------------------------------------------------------------+
| :py:mod:`base64`,                 | Kodierung/Dekodierung von Dateien oder Streams                                |
| :py:mod:`binascii`,               |                                                                               |
| :py:mod:`quopri`,                 |                                                                               |
| :py:mod:`uu`                      |                                                                               |
+-----------------------------------+-------------------------------------------------------------------------------+
| :py:mod:`struct`                  | konvertiert zwischen Python-Werten und C-Strukturen, die als                  |
|                                   | als Python-Bytes-Objekte dargestellt werden.                                  |
+-----------------------------------+-------------------------------------------------------------------------------+
| :py:mod:`zlib`,                   | für das Arbeiten mit Archivdateien und Komprimierungen                        |
| :py:mod:`gzip`,                   |                                                                               |
| :py:mod:`bz2`,                    |                                                                               |
| :py:mod:`zipfile`,                |                                                                               |
| :py:mod:`tarfile`                 |                                                                               |
+-----------------------------------+-------------------------------------------------------------------------------+

.. _end-file-modules:

.. _pandas-io-tools:

pandas IO tools
---------------

* :doc:`Python4DataScience:data-processing/pandas-io`

  Beispiele für die Serialisierungsformate:

  * :doc:`CSV
    <Python4DataScience:data-processing/serialisation-formats/csv/example>`
  * :doc:`JSON
    <Python4DataScience:data-processing/serialisation-formats/json/example>`
  * :doc:`Excel
    <Python4DataScience:data-processing/serialisation-formats/excel>`
  * :doc:`XML/HTML
    <Python4DataScience:data-processing/serialisation-formats/xml-html/index>`
  * :doc:`YAML
    <Python4DataScience:data-processing/serialisation-formats/yaml/example>`
  * :doc:`TOML
    <Python4DataScience:data-processing/serialisation-formats/toml/example>`
  * :doc:`Pickle
    <Python4DataScience:data-processing/serialisation-formats/pickle/pickle-examples>`

Checks
------

* Welche Anwendungsfälle könnt ihr euch vorstellen, in denen das
  :mod:`python3:struct`-Modul für das Lesen oder Schreiben von Binärdaten
  nützlich wäre?

* Warum könnte :doc:`pickle <python3:library/pickle>` für die folgenden
  Anwendungsfälle geeignet sein oder auch nicht:

  #. Speichern einiger Zustandsvariablen von einem Durchlauf zum nächsten
  #. Aufbewahren von Auswertungsergebnissen
  #. Speichern von Benutzernamen und Passwörtern
  #. Speichern eines großen Wörterbuchs mit englischen Begriffen
