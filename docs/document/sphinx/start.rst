Erstellt ein Sphinx-Projekt
===========================

Installation und Start
----------------------

#. Erstellt eine virtuelle Umgebung für euer Dokumentationsprojekt:

   .. tab:: Linux/macOS

      .. code-block:: console

         $ python3 -m venv .venv

   .. tab:: Windows

      .. code-block:: ps1con

         C:> python -m venv .venv

#. Wechselt in die virtuelle Umgebung und installiert dort Sphinx:

   .. tab:: Linux/macOS

      .. code-block:: console

         $ . .venv/bin/activate
         $ python -m pip install sphinx

   .. tab:: Windows

      .. code-block:: ps1con

         C:> .venv\Scripts\activate
         C:> python -m pip install sphinx

#. Erstellt euer Sphinx-Dokumentationsprojekt:

   .. tab:: Linux/macOS

      .. code-block:: console

         $ sphinx-quickstart docs
         Selected root path: docs
         > Separate source and build directories (y/n) [n]:
         > Name prefix for templates and static dir [_]:
         > Project name: my.package
         > Author name(s): Veit Schiele
         > Project release []: 1.0
         > Project language [en]:
         > Source file suffix [.rst]:
         > Name of your master document (without suffix) [index]:
         > autodoc: automatically insert docstrings from modules (y/n) [n]: y
         > doctest: automatically test code snippets in doctest blocks (y/n) [n]: y
         > intersphinx: link between Sphinx documentation of different projects (y/n) [n]: y
         > todo: write "todo" entries that can be shown or hidden on build (y/n) [n]: y
         > coverage: checks for documentation coverage (y/n) [n]:
         > imgmath: include math, rendered as PNG or SVG images (y/n) [n]:
         > mathjax: include math, rendered in the browser by MathJax (y/n) [n]:
         > ifconfig: conditional inclusion of content based on config values (y/n) [n]:
         > viewcode: include links to the source code of documented Python objects (y/n) [n]: y
         > githubpages: create .nojekyll file to publish the document on GitHub pages (y/n) [n]:
         > Create Makefile? (y/n) [y]:
         > Create Windows command file? (y/n) [y]:

         Creating file docs/source/conf.py.
         Creating file docs/source/index.rst.
         Creating file docs/Makefile.
         Creating file docs/make.bat.

   .. tab:: Windows

      .. code-block:: ps1con

         C:> sphinx-quickstart docs
         Selected root path: docs
         > Separate source and build directories (y/n) [n]:
         > Name prefix for templates and static dir [_]:
         > Project name: my.package
         > Author name(s): Veit Schiele
         > Project release []: 1.0
         > Project language [en]:
         > Source file suffix [.rst]:
         > Name of your master document (without suffix) [index]:
         > autodoc: automatically insert docstrings from modules (y/n) [n]: y
         > doctest: automatically test code snippets in doctest blocks (y/n) [n]: y
         > intersphinx: link between Sphinx documentation of different projects (y/n) [n]: y
         > todo: write "todo" entries that can be shown or hidden on build (y/n) [n]: y
         > coverage: checks for documentation coverage (y/n) [n]:
         > imgmath: include math, rendered as PNG or SVG images (y/n) [n]:
         > mathjax: include math, rendered in the browser by MathJax (y/n) [n]:
         > ifconfig: conditional inclusion of content based on config values (y/n) [n]:
         > viewcode: include links to the source code of documented Python objects (y/n) [n]: y
         > githubpages: create .nojekyll file to publish the document on GitHub pages (y/n) [n]:
         > Create Makefile? (y/n) [y]:
         > Create Windows command file? (y/n) [y]:

         Creating file docs\conf.py.
         Creating file docs\index.rst.
         Creating file docs\Makefile.
         Creating file docs\make.bat.

Sphinx-Layout
-------------

::

    .
    └── docs
        ├── Makefile
        ├── _static
        ├── _templates
        ├── conf.py
        ├── index.rst
        └── make.bat

``index.rst`` ist die Ausgangsdatei für die Dokumentation, in der sich das
Inhaltsverzeichnis befindet. Das Inhaltsverzeichnis kann von euch erweitert
werden, sobald ihr neue ``*.rst``-Dateien hinzufügt.

Generiert die Dokumentation
---------------------------

Ihr könnt die Dokumentation nun generieren, :abbr:`z.B. (zum Beispiel)` mit:

.. tab:: Linux/macOS

   .. code-block:: console

      $ sphinx-build -ab html docs/ docs/_build

.. tab:: Windows

   .. code-block:: ps1con

      C:> sphinx-build -ab html docs\ docs\_build

``a``
    generiert alle Seiten der Dokumentation neu.

    .. note::
       Dies ist immer dann sinnvoll, wenn ihr eurer Dokumentation neue Seiten
       hinzugefügt habt.

``b``
    gibt an, welcher Builder zum Generieren der Dokumentation verwendet werden
    soll. In unserem Beispiel ist dies ``html``.
