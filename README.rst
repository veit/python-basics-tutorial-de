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
.. image:: https://pyup.io/repos/github/veit/python-basics-tutorial-de/shield.svg
   :alt: Pyup
   :target: https://pyup.io/repos/github/veit/python-basics-tutorial-de/

Installation
------------

#. Herunterladen und auspacken:

   .. code-block:: console

    $ curl -O https://codeload.github.com/veit/python-basics-tutorial/zip/main
    $ unzip main
    Archive:  main
    …
       creating: python-basics-tutorial-main/
    …

#. Installieren von Python-Paketen:

   .. code-block:: console

    $ python3 -m venv .
    $ source bin/activate
    $ python -m pip install --upgrade pip
    $ python -m pip install -r docs/requirements.txt

#. Erstellen der HTML-Dokumentation:

   Beachtet, dass pandoc installiert sein muss. Auf Debian/Ubuntu könnt ihr
   hierfür folgendes ausführen:

   .. code-block:: console

    $  sudo apt-get install pandoc

   Zum Erstellen der HTML-Dokumentation führt den folgenden Befehl aus:

   .. code-block:: console

    $ sphinx-build -ab html docs/ docs/_build/

#. Erstellen eines PDF:

   Zum Erstellen einer PDF-Dokumentation benötigt ihr zusätzliche Pakete.

   Auf Debian/Ubuntu erhaltet ihr diese mit:

   .. code-block:: console

    $ sudo apt-get install texlive-latex-recommended texlive-latex-extra texlive-fonts-recommended latexmk

   … oder auf macOS mit:

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

* `GitHub <https://github.com/veit/python-basics-tutorial-de>`_

Pull-Requests
-------------

Wenn ihr Vorschläge für Verbesserungen und Ergänzungen habt, empfehle ich euch,
einen `Fork <https://github.com/veit/python-basics-tutorial-de/fork>`_ meines
`GitHub Repository <https://github.com/veit/python-basics-tutorial-de/>`_ zu
machen und eure Änderungen hier zu machen. Ihr könnt auch gerne einen  *pull
request* stellen. Wenn die dort enthaltenen Änderungen klein und atomar sind,
werde ich mir eure Vorschläge gerne anschauen.
