Testen
======

Builds und Links
----------------

.. _build-errors:

Build-Fehler
~~~~~~~~~~~~

Ihr habt die Möglichkeit, vor der Veröffentlichung eurer Änderungen zu
überprüfen, ob eure Inhalte ordnungsgemäß erstellt werden. Hierfür hat
`Sphinx <https://www.sphinx-doc.org/>`_ einen pingelig (:abbr:`engl.
(englisch)` nitpicky)-Modus, der mit der Option ``-n`` aufgerufen werden kann,
also :abbr:`z.B. (zum Beispiel)` mit:

.. tab:: Linux/macOS

   .. code-block:: console

       $ python -m sphinx -nb html docs/ docs/_build/

.. tab:: Windows

   .. code-block:: ps1con

       C:> python -m sphinx -nb html docs\ docs\_build\

.. _link-checks:

Links
~~~~~

Ihr könnt auch automatisiert sicherstellen, dass die von euch angegebenen
Linkziele erreichbar sind. Unser Dokumentationswerkzeug Sphinx verwendet hierfür
einen ``linkcheck``-Builder, den ihr :abbr:`ggf. (gegebenenfalls)` aufrufen
könnt mit:

.. tab:: Linux/macOS

   .. code-block:: console

       $ python -m sphinx -b linkcheck docs/ docs/_build/

.. tab:: Windows

   .. code-block:: ps1con

       C:> python -m sphinx -b linkcheck docs\ docs\_build\

Die Ausgabe kann dann :abbr:`z.B. (zum Beispiel)` so aussehen:

.. tab:: Linux/macOS

   .. code-block:: console

       $ python -m sphinx -b linkcheck docs/ docs/_build/
       Running Sphinx v3.5.2
       loading translations [de]... done
       …
       building [mo]: targets for 0 po files that are out of date
       building [linkcheck]: targets for 27 source files that are out of date
       …
       (content/accessibility: line   89) ok        https://bbc.github.io/subtitle-guidelines/
       (content/writing-style: line  164) ok        http://disabilityinkidlit.com/2016/07/08/introduction-to-disability-terminology/

       …
       (   index: line    5) redirect  https://cusy-design-system.readthedocs.io/ - with Found to https://cusy-design-system.readthedocs.io/de/latest/
       …
       (accessibility/color: line  114) broken    https://chrome.google.com/webstore/detail/nocoffee/jjeeggmbnhckmgdhmgdckeigabjfbddl - 404 Client Error: Not Found for url: https://chrome.google.com/webstore/detail/nocoffee/jjeeggmbnhckmgdhmgdckeigabjfbddl

.. tab:: Windows

   .. code-block:: ps1con

       C:> python -m sphinx -b linkcheck docs\ docs\_build\
       Running Sphinx v3.5.2
       loading translations [de]... done
       …
       building [mo]: targets for 0 po files that are out of date
       building [linkcheck]: targets for 27 source files that are out of date
       …
       (content/accessibility: line   89) ok        https://bbc.github.io/subtitle-guidelines/
       (content/writing-style: line  164) ok        http://disabilityinkidlit.com/2016/07/08/introduction-to-disability-terminology/

       …
       (   index: line    5) redirect  https://cusy-design-system.readthedocs.io/ - with Found to https://cusy-design-system.readthedocs.io/de/latest/
       …
       (accessibility/color: line  114) broken    https://chrome.google.com/webstore/detail/nocoffee/jjeeggmbnhckmgdhmgdckeigabjfbddl - 404 Client Error: Not Found for url: https://chrome.google.com/webstore/detail/nocoffee/jjeeggmbnhckmgdhmgdckeigabjfbddl

.. _ci-docs:

Kontinuierliche Integration
~~~~~~~~~~~~~~~~~~~~~~~~~~~

:abbr:`Ggf. (Gegebenenfalls)` könnt ihr auch automatisiert in eurer
:term:`CI`-Pipeline überprüfen, ob die Dokumentation gebaut wird und die Links
gültig sind. In :doc:`../../test/tox` kann die Konfiguration folgendermaßen
ergänzt werden:

.. code-block:: ini
   :caption: tox.ini

   [testenv:docs]
   # Keep base_python in sync with ci.yml and .readthedocs.yaml.
   base_python = py312
   extras = docs
   commands =
     sphinx-build -n -T -W -b html -d {envtmpdir}/doctrees docs docs/_build/html

   [testenv:docs-linkcheck]
   base_python = {[testenv:docs]base_python}
   extras = {[testenv:docs]extras}
   commands = sphinx-build -W -b linkcheck -d {envtmpdir}/doctrees docs docs/_build/html

Anschließend könnt ihr :abbr:`z.B. (zum Beispiel)` für GitHub folgende Jobs
definieren:

