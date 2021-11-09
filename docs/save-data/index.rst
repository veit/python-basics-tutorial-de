Daten speichern und abrufen
===========================

Die Python-Datenbank-API
------------------------

Die Python Database :abbr:`API (Application Programming Interface)` definiert
eine Standardschnittstelle für Python-Datenbankzugriffsmodule. Sie ist in `PEP
249 <https://www.python.org/dev/peps/pep-0249/>`_ definiert und wird häufig
verwendet, :abbr:`z.B. (zum Beispiel)` von :doc:`sqlite <sqlite>`, :doc:`psycopg
<psycopg>`, and `mysql-python 
<https://sourceforge.net/projects/mysql-python/>`_.

SQLAlchemy
----------

:doc:`jupyter-tutorial:data-processing/postgresql/sqlalchemy` ist ein weit
verbreitetes Datenbank-Toolkit. Es bietet nicht nur als ein :abbr:`ORM (Object
Relational Mapper)`, sondern bietet auch eine allgemeine API zum Schreiben von
datenbankagnostischem Code ohne SQL.
:doc:`jupyter-tutorial:data-processing/postgresql/alembic` basiert auf
SQLAlchemy und dient als Datenbankmigrationswerkzeug.


NoSQL-Datenbanken
-----------------

Es gibt Daten, die sich nur schwer in ein relationales Datenmodell übertragen
lassen. Dann solltet ihr zumindest einen Blick auf
:doc:`jupyter-tutorial:data-processing/nosql/index` werfen.

.. toctree::
   :titlesonly:
   :hidden:

   pickle
   sqlite
   create-db
   create-data
   create-data-from-csv
   query-data
   update-data
   delete-data
   normalise
   query-normalised
   psycopg
