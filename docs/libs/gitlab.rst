GitLab Package Registry
=======================

Ihr könnt eure Verteilungspakete auch in der Paketregistrierung eures
GitLab-Projekts veröffentlichen und sowohl mit :term:`Pip` als auch mit
:term:`twine` nutzen.

.. seealso::
   `PyPI packages in the Package Registry
   <https://docs.gitlab.com/ee/user/packages/pypi_repository/#publish-a-pypi-package-by-using-twine>`_

Authentifizierung
-----------------

Zur Authentifizierung an der GitLab Package Registry könnt ihr eine der
folgenden Methoden verwenden:

* Ein :ref:`persönliches Zugriffstoken
  <personal-access-tokens>` mit dem Geltungsbereich ``api``.
* Ein :ref:`Deploy-Token <deploy-tokens>` mit den Geltungsbereichen
  ``read_package_registry``, ``write_package_registry`` oder beiden.
* Ein :ref:`CI-Job-Token <ci-job-token>`.

.. _personal-access-tokens:

… mit einem persönlichen Zugriffstoken
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Um euch mit einem persönlichen Zugriffstoken zu authentifizieren, könnt ihr in
der ``~/.pypirc``-Datei :abbr:`z.B. (zum Beispiel)` folgendes hinzufügen:

.. code-block:: ini

    [distutils]
    index-servers=
        gitlab

    [gitlab]
    repository = https://ce.cusy.io/api/v4/projects/{PROJECT_ID}/packages/pypi
    username = {NAME}
    password = {YOUR_PERSONAL_ACCESS_TOKEN}

.. _deploy-tokens:

… mit einem Deploy-Token
~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: ini

    [distutils]
    index-servers =
        gitlab

    [gitlab]
    repository = https://ce.cusy.io/api/v4/projects/{PROJECT_ID}/packages/pypi
    username = {DEPLOY_TOKEN_USERNAME}
    password = {DEPLOY_TOKEN}

.. _ci-job-token:

… mit einem Job-Token
~~~~~~~~~~~~~~~~~~~~~

.. code-block:: yaml

    image: python:latest

    run:
      script:
        - pip install build twine
        - python -m build
        - TWINE_PASSWORD=${CI_JOB_TOKEN} TWINE_USERNAME=gitlab-ci-token python -m twine upload --repository-url ${CI_API_V4_URL}/projects/${CI_PROJECT_ID}/packages/pypi dist/*

… für den Zugriff auf Pakete innerhalb einer Gruppe
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Verwendet statt der :samp:`{PROJECT_ID}` die :samp:`{GROUP_URL}`.

Veröffentlichen des Verteilungspakets
-------------------------------------

Ihr könnt euer Paket mit Hilfe von :term:`twine` veröffentlichen:

.. code-block:: console

    python3 -m twine upload --repository gitlab dist/*

.. note::
   Wenn ihr versucht, ein Paket zu veröffentlichen, das bereits mit demselben
   Namen und derselben Version existiert, erhaltet ihr den Fehler ``400 Bad
   Request``; ihr müssen das vorhandene Paket dann zuerst löschen.

Installieren des Pakets
-----------------------

Ihr könnt die neueste Version eures Pakets installieren :abbr:`z.B. (zum
Beispiel)` mit

.. code-block:: console

   pip install --index-url https://{NAME}:{PERSONAL_ACCESS_TOKEN}@ce.cusy.io/api/v4/projects/{PROJECT_ID}/packages/pypi/simple --no-deps {PACKAGE_NAME}

… oder von der Gruppenebene aus mit

.. code-block:: console

   pip install --index-url https://{NAME}:{PERSONAL_ACCESS_TOKEN}@ce.cusy.io/api/v4/groups/{GROUP_ID}/-/packages/pypi/simple --no-deps {PACKAGE_NAME}

… oder in der :file:`requirements.txt`-Datei mit

.. code-block::

   --extra-index-url https://ce.cusy.io/api/v4/projects/{PROJECT_ID}/packages/pypi/simple {PACKAGE_NAME}
