Coverage
========

Ihr könnt einen Report für die Testabdeckung erstellen mit `Coverage.py
<https://github.com/nedbat/coveragepy>`_.

.. seealso::
   * `GitHub <https://github.com/nedbat/coveragepy>`_
   * `Docs <https://coverage.readthedocs.io/>`_

Installation
------------

.. tab:: Linux/macOS

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

  .. tab:: Linux/macOS

     .. code-block:: console

        $ bin/python -m pip install pytest-cov

  .. tab:: Windows

     .. code-block:: ps1con

        C:> Scripts\python -m pip install pytest-cov

  oder für verteilte Tests

  .. tab:: Linux/macOS

     .. code-block:: console

        $ bin/python -m pip install pytest-xdist

  .. tab:: Windows

     .. code-block:: ps1con

        C:> Scripts\python -m pip install pytest-xdist

  Anschließend könnt ihr die Testabdeckung überprüfen mit

  .. tab:: Linux/macOS

     .. code-block:: console

        $ bin/pytest --cov=myproj tests/

  .. tab:: Windows

     .. code-block:: ps1con

        C:> Scripts\pytest --cov=myproj tests\

  .. seealso::
     * `pytest-cov’s documentation <https://pytest-cov.readthedocs.io/>`_

* … mit :doc:`unittest`:

  .. tab:: Linux/macOS

     .. code-block:: console

        $ bin/coverage run -m unittest discover

  .. tab:: Windows

     .. code-block:: ps1con

        C:> Scripts\coverage run -m unittest discover

* … mit `nose <https://nose.readthedocs.io/>`_:

  .. tab:: Linux/macOS

     .. code-block:: console

        $ bin/coverage run -m nose arg1 arg2

  .. tab:: Windows

     .. code-block:: ps1con

        C:> Scripts\coverage run -m nose arg1 arg2

Erweiterungen
-------------

In `Coverage.py plugins
<https://gist.github.com/nedbat/2e9dbf7f33b1e0e857368af5c5d06202>`_ findet ihr
auch eine Reihe von Erweiterungen für Coverage.

.. _coverage-github-actions:

Testabdeckung aller Tests mit GitHub-Actions
--------------------------------------------

Nachdem ihr die Testabdeckung überprüft habt, könnt ihr die Dateien als
GitHub-Action :abbr:`z.B. (zum Beispiel)` in einer :download:`ci.yaml` als
Artefakte hochladen um sie später in weiteren Jobs wiederverwenden zu können:

.. literalinclude:: ci.yaml
   :language: yaml
   :lines: 47-52
   :lineno-start: 47

``if-no-files-found: ignore``
    ist sinnvoll, wenn nicht für alle Python-Versionen die Testabdeckung
    gemessen werden soll um schneller zum Ergebnis zu kommen. Daher solltet ihr
    nur für diejenigen Elemente eurer Matrix, die ihr berücksichtigen wollt, die
    Daten hochladen.

Nachdem alle Tests durchlaufen wurden, könnt ihr einen weiteren Job definieren,
der die Ergebnisse zusammenführt:

.. literalinclude:: ci.yaml
   :language: yaml
   :lines: 54-92
   :lineno-start: 54

``needs: tests``
    stellt sicher, dass alle Tests durchgeführt werden. Wenn euer Job, der die
    Tests ausführt, einen anderen Namen hat, müsst ihr ihn hier anpassen.

``name: "Download coverage data"``
    lädt die Daten der Testabdeckung herunter, die zuvor mit ``name: "Upload
    coverage data"`` hochgeladen wurden.
``name: "Combine coverage and fail it it’s under 100 %"``
    kombiniert die Testabdeckung und erstellt einen HTML-Bericht, wenn die
    Bedingung ``--fail-under=100`` erfüllt ist.

Sobald der Workflow abgeschlossen ist, könnt ihr den HTML-Bericht herunterladen
unter :menuselection:`YOUR_REPO --> Actions --> tests --> Combine and check
coverage`.

.. seealso::
   * `How to Ditch Codecov for Python Projects
     <https://hynek.me/articles/ditch-codecov-python/>`_
   * `structlog main.yml
     <https://github.com/hynek/structlog/blob/main/.github/workflows/ci.yml>`_

.. _coverage-badge:

Badge
-----

Ihr könnt GitHub Actions verwenden, um ein Badge mit eurer Code-Coverage zu
erstellen. Dabei wird zusätzlich ein GitHub Gist benötigt um die Parameter für
das Badge, das von `shields.io <https://shields.io>`_ gerendert wird, zu
speichern. Hierfür erweitern wir unsere :download:`ci.yaml` folgendermaßen:

