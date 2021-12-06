tox
===

`tox <https://tox.readthedocs.io/>`_ ist ein Werkzeug zur Automatisierung der
Verwaltung von Virtualenv-Umgebungen und zum Testen mit mehreren
Interpreterkonfigurationen.

.. seealso::
   * `tox plugins <https://tox.readthedocs.io/en/latest/plugins.html>`_

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

#. Ausführen

   Mit ``bin/tox`` werden dann die folgenden Schritte durchlaufen:

   #. Optional erstellen eines Python-Package mit

      .. code-block:: console

           $ pipenv run python setup.py sdist

   #. Erstellen der in ``envlist`` angegebenen Umgebungen

      In jeder dieser Umgebungen werden dann

      #. die Abhängigkeiten und Pakete installiert
      #. die Befehle aus ``commands`` ausgeführt

   #. Erstellen eines Reports mit den Ergebnissen aus jeder der Umgebungen,
      :abbr:`z.B. (zum Beispiel)`:

      .. code-block:: text

           ____________________ summary ____________________
           py27: commands succeeded
           ERROR:   py36: commands failed

   .. seealso::

      * `Beispiele <https://tox.readthedocs.io/en/latest/examples.html>`_

GitHub-Actions
--------------

Wenn euer Projekt auf `GitHub <https://github.com/>`_ gehostet ist, könnt ihr
GitHub-Actions verwenden um automatisiert eure Tests in verschiedenen Umgebungen
ausführen zu können. Dabei sind eine ganze Reihe von Umgebungen für die
GitHub-Actions verfügbar: `github.com/actions/virtual-environments
<https://github.com/actions/virtual-environments/#readme>`_.

#. Um eine GitHub-Action in eurem Projekt zu erstellen, klickt auf
   :menuselection:`Actions --> set up a workflow yourself`. Dies erstellt
   üblicherweise eine Datei :file:`.github/workflows/main.yml`.
#. Gebt dieser Datei einen aussagekräftigeren Namen. Wir verwenden hierfür
   üblicherweise :file:`ci.yml`, wobei ``ci`` für *Continuous Integration*,
   (Englisch: `Kontinuierliche Integration
   <https://de.wikipedia.org/wiki/Kontinuierliche_Integration>`_) steht.
#.  Die vorausgefüllte YAML-Datei ist für unsere Zwecke wenig hilfreich. Ihr
    könnt den Text ersetzen, :abbr:`z.B. (zum Beispiel)` mit:

   .. code-block:: yaml

    name: CI

    on:
      push:
        branches: ["main"]
      pull_request:
        branches: ["main"]
      workflow_dispatch:

    jobs:
      tests:
        name: "Python ${{ matrix.python-version }}"
        runs-on: "ubuntu-latest"
        env:
          USING_COVERAGE: '3.6,3.8'

        strategy:
          matrix:
            python-version: ["3.6", "3.7", "3.8"]

        steps:
          - uses: "actions/checkout@v2"
          - uses: "actions/setup-python@v2"
            with:
              python-version: "${{ matrix.python-version }}"
          - name: "Install dependencies"
            run: |
              set -xe
              python -VV
