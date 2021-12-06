Codecov
=======

Codecov sammelt Coverage-Reports für die Sprachen Python, C#/.net, Java,
Node/Javascript/Coffee, C/C++, D, Go, Groovy, Kotlin, PHP, R, Scala, Xtern,
Xcode, Lua und anderen, um sie dann an `codecov.io <https://about.codecov.io/>`_
zu übermitteln.

.. seealso::
   * `GitHub <https://github.com/codecov/codecov-python>`_
   * `Docs <https://docs.codecov.io/docs>`_

Installation
------------

Codecov kann einfach installiert werden mit

.. tabs::

   .. tab:: Linux/MacOS

      .. code-block:: console

         $ bin/python -m pip install codecov

   .. tab:: Windows

      .. code-block:: ps1con

            C:> Scripts\python -m pip install codecov

Verwendung
----------

…  im Terminal
~~~~~~~~~~~~~~

.. tabs::

   .. tab:: Linux/MacOS

      .. code-block:: console

         $ bin/codecov -t <repository-upload-token>

   .. tab:: Windows

      .. code-block:: ps1con

            C:> Scripts\codecov -t <repository-upload-token>

.. _together-with-github-actions:

… zusammen mit GitHub Actions
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Die Code-Abdeckung könnt ihr veröffentlichen, indem ihr in eurer
:file:`ci.yml`-Datei :abbr:`z.B. (zum Beispiel)` folgendes hinzufügt:

.. code-block:: yaml
   :emphasize-lines: 3-6

    - name: "Convert coverage"
      run: "python -m coverage xml"
    - name: "Upload coverage to Codecov"
      uses: "codecov/codecov-action@v1"
      with:
        fail_ci_if_error: true

.. seealso::
   * `Codecov GitHub Action <https://github.com/codecov/codecov-action>`_

…  zusammen mit Travis CI
~~~~~~~~~~~~~~~~~~~~~~~~~

Hierfür könnt Ihr in der ``.travis.yml``-Datei folgendes hinzufügen:

.. code-block:: yaml

    language:
      python
    after_success:
      - bash <(curl -s https://codecov.io/bash)

… zusammen mit ``tox``
~~~~~~~~~~~~~~~~~~~~~~

Codecov kann mit :doc:`../tox` eingerichtet werden:

.. code-block:: ini

    [testenv]
    passenv = TOXENV CI TRAVIS TRAVIS_* CODECOV_*
    deps = codecov>=1.4.0
    commands = codecov -e TOXENV

.. _codecov-badge:

Badge
-----

Nun könnt ihr in eurer :file:`README.rst`-Datei noch ein Badge hinzufügen,
:abbr:`z.B. (zum Beispiel)` mit:

.. code-block::

    .. image:: https://codecov.io/gh/YOU/YOUR_PROJECT/branch/main/graph/badge.svg
       :target: https://codecov.io/gh/YOU/YOUR_PROJECT
       :alt: Code Coverage Status (Codecov)