.. literalinclude:: ci.yaml
   :language: yaml
   :lines: 94-
   :lineno-start: 94

Zeile 97
    ``GIST_TOKEN`` ist ein persönliches GitHub-Zugangs-Token.
Zeile 98
    ``YOUR_GIST_ID`` solltet ihr durch eure eigene Gist-ID ersetzen. Falls ihr
    noch keine Gist-ID habt, könnt ihr diese erstellen mit:

    #. Ruft https://gist.github.com auf und erstellt einen neuen Gist, den ihr
       :abbr:`z.B. (zum Beispiel)` :file:`test.json` nennen könnt. Die ID des
       Gist ist der lange alphanumerische Teil der URL, den ihr hier benötigt.
    #. Anschließend geht ihr zu https://github.com/settings/tokens und erstellt
       ein neues Token mit dem Gist-Bereich.
    #. Geht schließlich zu :menuselection:`YOUR_REPO --> Settings --> Secrets
       --> Actions` und fügt dieses Token hinzu. Ihr könnt ihm einen beliebigen
       Namen geben, :abbr:`z.B. (zum Beispiel)` :samp:`GIST_SECRET`.

       Wenn ihr `Dependabot <https://github.com/dependabot>`_ verwendet, um die
       Abhängigkeiten eures Repository automatisch zu aktualisieren, müsst ihr
       das  :samp:`GIST_SECRET` auch in :menuselection:`YOUR_REPO --> Settings
       --> Secrets --> Dependabot` hinzufügen.

Zeilen 102-104
    Das Badge wird automatisch eingefärbt:

    * ≤ 50 % in rot
    * ≥ 90 % in grün
    * mit einem Farbverlauf zwischen den beiden


Jetzt kann das Badge mit einer URL wie dieser angezeigt werden:
:samp:`https://img.shields.io/endpoint?url=https://gist.githubusercontent.com/{YOUR_GITHUB_NAME}/{GIST_SECRET}/raw/covbadge.json`.

.. image:: https://img.shields.io/endpoint?url=https://gist.githubusercontent.com/nedbat/a27aaed4944c1f760a969a543fb52767/raw/covbadge2_40.json
.. image:: https://img.shields.io/endpoint?url=https://gist.githubusercontent.com/nedbat/a27aaed4944c1f760a969a543fb52767/raw/covbadge2_45.json
.. image:: https://img.shields.io/endpoint?url=https://gist.githubusercontent.com/nedbat/a27aaed4944c1f760a969a543fb52767/raw/covbadge2_50.json
.. image:: https://img.shields.io/endpoint?url=https://gist.githubusercontent.com/nedbat/a27aaed4944c1f760a969a543fb52767/raw/covbadge2_55.json
.. image:: https://img.shields.io/endpoint?url=https://gist.githubusercontent.com/nedbat/a27aaed4944c1f760a969a543fb52767/raw/covbadge2_60.json
.. image:: https://img.shields.io/endpoint?url=https://gist.githubusercontent.com/nedbat/a27aaed4944c1f760a969a543fb52767/raw/covbadge2_65.json
.. image:: https://img.shields.io/endpoint?url=https://gist.githubusercontent.com/nedbat/a27aaed4944c1f760a969a543fb52767/raw/covbadge2_70.json
.. image:: https://img.shields.io/endpoint?url=https://gist.githubusercontent.com/nedbat/a27aaed4944c1f760a969a543fb52767/raw/covbadge2_75.json
.. image:: https://img.shields.io/endpoint?url=https://gist.githubusercontent.com/nedbat/a27aaed4944c1f760a969a543fb52767/raw/covbadge2_80.json
.. image:: https://img.shields.io/endpoint?url=https://gist.githubusercontent.com/nedbat/a27aaed4944c1f760a969a543fb52767/raw/covbadge2_85.json
.. image:: https://img.shields.io/endpoint?url=https://gist.githubusercontent.com/nedbat/a27aaed4944c1f760a969a543fb52767/raw/covbadge2_90.json
.. image:: https://img.shields.io/endpoint?url=https://gist.githubusercontent.com/nedbat/a27aaed4944c1f760a969a543fb52767/raw/covbadge2_95.json
.. image:: https://img.shields.io/endpoint?url=https://gist.githubusercontent.com/nedbat/a27aaed4944c1f760a969a543fb52767/raw/covbadge2_100.json
