Hinzufügen weiterer Python-Bibliotheken
=======================================

Obwohl Pythons :doc:`batteries`-Philosophie bedeutet, dass ihr mit der
Standardinstallation von Python bereits eine Menge machen könnt, wird
unweigerlich die Situation kommen, in der ihr eine Funktionalität benötigt,
die nicht in Python enthalten ist.

Wenn ihr ein Modul eines Drittanbieters benötigt, das nicht für eure Plattform
vorgefertigt ist, müsst ihr dessen Quelldistribution verwenden. Dies bringt
jedoch zwei Probleme mit sich:

#. Um die Quelldistribution zu installieren, müsst ihr sie finden und
   herunterladen.
#. Es werden bestimmte Python-Pfade und Berechtigungen eures Systems erwartet.

Python bietet :ref:`pip` als aktuelle Lösung für beide Probleme an. ``pip``
versucht, das Modul im :term:`Python Package Index` (:term:`PyPI`) zu finden,
lädt es und alle Abhängigkeiten herunter und kümmert sich um die Installation.
Ihr könnt auch :term:`pypi.org` direkt aufrufen und nach Paketen zu suchen oder
die Pakete nach Kategorien filtern.

.. warning::
   Installiert niemals irgendetwas mit ``pip`` in das globale Python, auch nicht
   mit dem ``--user`` Flag. Verwendet immer :ref:`virtuelle-umgebungen`. So
   vermeidet ihr, dass eure Python-Installation mit Bibliotheken verunreinigt
   wird, die ihr installiert und dann vergesst. Jedes Mal, wenn ihr etwas Neues
   machen müsst, solltet ihr eine neue virtuelle Umgebung erstellen. Damit
   vermeidet ihr auch Bibliothekskonflikte zwischen verschiedenen Projekten.

.. tip::
   Ich empfehle euch, ``pip`` so zu konfigurieren, dass es nicht möglich ist,
   Python-Pakete global zu installieren. Hierfür könnt ihr folgendes in eure
   :file:`~/.config/pip/pip.conf` eintragen:

   .. code-block:: ini

      [global]
      require-virtualenv = true

.. _virtuelle-umgebungen:

Virtuelle Umgebungen
--------------------

Eine *virtuelle Umgebung* (``virtualenv``) ist eine in sich geschlossene
Verzeichnisstruktur, die sowohl eine Installation von Python als auch die
zusätzlichen Pakete enthält. Da die gesamte Python-Umgebung in diesem
Verzeichnis enthalten ist, können die dort installierten Bibliotheken und Module
nicht mit denen im Hauptsystem oder in anderen virtuellen Umgebungen
kollidieren, so dass verschiedene Anwendungen unterschiedliche Versionen von
Python und seinen Paketen verwenden können. Das Erstellen und Verwenden einer
virtuellen Umgebung erfolgt in zwei Schritten:

#. Zuerst erstellen wir die Umgebung:

   .. tab:: Linux/macOS

      .. code-block:: console

         $ python3 -m venv myenv

   .. tab:: Windows

      .. code-block:: ps1

         > py -m venv myenv

   Hiermit wird die Umgebung mit Python und :term:`pip` in einem Verzeichnis
   namens :samp:`myenv` erstellt.

#. Anschließend könnt ihr diese Umgebung aktivieren, sodass beim nächsten Aufruf
   von ``python`` das Python aus eurer neuen Umgebung verwendet wird:

   .. tab:: Linux/macOS

      .. code-block:: console

         $ . myenv/bin/activate

   .. tab:: Windows

      .. code-block:: ps1

         > myenv\Scripts\activate.bat

