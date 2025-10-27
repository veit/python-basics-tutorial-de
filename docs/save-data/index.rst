Daten speichern und abrufen
===========================

Ihr könnt eure Daten persistent in :doc:`files-directories` speichern. In der
Python-Standardbibliothek gibt es darüberhinaus mehrere Module, Daten in
eine lineare Form umzuwandeln. Dieser Prozess wird *Serialisierung* oder
*Marshalling* genannt. Der umgekehrte Vorgang heißt dann *Deserialisierung* oder
*Unmarshalling*. Und wenn die :ref:`eingebauten Module <builtin-file-modules>`
nicht ausreichen sollten, könnt ihr auch die :ref:`pandas-io-tools` verwenden.

.. tip::
   `cusy Seminar: Daten lesen, schreiben und bereitstellen mit Python
   <https://cusy.io/de/our-training-courses/read-write-and-provide-data-with-python.html>`_

.. toctree::
   :titlesonly:
   :hidden:

   files-directories
   modules
   pickle
   xml

Die Python-Datenbank-API
------------------------

Die Python Database :abbr:`API (Application Programming Interface)` definiert
eine Standardschnittstelle für Python-Datenbank-Zugriffsmodule. Sie ist in
:pep:`249` definiert und wird häufig verwendet, :abbr:`z.B. (zum Beispiel)` von
:doc:`sqlite <sqlite/index>`, :doc:`psycopg <psycopg>`, and `mysql-python
<https://sourceforge.net/projects/mysql-python/>`_.

.. toctree::
   :titlesonly:
   :hidden:

   sqlite/index
   psycopg

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
