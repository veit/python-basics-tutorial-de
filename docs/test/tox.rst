tox
===

`tox <https://tox.readthedocs.io/>`_ ist ein Werkzeug zur Automatisierung der
Verwaltung von Virtualenv-Umgebungen und zum Testen mit mehreren
Interpreterkonfigurationen.

#. Installation

   .. tabs::

      .. tab:: Linux/MacOS

         .. code-block:: console

            $ bin/python -m pip install tox

      .. tab:: Windows

         .. code-block:: ps1con

            C:> Scripts\python -m pip install tox

#. Konfiguration

   Mit tox könnt ihr komplexe Multiparameter-Testmatrizen über eine einfache
   Konfigurationsdatei im `INI <https://en.wikipedia.org/wiki/INI_file>`_-Stil,
   konfigurieren,  :abbr:`z.B. (zum Beispiel)`:

   .. literalinclude:: tox.ini
      :language: ini
      :lineno-start: 1
