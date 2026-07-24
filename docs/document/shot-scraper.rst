shot-scraper
============

`shot-scraper <https://shot-scraper.datasette.io/en/stable/>`_ ist ein Werkzeug,
mit dem sich die Aktualisierung von Screenshots automatisieren lässt.

Installation
------------

.. code-block:: console

   $ uv add --group docs shot-scraper
   $ uv run shot-scraper install

.. note::
   Die zweite Zeile installiert den benötigten Browser.

.. seealso::
   `shot-scraper Installation
   <https://shot-scraper.datasette.io/en/stable/installation.html>`_

Verwendung
----------

shot-scraper kann auf mehrere Arten verwendet werden

#. …für einzelne Screenshots auf der Kommandozeile:

   .. code-block:: console

      $ uv run shot-scraper https://jupyter-tutorial.readthedocs.io/de/latest/clean-prep/index.html -o ~/Downloads/clean-prep.png

   …oder mit zusätzlichen Optionen, :abbr:`z. B. (zum Beispiel)` für
   JavaScript- und CSS-Selektoren:

   .. code-block:: console

      $ uv run shot-scraper https://jupyter-tutorial.readthedocs.io/de/latest/clean-prep/index.html -s '#overview' -o ~/Downloads/clean-prep.png

#. …für eine Reihe von Screenshots, die in einer YAML-Datei konfiguriert sind:

   .. code-block:: yaml

      - url: https://jupyter-tutorial.readthedocs.io/de/latest/clean-prep/index.html
        output: ~/Downloads/clean-prep.png
      - url: https://www.example.org/
        width: 736
        quality: 40
        output: example.jpg

   Anschließend kann ``shot-scraper multi`` verwendet werden, :abbr:`z. B. (zum
   Beispiel)`:

   .. code-block:: console

      $ shot-scraper multi shots.yaml
      Screenshot of 'https://jupyter-tutorial.readthedocs.io/de/latest/clean-prep/index.html' written to '~(Downloads/clean-prep.png'
      Screenshot of 'https://www.example.org/' written to 'example.jpg'

   .. seealso::
      * `Taking multiple screenshots
        <https://shot-scraper.datasette.io/en/stable/multi.html>`_
      * Im shot-scraper-demo-Repository findet ihr eine deutlich umfangreichere
        `shots.yaml
        <https://github.com/simonw/shot-scraper-demo/blob/main/.github/workflows/shots.yml>`_-Datei.

#. …für Videos:

   Der ``shot-scraper video``-Befehl nimmt ein WebM-Video anhand eines
   :doc:`Python4DataScience:data-processing/serialisation-formats/yaml/index`-Storyboards
   auf. Storyboards beschreiben das Video als Abfolge von Szenen. Jede Szene
   kann eine Seite öffnen, auf Inhalte warten, Aktionen ausführen und zwischen
   den Schritten eine Pause einlegen, :abbr:`z. B. (zum Beispiel)`:

   .. literalinclude:: storyboard.yml
      :caption: storyboard.yml
      :language: yaml

   Führt anschließend folgenden Befehl aus:

   .. code-block:: console

      $ uv run shot-scraper video storyboard.yml

   Dadurch wird ``url`` geöffnet, die Szenen werden aufgezeichnet und das Video
   wird unter dem Namen :file:`demo.webm` gespeichert.

   Sofern `FFmpeg <https://www.ffmpeg.org/>`_ installiert ist, könnt ihr mit der
   Option ``--mp4`` das aufgezeichnete WebM-Video zusätzlich in MP4
   konvertieren.

   .. seealso::
      * `Recording videos
        <https://shot-scraper.datasette.io/en/stable/video.html>`_

GitHub-Actions
--------------

shot-scraper lässt sich einfach in GitHub Actions einbinden. Im
shot-scraper-demo-Repository findet sich auch eine exemplarische `shots.yml
<https://github.com/simonw/shot-scraper-demo/blob/main/.github/workflows/shots.yml>`_.
Einmal am Tag werden zwei Screenshots erzeugt und zurück in das Repository
übertragen. Beachtet jedoch, dass das Speichern von Bilddateien, die sich häufig
ändern, die Revisionshistorie sehr unleserlich machen können. Daher solltet ihr
shot-scraper mit Bedacht zusammen mit GitHub Actions verwenden.
