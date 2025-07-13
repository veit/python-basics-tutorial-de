Schnelleinstieg
===============

Status
------

.. image:: https://img.shields.io/github/contributors/veit/python-basics-tutorial-de.svg
   :alt: Contributors
   :target: https://github.com/veit/python-basics-tutorial-de/graphs/contributors
.. image:: https://img.shields.io/github/license/veit/python-basics-tutorial-de.svg
   :alt: License
   :target: https://github.com/veit/python-basics-tutorial-de/blob/main/LICENSE
.. image:: https://readthedocs.org/projects/python-basics-tutorial-de/badge/?version=latest
   :alt: Docs
   :target: https://python-basics-tutorial.readthedocs.io/de/latest/

Installation
------------

#. Herunterladen und auspacken:

   … auf Linux/macOS:

   .. code-block:: console

    $ curl -O https://codeload.github.com/veit/python-basics-tutorial-de/zip/main
    $ unzip main
    Archive:  main
    …
       creating: python-basics-tutorial-de-main/
    …

   … auf Windows:

   .. code-block:: ps1con

    C:> curl.exe -o main.zip -O https://codeload.github.com/veit/python-basics-tutorial-de/zip/main
    C:> tar -xvzf main.zip
    python-basics-tutorial-de-main/
    python-basics-tutorial-de-main/.gitignore
    …

#. Installieren von Python-Paketen:

   … auf Linux/macOS:

   .. code-block:: console

      $ python3 -m venv .venv
      $ . .venv/bin/activate
      $ python -m pip install --upgrade pip
      $ python -m pip install --group=dev

   … auf Windows:

   .. code-block:: ps1con

      C:> py -m venv .venv
      C:> .\.venv\Scripts\activate.bat
      C:> python -m pip install --upgrade pip
      C:> python -m pip install --group=dev

#. Erstellen der HTML-Dokumentation:

   .. note::
      pandoc und plantuml müssen installiert sein.

      … auf Debian/Ubuntu:

      .. code-block:: console

         $  sudo apt install pandoc plantuml

   Zum Erstellen der HTML-Dokumentation führt den folgenden Befehl aus:

   .. code-block:: console

      $ sphinx-build -ab html docs/ docs/_build/html/

#. Erstellen eines PDF:

   Zum Erstellen einer PDF-Dokumentation benötigt ihr zusätzliche Pakete, die
   ihr installieren könnt

   … auf Debian/Ubuntu mit

   .. code-block:: console

      $ sudo apt install texlive-latex-recommended texlive-latex-extra texlive-fonts-recommended latexmk

   … auf macOS mit

   .. code-block:: console

      $ brew cask install mactex
      …
      🍺  mactex was successfully installed!
      $ curl --remote-name https://www.tug.org/fonts/getnonfreefonts/install-getnonfreefonts
      $ sudo texlua install-getnonfreefonts
      …
      mktexlsr: Updating /usr/local/texlive/2020/texmf-dist/ls-R...
      mktexlsr: Done.

   Anschließend könnt ihr ein PDF generieren mit:

   .. code-block:: console

    $ cd docs/
    $ make latexpdf
    …
    The LaTeX files are in _build/latex.
    Run 'make' in that directory to run these through (pdf)latex
    …

   Das PDF findet ihr dann in ``docs/_build/latex/pythonbasics.pdf``.

Folgt uns
---------

.. _follow-us:

* `GitHub <https://github.com/veit/python-basics-tutorial-de>`_
* `Mastodon <https://mastodon.social/@Python4DataScience>`_
* `Bluesky <https://bsky.app/profile/python4data.science>`_

.. _end-follow-us:

Pull-Requests
-------------

Wenn ihr Vorschläge für Verbesserungen und Ergänzungen habt, empfehle ich euch,
einen `Fork <https://github.com/veit/python-basics-tutorial-de/fork>`_ meines
`GitHub Repository <https://github.com/veit/python-basics-tutorial-de/>`_ zu
machen und eure Änderungen hier zu machen. Ihr könnt auch gerne einen  *pull
request* stellen. Wenn die dort enthaltenen Änderungen klein und atomar sind,
werde ich mir eure Vorschläge gerne anschauen.
