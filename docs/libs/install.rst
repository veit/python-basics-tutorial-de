Hinzufügen weiterer Python-Bibliotheken
=======================================

Obwohl Pythons :doc:`batteries`-Philosophie bedeutet, dass ihr mit der
Standardinstallation von Python bereits eine Menge machen könnt, wird
unweigerlich die Situation kommen, in der ihr eine Funktionalität benötigt,
die nicht in Python enthalten ist.

Wenn ihr ein Modul eines Drittanbieters benötigt, das nicht für eure Plattform
vorgefertigt ist, müsst ihr dessen Quell-Distribution verwenden. Dies bringt
jedoch zwei Probleme mit sich:

#. Um die Quell-Distribution zu installieren, müsst ihr sie finden und
   herunterladen.
#. Es werden bestimmte Python-Pfade und Berechtigungen eures Systems erwartet.

Python bietet :ref:`pip` als aktuelle Lösung für beide Probleme an. ``pip``
versucht, das Modul im :term:`Python Package Index` (:term:`PyPI`) zu finden,
lädt es und alle Abhängigkeiten herunter und kümmert sich um die Installation.
Ihr könnt auch :term:`pypi.org` direkt aufrufen und nach Paketen zu suchen oder
die Pakete nach Kategorien filtern.

.. warning::
   Installiert niemals irgendetwas mit ``pip`` in das globale Python, auch nicht
   mit dem ``--user`` Flag. Verwendet immer :ref:`venv`. So vermeidet ihr, dass
   eure Python-Installation mit Bibliotheken verunreinigt wird, die ihr
   installiert und dann vergesst. Jedes Mal, wenn ihr etwas Neues machen müsst,
   solltet ihr eine neue virtuelle Umgebung erstellen. Damit vermeidet ihr auch
   Bibliothekskonflikte zwischen verschiedenen Projekten.

.. tip::
   wir empfehlen euch, ``pip`` so zu konfigurieren, dass es nicht möglich ist,
   Python-Pakete global zu installieren. Hierfür könnt ihr folgendes in eure
   :file:`~/.config/pip/pip.conf` eintragen:

   .. code-block:: ini

      [global]
      require-virtualenv = true

.. _venv:

``venv``
--------

Eine *virtuelle Umgebung* (``virtualenv``) ist eine in sich geschlossene
Verzeichnisstruktur, die sowohl eine Installation von Python als auch die
zusätzlichen Pakete enthält. Da die gesamte Python-Umgebung in diesem
Verzeichnis enthalten ist, können die dort installierten Bibliotheken und Module
nicht mit denen im Hauptsystem oder in anderen virtuellen Umgebungen
kollidieren, so dass verschiedene Anwendungen unterschiedliche Versionen von
Python und seinen Paketen verwenden können. Das Erstellen und Verwenden einer
virtuellen Umgebung erfolgt in zwei Schritten:

#. Zuerst erstellen wir ein Projektverzeichnis und dann darin die virtuelle
   Umgebung:

   .. tab:: Linux/macOS

      .. code-block:: console

         $ mkdir myproj
         $ cd myproj
         $ python3 -m venv .venv

   .. tab:: Windows

      .. code-block:: ps1

         > mkdir myproj
         > cd myproj
         > py -m venv .venv

   Hiermit wird die Umgebung mit Python und :term:`pip` in einem Verzeichnis
   namens :samp:`.venv` erstellt.

#. Anschließend könnt ihr diese Umgebung aktivieren, sodass beim nächsten Aufruf
   von ``python`` das Python aus eurer neuen Umgebung verwendet wird:

   .. tab:: Linux/macOS

      .. code-block:: console

         $ . .venv/bin/activate

   .. tab:: Windows

      .. code-block:: ps1

         > .venv\Scripts\activate

#. Python-Pakete nur für diese virtuelle Umgebung installieren, :abbr:`z.B. (zum
   Beispiel)` die beliebte ``pandas``-Bibliothek:

   .. tab:: Linux/macOS

      .. code-block:: console

         (.venv) $ python -m pip install pandas

   .. tab:: Windows

      .. code-block:: ps1

         (.venv) > python.exe -m pip install pandas

#. Wenn ihr eure Arbeit an diesem Projekt beenden wollt, könnt ihr die virtuelle
   Umgebung wieder deaktivieren mit

   .. tab:: Linux/macOS

      .. code-block:: console

         (.venv) $ deactivate

   .. tab:: Windows

      .. code-block:: ps1

         (.venv) > deactivate

.. seealso::
   * :doc:`python3:tutorial/venv`

.. _pip:

``pip``
-------

Die grundlegende Syntax von ``pip`` ist recht einfach:

.. code-block:: console

   (.venv) $ python -m pip install pandas

Wenn ihr eine bestimmte Version eines Pakets angeben wollt, könnt ihr die
Versionsnummern einfach anhängen:

.. code-block:: console

   (.venv) $ python -m pip install pandas==2.2.2

oder

.. code-block:: console

   (.venv) $ python -m pip install "pandas>=2"

Proxy-Server
~~~~~~~~~~~~

Um Python-Pakete über einen Proxy-Server zu installieren, könnt ihr folgendes
eingeben: :samp:`python -m pip install --proxy
http://{USER_NAME}{:{PASSWORD}}@{PROXYSERVER_NAME}:{PORT} {PKG_NAME}`

