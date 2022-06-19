shot-scraper
============

`shot-scraper <https://simonwillison.net/2022/Mar/10/shot-scraper/>`_ ist ein
Werkzeug, mit dem sich der Prozess der Aktualisierung von Screenshots
automatisieren lässt. 

Installation
------------

.. code-block:: console

   $ pipenv install shot-scraper
   $ shot-scraper install

.. note::
   Die zweite Zeile installiert den benötigten Browser.

Verwendung
----------

shot-scraper kann auf zweierleis Art verwendet werden

#. …für einzelne Screenshots auf der Kommandozeile:

   .. code-block:: console

        $  shot-scraper https://jupyter-tutorial.readthedocs.io/de/latest/clean-prep/index.html -o ~/Downloads/clean-prep.png

   …oder mit zusätzlichen Optionen, z.B. für JavaScript- und CSS-Selektoren:

    .. code-block::

        $ pipenv run shot-scraper https://jupyter-tutorial.readthedocs.io/de/latest/clean-prep/index.html -s '#overview' -o ~/Downloads/clean-prep.png

#. …für eine Reihe von Screenshots, die in einer YAML-Datei konfiguriert sind:

   .. code-block:: yaml

        - url: https://jupyter-tutorial.readthedocs.io/de/latest/clean-prep/index.html
          output: ~/Downloads/clean-prep.png
        - url: https://www.example.org/
          width: 736
          quality: 40
          output: example.jpg

   Anschließend kann ``shot-scraper multi`` verwendet werden, z.B.:

   .. code-block:: console

        $ pipenv run shot-scraper multi shots.yaml
        Screenshot of 'https://jupyter-tutorial.readthedocs.io/de/latest/clean-prep/index.html' written to '~(Downloads/clean-prep.png'
        Screenshot of 'https://www.example.org/' written to 'example.jpg'

   .. seealso::
      * In der `README.md
        <https://github.com/simonw/shot-scraper/blob/main/README.md>`_-Datei
        findet ihr eine vollständige Übersicht über die möglichen Optionen.
      * Im shot-scraper-demo-Repository findet ihr eine deutlich umfangreichere
        `shots.yaml
        <https://github.com/simonw/shot-scraper-demo/blob/main/.github/workflows/shots.yml>`_-Datei.

GitHub-Actions
--------------

shot-scraper lässt sich einfach in GitHub Actions einbinden. Im
shot-scraper-demo-Repository findet sich auch eine examplarische `shots.yml
<https://github.com/simonw/shot-scraper-demo/blob/main/.github/workflows/shots.yml>`_.
Einmal am Tag werden zwei Screenshots erzeugt und zurück in das Repository
übertragen. Beachtet jedoch, dass das Speichern von Bilddateien, die sich häufig
ändern, die Revisionshistorie sehr unleserlich machen können. Daher solltet ihr
shot-scraper mit Bedacht zusammen mit GitHub Actions verwenden.
