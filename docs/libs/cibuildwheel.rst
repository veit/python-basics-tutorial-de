``cibuildwheel``
================

:term:`cibuildwheel` vereinfacht die Erstellung von :term:`Python Wheels
<wheel>` für die verschiedenen Plattformen und Python-Versionen durch Continuous
Integration (CI) Workflows. Genauer gesagt baut es Manylinux-, macOS 10.9+- und
Windows-Wheels für CPython und PyPy mit GitHub Actions, Azure Pipelines, Travis
CI, AppVeyor, CircleCI, oder
:doc:`Python4DataScience:productive/git/advanced/gitlab/ci-cd`.

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
:file:`.github/workflows/build_wheels.yml`-Datei in eurem GitHub Repo:

.. literalinclude:: .github/workflows/build_wheels.yml
   :language: yaml
   :lines: 1-7

``workflow_dispatch``
    ermöglicht euch, in der grafischen Benutzeroberfläche auf eine Schaltfläche
    zu klicken, um einen Build auszulösen. Das ist perfekt zum manuellen Testen
    von Wheels vor einer Veröffentlichung eignet, da ihr sie einfach von
    *artifacts* herunterladen könnt.

    .. seealso::
       * `workflow_dispatch
         <https://github.blog/changelog/2020-07-06-github-actions-manual-triggers-with-workflow_dispatch/>`_
``release``
    wird bei der Übertragung einer getaggten Version ausgeführt.

    .. seealso::
       * `release
         <https://docs.github.com/en/actions/using-workflows/events-that-trigger-workflows#release>`_

Nun können die :term:`Wheels <wheel>` gebaut werden mit:

.. literalinclude:: .github/workflows/build_wheels.yml
   :language: yaml
   :lines: 9-21

Dadurch wird der CI-Workflow mit den folgenden Standardeinstellungen ausgeführt:

* ``package-dir: .``
* ``output-dir: wheelhouse``
* ``config-file: "{package}/pyproject.toml"``

Ihr könnt die Datei auch erweitern um die Wheels automatisch auf den
:term:`Python Package Index` (:term:`PyPI`) hochzuladen. Hierfür solltet ihr
jedoch zunächst noch eine :term:`Source Distribution` erstellen, :abbr:`z.B.
(zum Beispiel)` mit:

.. literalinclude:: .github/workflows/build_wheels.yml
   :language: yaml
   :lines: 27-41

Zudem muss dieser GitHub-Workflow in den PyPI-Einstellungen eures Projekts
eingestellt werden:

* `Creating a PyPI project with a trusted publisher
  <https://docs.pypi.org/trusted-publishers/creating-a-project-through-oidc>`_
* `Adding a trusted publisher to an existing PyPI project
  <https://docs.pypi.org/trusted-publishers/adding-a-publisher>`_

Nun könnt ihr endlich die Artefakte beider Jobs auf den :term:`PyPI` hochladen:

.. literalinclude:: .github/workflows/build_wheels.yml
   :language: yaml
   :lines: 43-

.. seealso::
   * `Workflow syntax for GitHub Actions
     <https://docs.github.com/en/actions/reference/workflow-syntax-for-github-actions>`_

GitLab CI/CD
------------

Um Linux-Wheels mit
:doc:`Python4DataScience:productive/git/advanced/gitlab/ci-cd` zu bauen,
erstellt eine  :file:`.gitlab-ci.yml`-Datei in eurem Repository:

.. literalinclude:: .gitlab-ci.yml
   :language: yaml

.. seealso::
   * `Keyword reference for the .gitlab-ci.yml file
     <https://docs.gitlab.com/ee/ci/yaml/>`_

Optionen
--------

``cibuildwheel`` kann entweder über Umgebungsvariablen oder über eine
Konfigurationsdatei wie :file:`pyproject.toml` konfiguriert werden, :abbr:`z.B.
(zum Beispiel)`:

.. literalinclude:: pyproject.toml
   :language: toml

.. seealso::
   * `cibuildwheel: Options
     <https://cibuildwheel.readthedocs.io/en/stable/options/>`_

Beispiele
---------

* Coverage.py: `.github/workflows/kit.yml <https://github.com/nedbat/coveragepy/blob/master/.github/workflows/kit.yml>`_
* matplotlib: `.github/workflows/cibuildwheel.yml <https://github.com/matplotlib/matplotlib/blob/master/.github/workflows/cibuildwheel.yml>`_
* MyPy: `.github/workflows/build.yml
  <https://github.com/mypyc/mypy_mypyc-wheels/blob/master/.github/workflows/build.yml>`__
* psutil: `.github/workflows/build.yml
  <https://github.com/giampaolo/psutil/blob/master/.github/workflows/build.yml>`__