.. code-block:: yaml
   :caption: .github/workflows/ci.yml

   docs:
     name: Build docs and run doctests
     needs: build-package
     runs-on: ubuntu-latest
     steps:
     - name: Download pre-built packages
       uses: actions/download-artifact@v4
       with:
         name: Packages
         path: dist
     - run: tar xf dist/*.tar.gz --strip-components=1

     - uses: actions/setup-python@v5
       with:
         # Keep in sync with tox.ini/docs and .readthedocs.yaml
         python-version: "3.12"
         cache: pip
     - run: python -m pip install tox
     - run: python -m tox run -e docs

reST-Formatierung
-----------------

Ob die :doc:`Sphinx <start>`-Dokumentation in gültigem :doc:`rest`-Format
geschrieben ist, lässt sich mit `sphinx-lint
<https://pypi.org/project/sphinx-lint/>`_ überprüfen. Dies binden wir
üblicherweise in unsere :doc:`pre-commit
<Python4DataScience:productive/git/advanced/hooks/pre-commit>`-Konfiguration
ein:

.. code-block:: yaml
   :caption: .pre-commit-config.yaml

   - repo: https://github.com/sphinx-contrib/sphinx-lint
     rev: v1.0.0
     hooks:
       - id: sphinx-lint
         types: [rst]

.. seealso::
   Mit :doc:`Sybil:index` könnt ihr nicht nur :doc:`rest` überprüfen, sondern
   :abbr:`z.B. (zum Beispiel)` auch :doc:`Markdown <Sybil:markdown>` und
   :doc:`Myst <Sybil:myst>`. Darüberhinaus kann Sybil auch Code-Blöcke in der
   Dokumentation entweder mit :doc:`../../test/pytest/index` oder mit
   :doc:`../../test/unittest` überprüfen.

.. _test_code:

Code
----

Mit der eingebauten Python-Bibliothek :doc:`../doctest` könnt ihr auch Code in
eurer Dokumentation mit der :func:`doctest.testfile`-Methode testen:

.. code-block:: Python

   import doctest

   doctest.testfile("example.rst")

Dieses kurze Skript führt alle interaktiven Python-Beispiele aus, die in der
Datei :file:`example.rst` enthalten sind, und überprüft sie. Der Inhalt der
Datei wird so behandelt, als wäre er ein einziger riesiger Docstring.

.. seealso::
   Ein einfaches Beispiel findet ihr in der Python-Dokumentation: `Simple Usage:
   Checking Examples in a Text File
   <https://docs.python.org/3/library/doctest.html#simple-usage-checking-examples-in-a-text-file>`_.

   Eine andere Möglichkeit, Code in Dokumentationen zu testen ist
   `pytest-doctestplus <https://github.com/scientific-python/pytest-doctestplus>`_.

Code-Formatierung
-----------------

Die Formatierung von Code-Blöcken lässt sich mit `blacken-docs
<https://github.com/adamchainz/blacken-docs>`_ überprüfen, das
:doc:`Python4DataScience:productive/qa/black` verwendet. Üblicherweise binden
wir die Bibliothek über das :doc:`pre-commit
<Python4DataScience:productive/git/advanced/hooks/pre-commit>`-Framework ein:

.. code-block:: yaml
   :caption: .pre-commit-config.yaml

   - repo: https://github.com/adamchainz/blacken-docs
     rev: "v1.12.1"
     hooks:
     - id: blacken-docs
       additional_dependencies:
       - black

blacken-docs unterstützt aktuell die folgenden black-Optionen:

* `line-length
  <https://black.readthedocs.io/en/stable/usage_and_configuration/the_basics.html#l-line-length>`_
* `preview
  <https://black.readthedocs.io/en/stable/usage_and_configuration/the_basics.html#preview>`_
* `skip-string-normalization
  <https://black.readthedocs.io/en/stable/usage_and_configuration/the_basics.html#s-skip-string-normalization>`_
* `target-version
  <https://black.readthedocs.io/en/stable/usage_and_configuration/the_basics.html#t-target-version>`_

Rechtschreibung
---------------

Die englische Rechtschreibung lässt sich überprüfen mit `codespell
<https://github.com/codespell-project/codespell>`_. Es nutzt eine erweiterte
Version der auf `Wikipedia
<https://en.wikipedia.org/wiki/Wikipedia:Lists_of_common_misspellings/For_machines>`_
verfügbaren Wörterbücher. :abbr:`Ggf. (Gegebenenfalls)` könnt ihr jedoch auch
eigene Wörterbucher mit der ``--builtin``-Option bereitstellen.

Ihr könnt ``codespell`` in der :file:`pyproject.toml` konfigurieren, :abbr:`z.B.
(zum Beispiel)`:

.. code-block:: toml
   :caption: pyproject.toml

   [project.optional-dependencies]
   docs = [
       "...",
       "codespell",
   ]

   [tool.codespell]
   ignore-words-list = "uint"
   skip = "./.*, *.po, ./docs/_build"
   count = true
   quiet-level = 3

Ihr könnt ``codespell`` automatisch vor jedem Git-Commit ausführen, indem ihr
folgendes in die :file:`.pre-commit-config.yaml`-Datei eintragt:

.. code-block:: yaml
   :caption: .pre-commit-config.yaml

   - repo: https://github.com/codespell-project/codespell
     rev: v2.3.0
     hooks:
       - id: codespell

`Vale <https://vale.sh>`_ geht über Rechtschreib- und Grammatikprüfungen hinaus.
Es überprüft auch den Sprachstil: Wiederholt sich das Gesagte? Ist die Sprache
zu informell? Ist die Ansprache inkonsistent? Werden unerwünschte Klischees
bedient? Oder ist die Sprache sexistisch?

Vale wird von vielen Open-Source-Projekten genutzt, :abbr:`u.a. (unter anderem)`
von

* GitLab (`.vale.ini
  <https://gitlab.com/gitlab-org/gitlab/blob/master/.vale.ini>`_, `Regeln
  <https://gitlab.com/gitlab-org/gitlab/-/tree/master/doc/.vale/gitlab_base>`__)
* Homebrew (`.vale.ini
  <https://github.com/Homebrew/brew/blob/master/.vale.ini>`__, `Regeln
  <https://github.com/Homebrew/brew/tree/master/docs/vale-styles/Homebrew>`__)

Mit Vale selbst kommen die folgenden Stile mit:

`Microsoft <https://github.com/errata-ai/Microsoft>`_
    Eine Implementierung des `Microsoft Writing Style Guide
    <https://learn.microsoft.com/en-us/style-guide/welcome/>`__.
`Google <https://github.com/errata-ai/Google>`_
    Eine Implementierung des Styleguides für den `Google developer documentation
    style guide <https://developers.google.com/style/>`__.
`write-good <https://github.com/errata-ai/write-good>`_
    Eine Umsetzung der vom `write-good
    <https://github.com/btford/write-good>`__-Linter erzwungenen Richtlinien.
`proselint <https://github.com/errata-ai/Joblint>`_
    Eine Umsetzung der vom `proselint
    <https://github.com/amperser/proselint/>`__-Linter erzwungenen Richtlinien.
`Joblint <https://github.com/errata-ai/Joblint>`_
    Eine Umsetzung der vom `Joblint
    <https://github.com/rowanmanning/joblint>`__-Linter erzwungenen Richtlinien.

Vale wird in der :file:`.vale.ini`-Datei konfiguriert:

.. code-block:: ini
   :caption: .vale.ini

   StylesPath = styles
   MinAlertLevel = suggestion

   Packages = https://github.com/cusyio/cusy-vale/archive/refs/tags/v0.1.0.zip

   [*.{md,rst}]
   BasedOnStyles = cusy-de

.. seealso::
   * `Vale Configuration <https://vale.sh/docs/topics/config/>`_

Anschließend solltet ihr :abbr:`ggf. (gegebenenfalls)` eure :ref:`.gitignore
<gitignore>`-Datei aktualisieren:

.. code-block:: ini
   :caption: .gitignore

   styles/*

Ihr könnt Vale für das :doc:`pre-commit
<Python4DataScience:productive/git/advanced/hooks/pre-commit>`-Framework
konfigurieren mit:

.. code-block:: yaml
   :caption: .pre-commit-config.yaml

   - repo: https://github.com/errata-ai/vale
     rev: v3.7.1
     hooks:
     - id: vale sync
       pass_filenames: false
       args: [sync]
     - id: vale
       args: [--output=line, --minAlertLevel=error, .]

.. _docstrings-coverage:

Docstrings-Coverage
-------------------

`interrogate <https://interrogate.readthedocs.io/en/latest/>`_ prüft eure
Codebasis auf fehlende Dokumentationsstrings und generiert ein
`shields.io-ähnliches Badge
<https://interrogate.readthedocs.io/en/latest/#other-usage>`_.

Ihr könnt ``interrogate`` :abbr:`z.B. (zum Beispiel)` in der
:ref:`pyproject-toml`-Datei konfigurieren:

.. code-block:: toml
   :caption: pyproject.toml
   :emphasize-lines: 4, 7-

   [project.optional-dependencies]
   docs = [
       "...",
       "interrogate",
   ]

   [tool.interrogate]
   ignore-init-method = true
   ignore-init-module = false
   ignore-magic = false
   ignore-semiprivate = false
   ignore-private = false
   ignore-module = false
   ignore-property-decorators = false
   fail-under = 95
   exclude = ["tests/functional/sample", "setup.py", "docs"]
   verbose = 0
   omit-covered-files = false
   quiet = false
   whitelist-regex = []
   ignore-regex = []
   color = true

.. seealso::

   * `Configuration <https://interrogate.readthedocs.io/en/latest/index.html#configuration>`_

Nun könnt ihr ``interrogate`` in eure :doc:`../../test/tox`-Datei einfügen,
:abbr:`z.B. (zum Beispiel)` mit

.. code-block:: ini
   :caption: tox.ini

   [testenv:doc]
   deps = interrogate
   skip_install = true
   commands =
       interrogate --quiet --fail-under 95 src tests

Ihr könnt ``interrogate`` auch mit :doc:`pre-commit
<Python4DataScience:productive/git/advanced/hooks/pre-commit>` nutzen:

.. code-block:: yaml
   :caption: .pre-commit-config.yaml

   repos:
     - repo: https://github.com/econchick/interrogate
       rev: 1.7.0
       hooks:
         - id: interrogate
           args: [--quiet, --fail-under=95]
           pass_filenames: false