Ihr könnt den Proxy-Server auch dauerhaft als Umgebungsvariable speichern:

.. tab:: Linux

   :abbr:`z.B. (zum Beispiel)` in der :file:`~/.bashrc` mit:

   .. code-block:: bash

      $ export HTTP_PROXY=http://{USER_NAME}:{PASSWORD}@{PROXYSERVER_NAME}:{PORT}

.. tab:: Windows

   Fügt die folgende Zeile den Umgebungsvariablen hinzu:

   .. code-block:: ps1

      > set HTTP_PROXY={PROXYSERVER_NAME}:{PORT}

Festschreiben …
~~~~~~~~~~~~~~~

… von Python
::::::::::::

Im Gegensatz zu Anwendungen unterstützen unsere Pakete normalerweise mehr als
eine Python-Version. Dennoch fügen wir auch bei :doc:`Paketen
<../packs/distribution>` üblicherweise die aktuelle Standard-Version in
:file:`.python-version` hinzu:

.. literalinclude:: ../../.python-version
   :caption: .python-version

Das Schöne daran ist, dass wir die gleiche Datei in GitHub Actions als Eingabe
für `setup-python <https://github.com/actions/setup-python>`_ verwenden können:

.. literalinclude:: ../../.github/workflows/ci.yml
   :caption: .github/workflows/ci.yml
   :lines: 14, 29-35, 37-40
   :emphasize-lines: 12

In unseren
:doc:`Python4DataScience:productive/git/advanced/gitlab/ci-cd/index`-Pipelines
verwenden wir jedoch ``requires-python`` aus der :ref:`pyproject-toml`-Datei, um
:doc:`Docker-Container mit der passenden Python-Version
<Python4DataScience:productive/git/advanced/gitlab/ci-cd/docker>` zu bauen.

… von Paketen
:::::::::::::

Für eine stabile Umgebung ist es sinnvoll, die exakten Varianten der
Abhängigkeiten festzuschreiben.

.. tip::
   In keinem unserer Bibliotheksprojekte passiert so viel, dass die
   :doc:`Git-Historie <Python4DataScience:productive/git/review>` vorwiegend aus
   Updates bestehen sollte. Lediglich bei Problemen schränken wir dort die zu
   verwendenden Versionsnummern ein. Bei Anwendungen (:abbr:`engl. (englisch)`:
   Apps) schreiben wir die Versionsnummern jedoch fest.

Um für unsere Anwendungen die exakte Variante festzuschreiben und
plattformübergreifende Lock-Dateien zu erhalten, verwenden wir üblicherweise
:ref:`uv`. Zudem unterstützt uns ``uv`` bei :ref:`reproduzierbaren
Python-Umgebungen <reproduce-virtual-env>`.

Pakete können jedoch auch mit pip≥26.1 festgeschrieben werden:

.. code-block::

   $ python -m pip install --upgrade pip
   $ python -m pip --version

:pep:`751` legte das Format für die :file:`pylock.toml`-Datei fest, die mit
folgendem Aufruf erzeugt werden kann, :abbr:`z. B. (zum Beispiel)` mit:

.. code-block:: console

   $ python -m pip lock -e . -o pylock.prod.toml

Alternativ kann auch aus einer :file:`requirements.txt`-Datei eine
:file:`pylock.toml`-Datei generiert werden:

.. code-block:: console

   $ python -m pip lock -r requirements.txt -o pylock.prod.toml

.. warning::
   In beiden Fällen werden jedoch nur die Pakete für die aktuelle Plattform- und
   Python-Version festgeschrieben.

.. tip::
   Wenn ``pip`` nicht das einzige verfügbare Tool ist, stellt die
   plattformübergreifende Ausgabe von ``uv export`` den reibungsloseren Weg dar.

Anschließend können die in der :samp:`pylock{.NAME}.toml` festgeschriebenen
Pakete in einer anderen Umgebung installiert werden mit:

.. code-block:: console

   $ python -m pip install -r pylock.prod.toml --no-deps

``--no-deps``
    sorgt dafür, dass auch die transitiven Abhängigkeiten nicht zusätzlich zur
    :samp:`pylock{.NAME}.toml`-Datei aufgelöst werden.

.. seealso::
   * `pip lock <https://pip.pypa.io/en/stable/cli/pip_lock/>`_
   * `pylock.toml Specification
     <https://packaging.python.org/en/latest/specifications/pylock-toml/>`_

.. _uv:

``uv``
------

:term:`uv` vereinfacht das Erstellen einer initialen Projektstruktur und die
Verwaltung eurer Abhängigkeiten.