#. Python-Pakete nur für diese virtuelle Umgebung installieren, :abbr:`z.B. (zum
Beispiel)` die beliebte ``pandas``-Bibliothek:

   .. tab:: Linux/macOS

      .. code-block:: console

         (myenv) $ python -m pip install pandas

   .. tab:: Windows

      .. code-block:: ps1

         (myenv) > python.exe -m pip install pandas

#. Wenn ihr eure Arbeit an diesem Projekt beenden wollt, könnt ihr die virtuelle
   Umgebung wieder deaktivieren mit

   .. tab:: Linux/macOS

      .. code-block:: console

         (myenv) $ deactivate

   .. tab:: Windows

      .. code-block:: ps1

         (myenv) > deactivate

.. seealso::
   * :doc:`python3:tutorial/venv`

.. _pip:

``pip``
-------

Die grundlegende Syntax von ``pip`` ist recht einfach:

.. code-block:: console

    $ python -m pip install pandas

Wenn ihr eine bestimmte Version eines Pakets angeben wollt, könnt ihr die
Versionsnummern einfach anhängen:

.. code-block:: console

    $ python -m pip install pandas==2.2.2

oder

.. code-block:: console

    $ python -m pip install "pandas>=2"

Proxy-Server
~~~~~~~~~~~~

Um Python-Pakete über einen Proxy-Server zu installieren, könnt ihr folgendes
eingeben: :samp:`python -m pip install --proxy
http://{USER_NAME}{:{PASSWORD}}@{PROXYSERVER_NAME}:{PORT} {PKG_NAME}`

Ihr könnt den Proxy-Server auch dauerhaft als Umgebungsvariable speichern:

.. tab:: Linux

   :abbr:`z.B. (zum Beispiel)` in der :file:`~/.bashrc` mit:

   .. code-block:: bash

      export HTTP_PROXY=http://{USER_NAME}:{PASSWORD}@{PROXYSERVER_NAME}:{PORT}

.. tab:: Windows

   Fügt die folgende Zeile den Umgebungsvariablen hinzu:

   .. code-block:: ps1

      set HTTP_PROXY={PROXYSERVER_NAME}:{PORT}

Festschreiben der Versionsnummern
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

… von Paketen
:::::::::::::

Für eine stabile Umgebung ist es sinnvoll, die Versionsnummern der
Abhängigkeiten festzuschreiben.

.. tip::
   In keinem meiner Bibliotheksprojekte passiert so viel, dass die
   :doc:`Git-History <Python4DataScience:productive/git/review>` vorwiegend aus
   Updates bestehen sollte. Lediglich bei Problemen schränke ich dort die zu
   verwendenden Versionsnummern ein. Lediglich bei Anwendungen (:abbr:`engl.
   (englisch)`: Apps) schreibe ich die Versionsnummern fest.

Um für unsere Anwendungen die Versionen festzuschreiben und
plattformübergreifende Lock-Dateien zu erhalten, verwenden wir `PDM
<https://pdm-project.org/en/latest>`_. Zudem unterstützt PDM die Verwaltung
virtueller Umgebungen mit ``pdm venv activate``.

… von Python
::::::::::::

Im Gegensatz zu Anwendungen unterstützen meine Pakete normalerweise mehr als
eine Python-Version. Dennoch füge ich auch bei :doc:`Paketen <distribution>`
üblicherweise die aktuelle Standard-Version in :file:`.python-version` hinzu:

.. literalinclude:: ../../.python-version
   :caption: .python-version

Das Schöne daran ist, dass ich die gleiche Datei in GitHub Actions als Eingabe
für `setup-python <https://github.com/actions/setup-python>`_ verwenden kann:

.. literalinclude:: ../../.github/workflows/ci.yml
   :caption: .github/workflows/ci.yml
   :lines: 20-29
   :emphasize-lines: 10

In unseren
:doc:`Python4DataScience:productive/git/advanced/gitlab/ci-cd`-Pipelines
verwenden wir jedoch ``requires-python`` aus der :ref:`pyproject-toml`-Datei, um
:doc:`Docker-Container mit der passenden Python-Version
<Python4DataScience:productive/git/advanced/gitlab/docker>` zu bauen.
