Apps
====

App-Projekte sind für Webserver, Skripte und Befehlszeilenschnittstellen
(:abbr:`CLI (engl.: Command Line Interface)`) geeignet. Sie können mit ``uv init`` oder mit ``uv init --app`` erstellt werden:

.. code-block:: console

   $ uv init --app myapp
   $  tree myapp -a
   myapp
   ├── .git
   │   └── ...
   ├── .gitignore
   ├── .python-version
   ├── README.md
   ├── hello.py
   └── pyproject.toml

:file:`myapp/pyproject.toml`
    Beim Erstellen einer App erzeugt ``uv`` eine minimale
    :file:`pyproject.toml`-Datei:

    .. literalinclude:: myapp/pyproject.toml
       :caption: myapp/pyproject.toml

    Ein Build-System ist nicht definiert und der Quellcode befindet sich im
    obersten Verzeichnis, :abbr:`z.B. (zum Beispiel)` :file:`hello.py`.

:file:`myapp/hello.py`
    Das erstellte Skript definiert eine :func:`main`-Funktion zusammen mit der
    ``if``-Anweisung ``if __name__ == "__main__":`` für die Verwendung als
    Befehlszeilenschnittstelle:

    .. literalinclude:: myapp/hello.py

    So kann :func:`main` mit ``uv run`` ausgeführt werden:

    .. code-block:: console

       $ uv run hello.py
       Hello from myapp!

    Alternativ könnt ihr auch eine :ref:`virtuelle Umgebung <venv>` bauen und
    dann :func:`main` aus Python heraus aufrufen:

    .. code-block:: console

       $  uv add --dev .
       Resolved 1 package in 1ms
       Audited in 0.01ms
       $ uv run python
       >>> import hello
       >>> hello.main()
       Hello from myapp!

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
