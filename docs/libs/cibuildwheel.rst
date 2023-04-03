``cibuildwheel``
================

:term`cibuildwheel` vereinfacht die Erstellung von :term:`Python Wheels <wheel>`
für die verschiedenen Plattformen und Python-Versionen durch Continuous
Integration (CI) Workflows. Genauer gesagt baut es Manylinux-, macOS 10.9+- und
Windows-Wheels für CPython und PyPy mit GitHub Actions, Azure Pipelines, Travis
CI, AppVeyor, CircleCI, oder GitLab CI.

Darüber hinaus bündelt es gemeinsam genutzte Bibliotheksabhängigkeiten unter
Linux und macOS durch `auditwheel <https://github.com/pypa/auditwheel>`_ und
`delocate <https://github.com/matthew-brett/delocate>`_.

Schließlich können die Tests auch gegen die Wheels laufen.

.. seealso::
   * `Docs <https://cibuildwheel.readthedocs.io/>`_
   * `GitHub <https://github.com/pypa/cibuildwheel>`_

GitHub Actions
--------------

Um Linux-, macOS- und Windows-Wheels erstellen zu können, erstellt eine
``.github/workflows/build_wheels.yml``-Datei in eurem GitHub Repo:

.. code-block:: yaml

    name: Build

    on: [push, pull_request]

    jobs:
      build_wheels:
        name: Build wheels on ${{ matrix.os }}
        runs-on: ${{ matrix.os }}
        strategy:
          matrix:
            os: [ubuntu-20.04, windows-2019, macos-10.15]

        steps:
          - uses: actions/checkout@v2

          - name: Build wheels
            uses: pypa/cibuildwheel@v1.11.0
            # to supply options, put them in 'env', like:
            # env:
            #   CIBW_SOME_OPTION: value

          - uses: actions/upload-artifact@v2
            with:
              path: ./wheelhouse/*.whl

Dadurch wird der CI-Workflow mit den folgenden Standardeinstellungen ausgeführt:

* ``package-dir: .``
* ``output-dir: wheelhouse``

Ihr könnt die Datei erweitern um die Wheels automatisch auf den :term:`Python
Package Index` (:term:`PyPI`) hochzuladen mit:

.. code-block:: yaml

      upload_pypi:
        needs: [build_wheels, build_sdist]
        runs-on: ubuntu-latest
        # upload to PyPI on every tag starting with 'v'
        if: github.event_name == 'push' && startsWith(github.event.ref, 'refs/tags/v')
        # alternatively, to publish when a GitHub Release is created, use the following rule:
        # if: github.event_name == 'release' && github.event.action == 'published'
        steps:
          - uses: actions/download-artifact@v2
            with:
              name: artifact
              path: dist

          - uses: pypa/gh-action-pypi-publish@master
            with:
              user: __token__
              password: ${{ secrets.pypi_password }}
              # To test: repository_url: https://test.pypi.org/legacy/

.. seealso::
   * `Workflow syntax for GitHub Actions
     <https://docs.github.com/en/actions/reference/workflow-syntax-for-github-actions>`_

Gitlab CI
---------

Um Linux-Wheels mit Gitlab CI zu bauen, erstellt eine  ``.gitlab-ci.yml``-Datei
in eurem Repository:

.. code-block:: yaml

    linux:
      image: python:3.8
      # make a docker daemon available for cibuildwheel to use
      services:
        - name: docker:dind
          entrypoint: ["env", "-u", "DOCKER_HOST"]
          command: ["dockerd-entrypoint.sh"]
      variables:
        DOCKER_HOST: tcp://docker:2375/
        DOCKER_DRIVER: overlay2
        # See https://github.com/docker-library/docker/pull/166
        DOCKER_TLS_CERTDIR: ""
      script:
        - curl -sSL https://get.docker.com/ | sh
        - python -m pip install cibuildwheel==1.11.0
        - cibuildwheel --output-dir wheelhouse
      artifacts:
        paths:
          - wheelhouse/

.. seealso::
   * `Keyword reference for the .gitlab-ci.yml file
     <https://docs.gitlab.com/ee/ci/yaml/>`_

Beispiele
---------

* Coverage.py: `.github/workflows/kit.yml <https://github.com/nedbat/coveragepy/blob/master/.github/workflows/kit.yml>`_
* matplotlib: `.github/workflows/cibuildwheel.yml <https://github.com/matplotlib/matplotlib/blob/master/.github/workflows/cibuildwheel.yml>`_
* MyPy: `.github/workflows/build.yml
  <https://github.com/mypyc/mypy_mypyc-wheels/blob/master/.github/workflows/build.yml>`__
* psutil: `.github/workflows/build.yml
  <https://github.com/giampaolo/psutil/blob/master/.github/workflows/build.yml>`__
