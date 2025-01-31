Installation
============

Voraussetzungen
---------------

* Python-Interpreter

* Pfad zum Basisverzeichnis für euere Python-Pakete

  Stellt sicher, dass sich euer ``bin``-Verzeichnis im Pfad  befindet. In der
  Regel ist dies ``~/.local/`` für Linux und macOS oder ``%APPDATA%\Python``
  unter Windows. Weitere Infos findet ihr in `site.USER_BASE
  <https://docs.python.org/3/library/site.html#site.USER_BASE>`_.

  .. tab:: Linux/macOS

     Für Bash könnt ihr den Pfad in eurer ``~/.bash_profile`` angeben:

     .. code-block:: bash

        export PATH=$HOME/.local/bin:$PATH

     und anschließend die Datei einlesen mit:

     .. code-block:: console

        $ source ~/.bash_profile

  .. tab:: Windows

     Stellt sicher, dass das Verzeichnis, in dem Cookiecutter installiert wird,
     sich in eurem ``Path`` befindet, damit ihr es direkt aufrufen könnt. Sucht
     dazu auf eurem Computer nach *Environment Variables* und fügt dieses
     Verzeichnis zu ``Path`` hinzu, also :abbr:`z.B.(zum Beispiel)`
     ``%APPDATA%\Python\Python3x\Scripts``. Anschließend müsst ihr vermutlich
     die Session neu starten um die Umgebungsvariablen nutzen zu können.

     .. seealso::
        `Configuring Python
        <https://docs.python.org/3/using/windows.html#configuring-python>`_

Installation
------------

.. code-block:: console

    $ python3 -m venv cookiecutter_env
    $ cd !$
    cd cookiecutter_env
    $ . bin/activate
    $ python -m pip install cookiecutter
