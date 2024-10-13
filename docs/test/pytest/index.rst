pytest
======

:doc:`pytest <pytest:index>` ist eine Alternative zu Pythons
:doc:`../unittest`-Modul, die das Testen noch weiter vereinfacht.

Merkmale
--------

* Ausführlichere Informationen über fehlgeschlagene ``assert``-Anweisungen
* Automatische Erkennung von Testmodulen und -Funktionen
* Modulare Fixtures für die Verwaltung von kleinen oder parametrisierten,
  langlebigen Testressourcen
* Kann auch Unittests ohne Voreinstellungen ausführen
* Umfangreiche Plugin-Architektur, mit über 800 externen Plugins

Installation
------------

Ihr könnt pytest in :ref:`virtuellen Umgebungen <venv>` installieren mit:

.. tab:: Linux/macOS

   .. code-block:: console

      $ python -m pip install pytest
      Collecting pytest
      …
      Successfully installed attrs-21.2.0 iniconfig-1.1.1 pluggy-1.0.0 py-1.10.0 pytest-6.2.5 toml-0.10.2

.. tab:: Windows

   .. code-block:: ps1con

      C:> python -m pip install pytest
      Collecting pytest
      …
      Successfully installed attrs-21.2.0 iniconfig-1.1.1 pluggy-1.0.0 py-1.10.0 pytest-6.2.5 toml-0.10.2

.. toctree::
   :titlesonly:
   :hidden:

   examples
   functions
   testsuite
   fixtures
   builtin-fixtures
   params
   markers
   plugins
   config
   debug