.. note::
   Viele Coding-Agenten verwenden üblicherweise ``pip``, wenn Pakete installiert
   oder Skripte ausgeführt werden sollen. Daher müssen wir sie zunächst so
   konfigurieren, dass sie ``uv`` verwenden:

   .. code-block:: md
      :caption: AGENTS.md

      - Use `uv` to manage Python environments and dependencies.
      - Use `uv run` to execute Python scripts and commands.
      - Don't edit `pyproject.toml` directly. Instead, use `uv add` and `uv add --dev` to manage dependencies.

   .. seealso::
      * :ref:`agentic-software-engineering:uv`

Installation
~~~~~~~~~~~~

``uv`` hängt nicht von Python ab. Vorkompilierte, eigenständige Binärdateien
können auf Linux, macOS und Windows installiert werden:

.. tab:: Linux/macOS

   .. code-block:: console

      $ curl -LsSf https://astral.sh/uv/install.sh | sh

.. tab:: Windows

   .. code-block:: ps1

      > powershell -c "irm https://astral.sh/uv/install.ps1 | iex"

``uv`` aktualisiert sich bei dieser Installation regelmäßig selbst.

Automatische Shell-Vervollständigung
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Um die automatische Shell-Vervollständigung für ``uv``-Befehle zu aktivieren,
führt einen der folgenden Schritte aus:

.. tab:: Linux/macOS

   Bestimmt eure Shell, :abbr:`z.B. (zum Beispiel)` mit ``echo $SHELL``, dann
   führt einen der folgenden Befehle aus:

   .. code-block:: console

      $ echo 'eval "$(uv generate-shell-completion bash)"' >> ~/.bashrc
      $ echo 'eval "$(uv generate-shell-completion zsh)"' >> ~/.zshrc
      $ echo 'uv generate-shell-completion fish | source' >> ~/.config/fish/config.fish
      $ echo 'eval (uv generate-shell-completion elvish | slurp)' >> ~/.elvish/rc.elv

.. tab:: Windows

   .. code-block:: ps1

      Add-Content -Path $PROFILE -Value '(& uv generate-shell-completion powershell) | Out-String | Invoke-Expression'
      Add-Content -Path $PROFILE -Value '(& uvx --generate-shell-completion powershell) | Out-String | Invoke-Expression'

Startet dann die Shell neu oder ruft ``source`` mit eurer
Shell-Konfigurationsdatei ein.

Update
~~~~~~

Ihr könnt uv ganz einfach aktualisieren mit:

.. code:: console

   $ uv self update
   info: Checking for updates...
   success: Upgraded uv from v0.8.12 to v0.8.13! https://github.com/astral-sh/uv/releases/tag/0.8.13

Python-Installation
~~~~~~~~~~~~~~~~~~~

Mit ``uv python install`` lässt sich die aktuelle Python-Version installieren.
Alternativ lässt sich auch eine spezifische Version installieren mit :samp:`uv
python install {3.14}`. Es lassen sich jedoch nicht nur ältere CPython-Versionen
installieren, sondern :abbr:`z.B. (zum Beispiel)` auch `PyPy
<https://pypy.org>`_ mit :samp:`uv python install pypy@{3.12}` oder
Free-threaded Python mit :samp:`uv python install --python-preference
only-managed {3.14t}`. Die bereits installierten Python-Versionen erhaltet ihr
mit ``uv python list``. Eine installierte Python-Version könnt ihr aufrufen mit
:samp:`uv run --python {3.14} python`.

Projektstruktur erstellen
~~~~~~~~~~~~~~~~~~~~~~~~~

Je nachdem, ob ihr eine :doc:`Bibliothek <../packs/distribution>` oder
:doc:`Anwendung <../packs/apps>` erstellen wollt, kann ``uv`` eine passende
Projektstruktur erstellen.

Abhängigkeiten installieren
~~~~~~~~~~~~~~~~~~~~~~~~~~~

Mit ``uv sync --frozen`` könnt ihr die Abhängigkeiten eures Projekts
installieren in den in der :file:`uv.lock`-Datei exakt festgeschriebenen
Varianten.

.. seealso::
   * :ref:`uv_lock`
   * :ref:`reproduce-virtual-env`

Mit :samp:`uv pip install --pylock pylock{.NAME}.toml` könnt ihr die
Abhängigkeiten auch von einer bestehenden :samp:`pylock{.NAME}.toml`-Datei
installieren.

Abhängigkeiten hinzufügen
~~~~~~~~~~~~~~~~~~~~~~~~~

Mit :samp:`uv add {PACKAGE}` könnt ihr eurem Projekt weitere Abhängigkeiten
hinzufügen. Dabei wird das hinzugefügte Paket sowohl im
``dependencies``-Abschnitt der :file:`pyproject.toml`-Datei hinzugefügt, wie
auch die exakte Vatiante in die :file:`uv.lock`-Datei geschrieben.
