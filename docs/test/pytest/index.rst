pytest
======

:doc:`pytest <pytest:index>` ist eine Alternative zu Pythons
:doc:`../unittest`-Modul, die das Testen noch weiter vereinfacht.

* pytest erkennt Tests automatisch anhand von Dateinamen und Funktionen, die mit
  ``test_`` beginnen, während bei unittest Testklassen und -Methoden von
  :class:`unittest.TestCase` abgeleitet werden. Dies führt zu einer einfacheren,
  besser lesbaren Syntax mit weniger Boilerplate-Code.
* unittest bietet eine Reihe von Assertion-Methoden (z. B. :func:`assertEqual`,
  :func:`assertTrue`, :func:`assertRaises`). Mit pytest lassen sich dieselben
  Annahmen definieren, allerdings mit der Standard-:func:`assert`-Anweisung von
  Python. Dies führt häufig zu aussagekräftigeren Fehlermeldungen und einer
  besseren Introspektion.
* unittest stellt für Fixtures nur :func:`setUp`- und :func:`tearDown`-Methoden
  bereit. In pytest hingegen werden :doc:`Fixtures <fixtures>` als Funktionen
  definiert, was die Wiederverwendbarkeit fördert und die Verwaltung von
  Testabhängigkeiten vereinfacht.
* In unittest sind parametrisierte Tests zwar möglich, erfordern jedoch
  zusätzliche Aufwand. pytest enthält hingegen den :doc:`Dekorator
  <../../functions/decorators>` ``@pytest.mark.parametrize``, mit dem mühelos
  Testfunktionen mit unterschiedlichen Eingaben und erwarteten Ergebnissen
  ausgeführt werden können.
* pytest verfügt über ein umfangreiches Ökosystem mit über 800 :doc:`plugins`
  für fortgeschrittene Testanforderungen; unittest ist in seiner Erweiterbarkeit
  eingeschränkter.

Installation
------------

Ihr könnt pytest in :ref:`virtuellen Umgebungen <venv>` installieren mit:

.. tab:: Linux/macOS

   .. code-block:: console

      $ python -m pip install pytest
      Collecting pytest
      ...
      Successfully installed attrs-21.2.0 iniconfig-1.1.1 pluggy-1.0.0 py-1.10.0 pytest-6.2.5 toml-0.10.2

.. tab:: Windows

   .. code-block:: ps1con

      C:> python -m pip install pytest
      Collecting pytest
      ...
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
   coverage
