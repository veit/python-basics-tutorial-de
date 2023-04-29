tox
===

`tox <https://tox.readthedocs.io/>`_ ist ein Werkzeug zur Automatisierung der
Verwaltung von Virtualenv-Umgebungen und zum Testen mit mehreren
Interpreterkonfigurationen.

.. seealso::
   * `tox plugins <https://tox.readthedocs.io/en/latest/plugins.html>`_

#. Installation

   .. tab:: Linux/macOS

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
      :lines: 1-14
      :lineno-start: 1

#. Ausführen

   Mit ``bin/tox`` werden dann die folgenden Schritte durchlaufen:

   #. Erstellen der in :envvar:`env_list` angegebenen Umgebungen

      In jeder dieser Umgebungen werden dann die Abhängigkeiten und Pakete
      installiert

   #. Erstellen eines Reports mit den Ergebnissen aus jeder der Umgebungen,
      :abbr:`z.B. (zum Beispiel)`:

      .. code-block:: text

           ____________________ summary ____________________
           ERROR:   py37: commands failed
           py38: commands succeeded
           py39: commands succeeded
           py310: commands succeeded
           py311: commands succeeded

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

   .. literalinclude:: ci.yaml
      :language: yaml
      :lines: 1-45
      :lineno-start: 1

   .. note::
      Passt :abbr:`ggf. (gegebenenfalls)` die Python-Versionen in
      :envvar:`python-version` an. ihr müsst jedoch nicht auch die
      Umgebungsvariable in :envvar:`USING_COVERAGE` ändern, da dies durch das
      tox-Plugin ``tox-gh-actions`` (:abbr:`s.u. (siehe unten)`) erfolgt.

#. Anschließend könnt ihr auf :guilabel:`Start commit` klicken. Da wir noch
   weitere Änderungen vornehmen wollen bevor die Tests automatisiert ausgeführt
   werden sollen, wählen wir :guilabel:`Create a new branch for this commit and
   start a pull request` und als Name für den neuen :term:`Branch <branch>`
   ``github-actions``. Schließlich könnt ihr auf :guilabel:`Create pull request`
   klicken.
#. Um nun in den neuen Branch zu wechseln, gehen wir zu :menuselection:`Code -->
   main --> github-actions`.
#. `tox-gh-actions <https://pypi.org/project/tox-gh-actions/>`_ vereinfacht das
   Ausführen von tox in GitHub-Actions indem es als Umgebung für die Tests
   diejenige bereitstellt, die auch tox selbst verwendet. Hierfür müssen wir
   jedoch noch unsere :file:`tox.ini`-Datei anpassen, :abbr:`z.B. (zum
   Beispiel)`:

   .. literalinclude:: tox.ini
      :language: ini
      :lines: 15-
      :lineno-start: 15

   Dies ordnet GitHub-Actions tox-Umgebungen zu.

   .. note::
      * Es müssen nicht alle Varianten eurer Umgebung angegeben werden. Dies
        unterscheidet ``tox-gh-actions`` von ``tox -e py``.
      * Stellt sicher, dass die Versionen im ``[gh-actions]``-Abschnitt mit den
        verfügbaren Python-Versionen und :abbr:`ggf. (gegebenenfalls)` mit denen
        in den :ref:`GitHub-Actions für Git pre-commit Hooks
        <gh-action-pre-commit-example>` übereinstimmen.
      * Da alle Tests für eine spezifische Python-Version nacheinander in einem
        Container ausgeführt werden, gehen hierbei die Vorteile der parallelen
        Ausführung verloren.

   .. seealso::
      * `Building and testing Python
        <https://docs.github.com/en/actions/automating-builds-and-tests/building-and-testing-python>`_
      * `Workflow syntax for GitHub Actions
        <https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions>`_

#. Nun könnt ihr in eurer :file:`README.rst`-Datei noch ein Badge eures
   :abbr:`CI (Continuous Integration)`-Status hinzufügen, :abbr:`z.B. (zum
   Beispiel)` mit:

   .. code-block::

    .. image:: https://github.com/YOU/YOUR_PROJECT/workflows/CI/badge.svg?branch=main
         :target: https://github.com/YOU/YOUR_PROJECT/actions?workflow=CI
         :alt: CI Status

#. Die Testabdeckung könnt ihr auf GitHub veröffentlichen, :abbr:`s.a. (siehe
   auch)` :ref:`Coverage GitHub-Actions <coverage-github-actions>`.
#. Ihr könnt auch noch ein Badge für die Code-Abdeckung in eurer
   :file:`README.rst`-Datei anzeigen, :abbr:`s.a. (siehe auch)` :ref:`Coverage
   badge <coverage-badge>`.
