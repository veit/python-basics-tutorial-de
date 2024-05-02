Testen
======

Formatierung
------------

Ob die :doc:`Sphinx <start>`-Dokumentation in gültigem :doc:`rest`-Format
geschrieben ist, lässt sich mit `sphinx-lint
<https://pypi.org/project/sphinx-lint/>`_ überprüfen. Dies binden wir
üblicherweise in unsere :doc:`pre-commit
<Python4DataScience:productive/git/advanced/hooks/pre-commit>`-Konfiguration
ein:

.. code-block:: yaml
   :caption: .pre-commit-config.yaml

   - repo: https://github.com/sphinx-contrib/sphinx-lint
     rev: v0.9.1
     hooks:
       - id: sphinx-lint
         args: [--jobs=1]
         types: [rst]

.. seealso::
   Mit :doc:`Sybil:index` könnt ihr nicht nur :doc:`rest` überprüfen, sondern
   :abbr:`z.B. (zum Beispiel)` auch :doc:`Markdown <Sybil:markdown>` und
   :doc:`Myst <Sybil:myst>`. Darüberhinaus kann Sybil auch Code-Blöcke in der
   Dokumentation entweder mit :doc:`../test/pytest/index` oder mit
   :doc:`../test/unittest` überprüfen.

Code-Formatierung
-----------------

Die Formatierung von Code-Blöcken lässt sich mit `blacken-docs
<https://github.com/adamchainz/blacken-docs>`_ überprüfen, das
:doc:`Python4DataScience:productive/qa/black` verwendet. Üblicherweise binden
wir die Bibliothek über das :doc:`pre-commit
<Python4DataScience:productive/git/advanced/hooks/pre-commit>`-Framework ein:

.. code-block:: yaml

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

.. _build-errors:

Build-Fehler
------------

Ihr habt die Möglichkeit, vor der Veröffentlichung eurer Änderungen zu
überprüfen, ob eure Inhalte ordnungsgemäß erstellt werden. Hierfür hat
`Sphinx <https://www.sphinx-doc.org/>`_ einen pingelig (:abbr:`engl.
(englisch)` nitpicky)-Modus, der mit der Option ``-n`` aufgerufen werden kann,
also :abbr:`z.B. (zum Beispiel)` mit:

.. tab:: Linux/macOS

   .. code-block:: console

       $ bin/python -m sphinx -nb html docs/ docs/_build/

.. tab:: Windows

   .. code-block:: ps1con

       C:> Scripts\python -m sphinx -nb html docs\ docs\_build\

.. _link-checks:

Links überprüfen
----------------

Ihr könnt auch automatisiert sicherstellen, dass die von euch angegebenen
Linkziele erreichbar sind. Unser Dokumentationswerkzeug Sphinx verwendet hierfür
einen ``linkcheck``-Builder, den ihr :abbr:`ggf. (gegebenenfalls)` aufrufen
könnt mit:

.. tab:: Linux/macOS

   .. code-block:: console

       $ bin/python -m sphinx -b linkcheck docs/ docs/_build/

.. tab:: Windows

   .. code-block:: ps1con

       C:> Scripts\python -m sphinx -b linkcheck docs\ docs\_build\

Die Ausgabe kann dann :abbr:`z.B. (zum Beispiel)` so aussehen:

.. tab:: Linux/macOS

   .. code-block:: console

       $ bin/python -m sphinx -b linkcheck docs/ docs/_build/
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

       C:> Scripts\python -m sphinx -b linkcheck docs\ docs\_build\
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

Kontinuierliche Integration
---------------------------

:abbr:`Ggf. (Gegebenenfalls)` könnt ihr auch automatisiert in eurer
:term:`CI`-Pipeline überprüfen, ob die Dokumentation gebaut wird und die Links
gültig sind. In :doc:`../test/tox` kann die Konfiguration folgendermaßen ergänzt
werden:

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
