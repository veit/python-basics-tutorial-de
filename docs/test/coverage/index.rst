Coverage
========

Ihr könnt einen Report für die Testabdeckung erstellen mit `Coverage.py
<https://github.com/nedbat/coveragepy>`_.

.. seealso::
   * `GitHub <https://github.com/nedbat/coveragepy>`_
   * `Docs <https://coverage.readthedocs.io/>`_

Installation
------------

.. tabs::

   .. tab:: Linux/MacOS

      .. code-block:: console

         $ bin/python -m pip install coverage

   .. tab:: Windows

      .. code-block:: ps1con

            C:> Scripts\python -m pip install coverage

.. note::
   Wollt ihr die Testabdeckung für Python 2 and Python<3.6 ermitteln, müsst ihr
   Coverage<6.0 verwenden.

Nutzung
-------

Ihr könnt euren üblichen Test-Runner zusammen mit Coverage ausführen

* … mit `pytest <https://docs.pytest.org/>`_:

  .. tabs::

     .. tab:: Linux/MacOS

        .. code-block:: console

           $ bin/python -m pip install pytest-cov

     .. tab:: Windows

        .. code-block:: ps1con

           C:> Scripts\python -m pip install pytest-cov

  oder für verteilte Tests

  .. tabs::

     .. tab:: Linux/MacOS

        .. code-block:: console

           $ bin/python -m pip install pytest-xdist

     .. tab:: Windows

        .. code-block:: ps1con

           C:> Scripts\python -m pip install pytest-xdist

  Anschließend könnt ihr die Testabdeckung überprüfen mit

  .. tabs::

     .. tab:: Linux/MacOS

        .. code-block:: console

           $ bin/pytest --cov=myproj tests/

     .. tab:: Windows

        .. code-block:: ps1con

           C:> Scripts\pytest --cov=myproj tests\

  .. seealso::
     * `pytest-cov’s documentation <https://pytest-cov.readthedocs.io/>`_

* … mit :doc:`../unittest`:

  .. tabs::

     .. tab:: Linux/MacOS

        .. code-block:: console

           $ bin/coverage run -m unittest discover

     .. tab:: Windows

        .. code-block:: ps1con

           C:> Scripts\coverage run -m unittest discover

* … mit `nose <https://nose.readthedocs.io/>`_:

  .. tabs::

     .. tab:: Linux/MacOS

        .. code-block:: console

           $ bin/coverage run -m nose arg1 arg2

     .. tab:: Windows

        .. code-block:: ps1con

           C:> Scripts\coverage run -m nose arg1 arg2

Testabdeckung aller Tests mit GitHub-Actions
--------------------------------------------

Nachdem ihr die Testabdeckung ausgeführt habt, könnt ihr die Dateien als
Artefakte hochladen um sie später in einem weiteren Job wiederverwenden zu
können:

.. code-block:: yaml

    - name: Upload coverage data
      uses: actions/upload-artifact@v2
      with:
        name: coverage-data
        path: ".coverage.*"
        if-no-files-found: ignore

``if-no-files-found: ignore`` ist sinnvoll, wenn nicht für alle Python-Versionen
die Testabdeckung gemessen werden soll um schneller zum Ergebnis zu kommen.
Daher solltet ihr nur für diejenigen Elemente eurer Matrix, die ihr
berücksichtigen wollt, die Daten hochladen.

Nachdem alle Tests durchlaufen wurden, könnt ihr einen weiteren Job definieren,
der die Ergebnisse zusammenführt:

.. code-block:: yaml

    coverage:
      runs-on: "ubuntu-latest"
      needs: tests
      steps:
        - uses: actions/checkout@v2
        - uses: actions/setup-python@v2
          with:
            # Use latest, so it understands all syntax.
            python-version: "3.10"

        - name: Install Coverage.py
          run: python -m pip install --upgrade coverage[toml]

        - name: Download coverage data
          uses: actions/download-artifact@v2
          with:
            name: coverage-data

        - name: Combine coverage and fail if it's <100%
          run: |
            python -m coverage combine
            python -m coverage html --skip-covered --skip-empty
            python -m coverage report --fail-under=100

        - name: Upload HTML report for failed check
          uses: actions/upload-artifact@v2
          with:
            name: html-report
            path: htmlcov
          if: ${{ failure() }}

``needs: tests`` stellt sicher, dass alle Tests durchgeführt werden. Wenn euer
Job, der die Tests ausführt, einen anderen Namen hat, müsst ihr ihn hier
anpassen. Anschließend werden die Daten der Testabdeckung heruntergeladen, die
zuvor die Tests als Artefakte hochgeladen haben, kombiniert sie, erstellt einen
HTML-Bericht und prüft schließlich mit ``fail_under``, ob die Abdeckung 100 %
beträgt – wenn nicht, wird der Job abgebrochen. Wenn – und nur wenn – dieser
Schritt fehlschlägt, wird auch der HTML-Bericht als Artefakt hochgeladen.

Sobald der Workflow abgeschlossen ist, könnt ihr den HTML-Bericht unten auf der
Workflow-Summary-Seite herunterladen:

.. seealso::
   * `structlog main.yml
     <https://github.com/hynek/structlog/blob/main/.github/workflows/main.yml>`_

Alternativen
------------

Mit :doc:`codecov` und :doc:`opencoverage` gibt es externe Services, um die
Testabdeckung zu überprüfen und anzuzeigen.

.. toctree::
    :hidden:
    :titlesonly:
    :maxdepth: 0

    codecov
    opencoverage
