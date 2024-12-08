GitLab Package Registry
=======================

Ihr könnt eure Verteilungspakete auch in der Paketregistrierung eures
GitLab-Projekts veröffentlichen. Folgende Bedingungen müssen hierfür jedoch
erfüllt sein:

* Ihr müsst euch bei der `Paketregistrierung authentifizieren
  <https://docs.gitlab.com/ee/user/packages/pypi_repository/?tab=With+a+deploy+token#authenticate-with-the-gitlab-package-registry>`_.
* Eure `Versionsangabe
  <https://docs.gitlab.com/ee/user/packages/pypi_repository/?tab=With+a+deploy+token#use-valid-version-strings>`_
  muss gültig sein.
* Das Paket muss kleiner als 5 GB sein und ``description`` darf höchstens 4000
  Zeichen lang sein.
* Das Paket wurde noch nicht in der Paketregistrierung veröffentlicht. Der
  Versuch, die gleiche Version eines Pakets zu veröffentlichen, liefert ``400
  Bad Request`` zurück.

Anschließend könnt ihr sowohl mit :term:`pip` als auch mit :term:`uv` das Paket
nutzen.

.. seealso::
   `PyPI packages in the Package Registry
   <https://docs.gitlab.com/ee/user/packages/pypi_repository/#publish-a-pypi-package-by-using-twine>`_

Authentifizierung
-----------------

Zur Authentifizierung an der GitLab Package Registry könnt ihr eine der
folgenden Methoden verwenden:

* Ein `persönlicher Zugriffstoken
  <https://docs.gitlab.com/ee/user/profile/personal_access_tokens.html>`_ mit
  dem Geltungsbereich ``api``.
* Ein `Deploy-Token
  <https://docs.gitlab.com/ee/user/project/deploy_tokens/index.html>`_ mit den
  Geltungsbereichen ``read_package_registry``, ``write_package_registry`` oder
  beiden.
* Ein `CI-Job-Token <https://docs.gitlab.com/ee/ci/jobs/ci_job_token.html>`_.

.. tab:: Persönlicher Zugriffstoken

   .. code-block:: yaml
      :caption: .gitlab-ci.yml
      :emphasize-lines: 5-6

      variables:
        UV_VERSION: 0.5
        PYTHON_VERSION: 3.12
        BASE_LAYER: bookworm-slim
        UV_PUBLISH_USERNAME: <personal_access_token_name>
        UV_PUBLISH_PASSWORD: <personal_access_token>

.. tab:: Deploy-Token

   .. code-block:: yaml
      :caption: .gitlab-ci.yml
      :emphasize-lines: 5-6

      variables:
        UV_VERSION: 0.5
        PYTHON_VERSION: 3.12
        BASE_LAYER: bookworm-slim
        UV_PUBLISH_USERNAME: <deploy_token_username>
        UV_PUBLISH_PASSWORD: <deploy_token>

.. tab:: Job-Token

   .. code-block:: yaml
      :caption: .gitlab-ci.yml
      :emphasize-lines: 5-6

      variables:
        UV_VERSION: 0.5
        PYTHON_VERSION: 3.12
        BASE_LAYER: bookworm-slim
        UV_PUBLISH_USERNAME: <gitlab-ci-token>
        UV_PUBLISH_PASSWORD: $CI_JOB_TOKEN

Authentifizierung für eine Gruppe
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Verwendet statt der :samp:`{PROJECT_ID}` die :samp:`{GROUP_URL}`.

Veröffentlichen des Verteilungspakets
-------------------------------------

Nun könnt ihr euer Paket auf GitLab veröffentlichen mit:

.. code-block:: yaml
   :caption: .gitlab-ci.yml

   …
   stages:
     - publish

   uv:
     stage: publish
     image: ghcr.io/astral-sh/uv:$UV_VERSION-python$PYTHON_VERSION-$BASE_LAYER
     script:
       - uv build
       - uv publish --publish-url ${CI_API_V4_URL}/projects/${CI_PROJECT_ID}/packages/pypi dist/*

.. tip::
   :abbr:`Ggf. (Gegebenenfalls)` könnt ihr mit ``RUST_LOG=uv=trace`` weitere
   Informationen zu den Authentifizierungsversuchen erhalten, also :abbr:`z.B.
   (zum Beispiel)` mit ``RUST_LOG=uv=trace uv --verbose publish --publish-url
   ${CI_API_V4_URL}/projects/${CI_PROJECT_ID}/packages/pypi dist/*``.

.. seealso::
   In :ref:`uv-gitlab` erhaltet ihr weitere Ninweise zur Konfiguration der
   :file:`.gitlab-ci.yml`-Datei.

Installieren des Pakets
-----------------------

Ihr könnt die neueste Version eures Pakets installieren :abbr:`z.B. (zum
Beispiel)` mit

.. code-block:: console

   $ uv add -i https://{NAME}:{PERSONAL_ACCESS_TOKEN}@ce.cusy.io/api/v4/projects/{PROJECT_ID}/packages/pypi/simple --no-deps {PACKAGE_NAME}

… oder von der Gruppenebene aus mit

.. code-block:: console

   $ uv add -i https://{NAME}:{PERSONAL_ACCESS_TOKEN}@ce.cusy.io/api/v4/groups/{GROUP_ID}/-/packages/pypi/simple --no-deps {PACKAGE_NAME}

… oder in der :file:`pyproject.toml`-Datei mit:

.. code-block:: toml
   :caption: pyproject.toml

   [tool.uv]
   extra-index-url = ["https://ce.cusy.io/api/v4/projects/{PROJECT_ID}/packages/pypi/simple {PACKAGE_NAME}"]
