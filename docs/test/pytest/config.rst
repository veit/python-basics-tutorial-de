Konfiguration
=============

Ihr solltet eine Konfigurationsdatei haben, entweder :file:`pytest.ini`, oder
einem ``pytest``-Abschnitt in :file:`tox.ini`, :file:`pyproject.toml` oder in
:file:`setup.cfg`.

Sie Konfigurationsdatei legt das oberste Verzeichnis fest, von dem aus
``pytest`` gestartet wird.

Die meisten meiner Projekte starten mit folgender Konfiguration:

.. code-block:: ini

   addopts =
       --strict-markers
       --strict-config
       -ra

.. seealso::
   * `Configuration
     <https://docs.pytest.org/en/latest/reference/customize.html>`_
   * `Configuration Options
     <https://docs.pytest.org/en/latest/reference/reference.html#configuration-options>`_
