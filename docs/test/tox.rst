tox
===

`tox <https://tox.readthedocs.io/>`_ ist ein Werkzeug zur Automatisierung der
Verwaltung von Virtualenv-Umgebungen und zum Testen mit mehreren
Interpreterkonfigurationen.

#. Installation

   .. code-block:: console

      $ python -m pip install tox

#. Konfiguration

   Mit tox könnt ihr komplexe Multiparameter-Testmatrizen über eine einfache
   Konfigurationsdatei im `INI <https://en.wikipedia.org/wiki/INI_file>`_-Stil,
   konfigurieren,  :abbr:`z.B. (zum Beispiel)`:

   .. literalinclude:: tox.ini
      :language: ini
      :lineno-start: 1
