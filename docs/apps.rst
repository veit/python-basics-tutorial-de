Apps
====

App-Projekte sind für Webserver, Skripte und Befehlszeilenschnittstellen
(:abbr:`CLI (engl.: Command Line Interface)`) geeignet. Auch sie können wir mit
``uv init --package`` erstellen:

.. code-block:: console

   $ uv init --package myapp
   tree myapp -a
   myapp
   ├── .git
   │   └── ...
   ├── .gitignore
   ├── .python-version
   ├── README.md
   ├── pyproject.toml
   └── src
       └── myapp
           └── __init__.py

:file:`myapp/pyproject.toml`
    Die Datei :file:`pyproject.toml` enthält einen ``scripts``-Einstiegspunkt
    ``myapp:main``:

    .. literalinclude:: myapp/pyproject.toml
       :caption: myapp/pyproject.toml
       :lines: 12-13

:file:`myapp/src/myapp/__init__.py`
    Das Modul definiert eine CLI-Funktion :func:`main`:

    .. literalinclude:: myapp/src/myapp/__init__.py
       :caption: myapp/src/myapp/__init__.py

    Sie kann mit ``uv run`` aufgerufen werden:

    .. code-block:: console

       $ uv run mypapp
       Hello from myapp!

    Alternativ könnt ihr auch eine :ref:`virtuelle Umgebung <venv>` bauen und
    dann :func:`main` aus Python heraus aufrufen:

    .. code-block:: console

       $  uv add --dev .
       Resolved 1 package in 1ms
       Audited in 0.01ms
       >>> import myapp
       >>> myapp.main()
       Hello from myapp!

.. note::
   Ich bin der festen Überzeugung, dass eine Python-Anwendung richtig gepackt
   sein sollte, um die vielen Vorteile zu genießen, wie :abbr:`z.B. (zum
   Beispiel)`:

   * die Ressourcenverwaltung mit :doc:`importlib <python3:library/importlib>`
   * mit ``project.scripts`` ausführbare Skripte anstelle angehängter
     :file:`scripts`-Ordner
   * die Vorteile des :file:`src`-Layouts mit einer allgemeinen, dokumentierten
     und gut verstandenen Struktur.

.. _uv_lock:

:file:`uv.lock`-Datei
---------------------

Mit ``uv add --dev .`` wurde auch die :file:`uv.lock`-Datei neben der
:file:`pyproject.toml`-Datei erstellt. :file:`uv.lock` ist ein
plattformübergreifendes Lockfile, das die Pakete erfasst, die über alle
möglichen Python-Merkmale wie Betriebssystem, Architektur und Python-Version
installiert werden sollen.

Im Gegensatz zur :file:`pyproject.toml`, die die allgemeinen Anforderungen eures
Projekts spezifiziert, enthält :file:`uv.lock` die genauen aufgelösten
Versionen, die in der Projektumgebung installiert sind. Diese Datei sollte in
die Versionskontrolle :doc:`Git <Python4DataScience:productive/git/index>`
eingecheckt werden, um konsistente und reproduzierbare Installationen auf
verschiedenen Rechnern zu ermöglichen.

.. literalinclude:: myapp/uv.lock
   :caption: myapp/uv.lock

:file:`uv.lock` ist eine für Menschen lesbare
:doc:`Python4DataScience:data-processing/serialisation-formats/toml/index`-Datei,
wird aber von ``uv`` verwaltet und sollte nicht manuell bearbeitet werden.

.. note::
   Wenn ``uv`` in andere Tools oder Workflows integriert werden soll, könnt ihr
   die Inhalte mit :samp:`uv export --format requirements-txt >
   {CONSTRAINTS.TXT}` in das `Requirements File Format
   <https://pip.pypa.io/en/stable/reference/requirements-file-format/>`_
   exportieren. Umgekehrt kann die erzeugte :samp:`{CONSTRAINTS.TXT}`-Datei dann
   mit ``uv pip install`` oder anderen Tools verwendet werden.

.. seealso::
   * `Project lockfile
     <https://docs.astral.sh/uv/concepts/projects/#project-lockfile>`_

.. _update-uv-lock:

Aktualisieren von :file:`uv.lock`
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

:file:`uv.lock` wird regelmäßig beim Aufruf von ``uv sync`` und ``uv run``
aktualisiert.

``--frozen``
    lässt die :file:`uv.lock`-Datei unverändert.
``--no-sync``
    vermeidet die Aktualisierung der Umgebung während ``uv run``-Aufrufen.
``--locked``
    stellt sicher, dass das Lockfile mit den Projekt-Metadaten übereinstimmt.
    Wenn das Lockfile nicht aktuell ist, wird eine Fehlermeldung ausgegeben,
    anstatt das Lockfile zu aktualisieren.

Standardmässig bevorzugt ``uv`` bei der Ausführung von ``uv sync`` und
``uv lock`` die gesperrten Versionen der Pakete. Paketversionen werden nur
geändert, wenn die Abhängigkeitsbedingungen des Projekts die vorherige,
gesperrte Version ausschließen.

``uv lock --upgrade``
    aktualisiert alle Pakete.
:samp:`uv lock --upgrade-package {PACKAGE}=={VERSION}`
    aktualisiert ein einzelnes Paket auf eine bestimmte Version.

Ihr könnt auch mit dem
:doc:`Python4DataScience:productive/git/advanced/hooks/pre-commit` regelmäßig
eure eure :file:`uv.lock`-Datei aktualisieren:

.. code-block:: yaml
   :caption: .pre-commit-config.yaml

   - repo: https://github.com/astral-sh/uv-pre-commit
     rev: 0.4.24
     hooks:
       - id: uv-lock

Plattform- und Python-Versionen einschränken
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Wenn euerProjekt nur eine begrenzte Anzahl von Plattformen oder Python-Versionen
unterstützt, könnt ihr dies in der :file:`pyprojects.toml`-Datei
:pep:`508`-konform tun, :abbr:`z.B. (zum Beispiel)` um die :file:`uv.lock`-Datei
nur auf macOS und Linux einzuschränken:

.. code-block:: toml

   [tool.uv]
   environments = [
       "sys_platform == 'darwin'",
       "sys_platform == 'linux'",
   ]
