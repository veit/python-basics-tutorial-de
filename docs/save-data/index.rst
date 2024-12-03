Daten speichern und abrufen
===========================

Um Daten persistent zu speichern, kann ein Prozess verwendet werden, der sich
*Serialisierung* oder *Marshalling* nennt. In ihm werden Datenstrukturen in eine
lineare Form umgewandelt und gespeichert. Der umgekehrte Vorgang wird dann
*Deserialisierung* oder *Unmarshalling* genannt. Python bietet in der
Standardbibliothek mehrere Module, mit denen ihr Objekte serialisieren und
deserialisieren könnt:

das :doc:`marshal <python3:library/marshal>`-Modul
    wird im Wesentlichen intern von Python genutzt und sollte nicht verwendet
    werden um Daten abwärtskompatibel zu speichern.
das :doc:`pickle <pickle>`-Modul
    könnt ihr verwenden, wenn ihr weder ein lesbares Format noch
    Interoperabilität benötigt.
das :doc:`json <python3:library/json>`-Modul
    könnt ihr verwenden um Daten für verschiedene Sprachen in einer lesbaren
    Form auszutauschen.
das :doc:`xml <xml>`-Modul
    könnt ihr ebenfalls verwenden um Daten in verschiedene Sprachen in einer
    lesbaren Form auszutauschen.

.. tip::
   `cusy Seminar: Daten lesen, schreiben und bereitstellen mit Python
   <https://cusy.io/de/unsere-schulungsangebote/daten-lesen-schreiben-und-bereitstellen-mit-python>`_

Die Python-Datenbank-API
------------------------

Die Python Database :abbr:`API (Application Programming Interface)` definiert
eine Standardschnittstelle für Python-Datenbank-Zugriffsmodule. Sie ist in
:pep:`249` definiert und wird häufig verwendet, :abbr:`z.B. (zum Beispiel)` von
:doc:`sqlite <sqlite/index>`, :doc:`psycopg <psycopg>`, and `mysql-python
<https://sourceforge.net/projects/mysql-python/>`_.

SQLAlchemy
----------

:doc:`Python4DataScience:data-processing/postgresql/sqlalchemy` ist ein weit
verbreitetes Datenbank-Toolkit. Es bietet nicht nur als ein :abbr:`ORM (Object
Relational Mapper)`, sondern bietet auch eine allgemeine API zum Schreiben von
datenbankagnostischem Code ohne SQL.
:doc:`Python4DataScience:data-processing/postgresql/alembic` basiert auf
SQLAlchemy und dient als Datenbank-Migrationswerkzeug.


NoSQL-Datenbanken
-----------------

Es gibt Daten, die sich nur schwer in ein relationales Datenmodell übertragen
lassen. Dann solltet ihr zumindest einen Blick auf
:doc:`Python4DataScience:data-processing/nosql/index` werfen.

.. toctree::
   :titlesonly:
   :hidden:

   filesystem
   pickle
   xml
   sqlite/index
   psycopg
