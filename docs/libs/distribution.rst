Verteilungspaket erstellen
==========================

Verteilungspakete (engl.: :term:`Distribution Packages <Distribution Package>`
sind Archive, die in einen Paket-Index wie :abbr:`z.B. (zum Beispiel)`
:term:`pypi.org` hochgeladen und mit :term:`pip` installiert werden können.

Einige der folgenden Befehle erfordern eine neue Version von pip, sodass ihr
sicherstellen solltet, dass ihr die neueste Version installiert habt:

.. tab:: Linux/macOS

   .. code-block:: console

      $ python3 -m pip install --upgrade pip

.. tab:: Windows

   .. code-block:: ps1

      > python  -m pip install --upgrade pip

Struktur
--------

Ein minimales Distribution Package kann :abbr:`z.B. (zum Beispiel)` so aussehen:

.. code-block:: console

    dataprep
    ├── pyproject.toml
    └── src
        └── dataprep
            ├── __init__.py
            └── loaders.py

.. _pyproject-toml:

``pyproject.toml``
------------------

:pep:`517` und :pep:`518` brachten erweiterbare Build-Backends, isolierte Builds
und :term:`pyproject.toml` im
:doc:`Python4DataScience:data-processing/serialisation-formats/toml/index`-Format.

:file:`pyproject.toml` teilt :abbr:`u.a. (unter anderem)` :term:`pip` und
:term:`build` mit, welches *Backend*-Werkzeug verwendet werden soll, um
Distributionspakete für euer Projekt zu erstellen. Ihr könnt aus einer Reihe von
Backends wählen, wobei dieses Tutorial standardmäßig ``hatchling`` verwendet.

Eine minimale und dennoch funktionale :download:`dataprep/pyproject.toml`-Datei
sieht dann :abbr:`z.B. (zum Beispiel)` so aus:

.. code-block:: toml
   :linenos:
   :lineno-start: 1

   [build-system]
   requires = ["hatchling"]
   build-backend = "hatchling.build"

``build-system``
    definiert einen Abschnitt, der das Build-System beschreibt
``requires``
    definiert eine Liste von Abhängigkeiten, die installiert sein müssen, damit
    das Build-System funktioniert, in unserem Fall ``hatchling``.

    .. note::
       Versionsnummern von Abhängigkeiten sollten üblicherweise jedoch nicht
       hier festgeschrieben werden sondern in der `requirements.txt
       <https://pip.pypa.io/en/latest/user_guide/#requirements-files>`_-Datei.

``build-backend``
    identifiziert den Einstiegspunkt für das Build-Backend-Objekt als
    gepunkteten Pfad. Das ``hatchling``-Backend-Objekt ist unter
    ``hatchling.build`` verfügbar.

.. seealso::
   Wenn ihr euch Alternativen zu ``hatchling`` anschauen wollt:

   * :term:`setuptools`
   * :term:`Flit`
   * `poetry <https://github.com/python-poetry/poetry>`_
   * `pypackaging-native <https://pypackaging-native.github.io>`_

Metadaten
~~~~~~~~~

In :file:`pyproject.toml` könnt ihr auch Metadaten zu eurem Paket angeben, wie
:abbr:`z.B. (zum Beispiel)`:

.. literalinclude:: dataprep/pyproject.toml
   :language: toml
   :lines: 5-19, 21-23, 40-
   :lineno-start: 5

``name``
    ist der Distributionsname eures Pakets. Dies kann ein beliebiger Name sein,
    solange er nur Buchstaben, Zahlen, ``.``, ``_`` und ``-`` enthält. Er sollte
    auch nicht bereits auf dem :term:`Python Package Index` (:term:`PyPI`)
    vergeben sein.
``version``
    ist die Version des Pakets.

    In unserem Beispiel ist die Versionsnummer statisch gesetzt worden. Es gibt
    jedoch auch die Möglichkeit, die Version dynamisch anzugeben, :abbr:`z.B.
    (zum Beispiel)` durch eine Datei:

    .. code-block:: toml

       [project]
       ...
       dynamic = ["version"]

       [tool.hatch.version]
       path = "src/dataprep/__about__.py"

    Das Standardmuster sucht nach einer Variablen namens :samp:`__version__`
    oder :samp:`VERSION`, die die Version enthält, optional mit dem
    vorangestellten Kleinbuchstaben :samp:`v`. Dabei basiert das Standardschema
    basiert auf :pep:`440`.

    Wenn dies nicht der Art entspricht, wie ihr Versionen speichern wollt,
    könnt ihr mit der Option :samp:`pattern` auch einen anderen regulären
    Ausdruck definieren.

    Es gibt jedoch noch weitere Versionsschema-Plugins, wie :abbr:`z.B. (zum
    Beispiel)` `hatch-semver <https://github.com/Nagidal/hatch-semver>`_ für
    `Semantic Versioning <https://semver.org>`_.

    Mit dem Version-Source-Plugin `hatch-vcs
    <https://github.com/ofek/hatch-vcs>`_ könnt ihr auch
    :doc:`Python4DataScience:productive/git/tag` verwenden:

    .. code-block:: toml

       [build-system]
       requires = ["hatchling", "hatch-vcs"]
       ...
       [tool.hatch.version]
       source = "vcs"
       raw-options = { local_scheme = "no-local-version" }

    .. seealso::
       * `Calendar Versioning <https://calver.org>`_
       * `ZeroVer <https://0ver.org/>`_
       * `bump2version <https://pypi.org/project/bump2version/>`_

    Auch das setuptools-Backend erlaubt dynamische Versionierung:

    .. code-block:: toml

       [build-system]
       requires = ["setuptools", "setuptools-scm"]
       build-backend = "setuptools.build_meta"

       [project]
       ...
       dynamic = ["version"]

       [tool.setuptools.dynamic]
       version = {attr = "dataprep.VERSION"}

    .. seealso::

       * `Configuring setuptools using pyproject.toml files: Dynamic Metadata
         <https://setuptools.pypa.io/en/latest/userguide/pyproject_config.html#dynamic-metadata>`_

    The setuptools backend also allows dynamic versioning:

    .. code-block:: toml

       [build-system]
       requires = ["setuptools", "setuptools-scm"]
       build-backend = "setuptools.build_meta"

       [project]
       ...
       dynamic = ["version"]

       [tool.setuptools.dynamic]
       version = {attr = "dataprep.VERSION"}

    .. seealso::

       * `Configuring setuptools using pyproject.toml files: Dynamic Metadata
         <https://setuptools.pypa.io/en/latest/userguide/pyproject_config.html#dynamic-metadata>`_

``authors``
    wird verwendet, um die Autoren des Pakets anahnd ihrer Namen und
    E-Mail-Adressen zu identifizieren.

    Ihr könnt auch ``maintainers`` im selben Format auflisten.

``description``
    ist eine kurze Zusammenfassung des Pakets, die aus einem Satz besteht.
``readme``
    ist ein Pfad zu einer Datei, die eine detaillierte Beschreibung des Pakets
    enthält. Diese wird auf der Paketdetailseite auf :term:`Python Package
    Index` (:term:`PyPI`) angezeigt. In diesem Fall wird die Beschreibung aus
    ``README.rst`` geladen.
``requires-python``
    gibt die Versionen von Python an, die von eurem Projekt unterstützt werden.
    Dabei werden Installationsprogramme wie :term:`pip` ältere Versionen von
    Paketen durchsuchen, bis sie eines finden, das eine passende Python-Version
    hat.
``classifiers``
    gibt dem :term:`Python Package Index` (:term:`PyPI`) und :term:`pip` einige
    zusätzliche Metadaten über euer Paket. In diesem Fall ist das Paket nur mit
    Python 3 kompatibel, steht unter der BSD-Lizenz und ist OS-unabhängig. Ihr
    solltet immer zumindest die Versionen von Python angeben, unter denen euer
    Paket läuft, unter welcher Lizenz euer Paket verfügbar ist und auf welchen
    Betriebssystemen euer Paket läuft. Eine vollständige Liste der
    Klassifizierer findet ihr unter https://pypi.org/classifiers/.

    Außerdem haben sie eine nützliche Zusatzfunktion: Um zu verhindern, dass ein
    Paket zu :term:`PyPI` hochgeladen wird, verwendet den speziellen
    Klassifikator ``"Private :: Do Not Upload"``. :term:`PyPI` wird immer Pakete
    ablehnen, deren Klassifizierer mit ``"Private ::"`` beginnt.

``dependencies``
    gibt die Abhängigkeiten für euer Paket in einem Array an.

    .. seealso::
       :pep:`631`

``urls``
    lässt euch eine beliebige Anzahl von zusätzlichen Links auflisten, die auf
    dem :term:`Python Package Index` (:term:`PyPI`) angezeigt werden. Im
    Allgemeinen könnte dies zum Quellcode, zur Dokumentation, zu
    Aufgabenverwaltungen :abbr:`usw. (und so weiter)` führen.

.. seealso::
   * `Declaring project metadata
     <https://packaging.python.org/en/latest/specifications/declaring-project-metadata/#declaring-project-metadata>`_
   * :pep:`345`

Optionale Abhängigkeiten
~~~~~~~~~~~~~~~~~~~~~~~~

``project.optional-dependencies``
    erlaubt euch, optionale Abhängigkeiten für euer Paket anzugeben. Dabei könnt
    ihr auch zwischen verschiedenen Sets unterscheiden:

.. literalinclude:: dataprep/pyproject.toml
   :language: toml
   :lines: 24-34
   :lineno-start: 34

Auch rekursive optionale Abhängigkeiten sind mit pip ≥ 21.2 möglich. So könnt
ihr beispielsweise für ``dev`` neben ``pre-commit`` auch alle Abhängigkeiten aus
``docs`` und ``test`` übernehmen:

.. literalinclude:: dataprep/pyproject.toml
   :language: toml
   :lines: 35-39
   :lineno-start: 35

Ihr könnt diese optionalen Abhängigkeiten installieren, :abbr:`z.B. (zum
Beispiel)` mit:

.. tab:: Linux/macOS

   .. code-block:: console

      $ cd /PATH/TO/YOUR/DISTRIBUTION_PACKAGE
      $ python3 -m venv .
      $ source bin/activate
      $ python -m pip install --upgrade pip
      $ python -m pip install -e '.[dev]'

.. tab:: Windows

   .. code-block:: ps1

      > cd C:\PATH\TO\YOUR\DISTRIBUTION_PACKAGE
      > python3 -m venv .
      > Scripts\activate.bat
      > python -m pip install --upgrade pip
      > python -m pip install -e '.[dev]'

``src``-Package
---------------

Wenn ihr ein neues Paket erstellt, solltet ihr kein flaches sondern das
``src``-Layout verwenden, das auch in `Packaging Python Projects
<https://packaging.python.org/en/latest/tutorials/packaging-projects/>`_ der
:term:`PyPA` empfohlen wird. Ein wesentlicher Vorteil dieses Layouts ist, dass
Tests mit der installierten Version eures Pakets und nicht mit den Dateien in
eurem Arbeitsverzeichnis ausgeführt werden.

.. seealso::
   * Hynek Schlawack: `Testing & Packaging
     <https://www.pyopensci.org/python-package-guide/package-structure-code/python-package-structure.html>`_

.. note::
   In Python ≥ 3.11 kann mit :envvar:`PYTHONSAFEPATH` sichergestellt werden,
   dass die installierten Pakete zuerst verwendet werden.

``dataprep``
    ist das Verzeichnis, das die Python-Dateien enthält. Der Name sollte mit dem
    Projektnamen übereinstimmen um die Konfiguration zu vereinfachen und für
    diejenigen, die das Paket installieren, besser erkennbar zu sein.
``__init__.py``
    ist erforderlich, um das Verzeichnis als Paket zu importieren. Die Datei
    sollte leer sein.
``loaders.py``
    ist ein Beispiel für ein Modul innerhalb des Pakets, das die Logik
    (Funktionen, Klassen, Konstanten, :abbr:`etc. (et cetera)`) eures Pakets
    enthalten könnte.

Andere Dateien
--------------

``setup.py``
~~~~~~~~~~~~

Eine minimale und dennoch funktionale :download:`dataprep/setup.py` kann
:abbr:`z.B. (zum Beispiel)` so aussehen:

.. literalinclude:: dataprep/setup.py
   :language: python
   :lines: 4-5, 9-12, 15-21,41
   :lineno-start: 1

`package_dir
<https://docs.python.org/3/distutils/setupscript.html#listing-whole-packages>`_
verweist auf das Verzeichnis ``src``, in dem sich ein oder mehrere Pakete
befinden können. Anschließend könnt ihr mit setuptools’s `find_packages()
<https://setuptools.pypa.io/en/latest/userguide/package_discovery.html#finding-simple-packages>`_
alle Pakete in diesem Verzeichnis finden.

.. note::
    ``find_packages()`` ohne ``src/``-Verzeichnis würde alle Verzeichnisse mit
    einer ``__init__.py``-Datei paketieren, also auch ``tests/``-Verzeichnisse.

``MANIFEST.in``
~~~~~~~~~~~~~~~

Die Datei enthält alle Dateien und Verzeichnisse, die nicht bereits mit
``packages`` oder ``py_module`` erfasst werden. Sie kann :abbr:`z.B. (zum
Beispiel)` so aussehen: :download:`dataprep/MANIFEST.in`:

.. literalinclude:: dataprep/MANIFEST.in
   :linenos:

Weitere Anweisungen in ``Manifest.in`` findet ihr in `Creating a source
distribution
<https://docs.python.org/3/distutils/commandref.html?highlight=manifest#creating-a-source-distribution-the-sdist-command>`__.

.. note::
    Häufig wird die Aktualisierung der ``Manifest.in``-Datei vergessen. Um dies
    zu vermeiden, könnt ihr `check-manifest
    <https://pypi.org/project/check-manifest/>`_ in einem ``pre-commit``-Hook
    verwenden.

.. note::
    Wenn ihr Dateien und Verzeichnisse aus ``MANIFEST.in`` auch installiert
    werden sollen, z.B. wenn es sich um laufzeitrelevante Daten handelt, könnt
    ihr dies mit ``include_package_data=True`` in eurem ``setup()``-Aufruf
    angeben.

``setup.cfg``
~~~~~~~~~~~~~

Diese Datei wird nicht mehr benötigt, zumindest nicht für die Paketierung.
``wheel`` sammelt heutzutage alle erforderlichen Lizenzdateien automatisch und
``setuptools`` kann mit dem ``options``-Keyword-Argument universelle
``wheel``-Pakete bauen, :abbr:`z.B. (zum Beispiel)`
``dataprep-0.1.0-py3-none-any.whl``.

``CONTRIBUTORS.rst``
~~~~~~~~~~~~~~~~~~~~

.. seealso::
    * `All contributors <https://allcontributors.org/>`_

``LICENSE``
~~~~~~~~~~~

Ausführliche Informationen hierzu findet ihr im Abschnitt
:doc:`Python4DataScience:productive/licensing`.

``README.rst``
~~~~~~~~~~~~~~

Diese Datei teilt denjenigen, die sich für das Paket interessieren, in kurzer
Form mit, wie sie es nutzen können.

.. seealso::
    * `Make a README <https://www.makeareadme.com>`_
    * `readme.so <https://readme.so>`_

Wenn ihr das Dokument in :doc:`/document/rest` schreibt, könnt ihr die Inhalte
auch als ausführliche Beschreibung in euer Paket übernehmen:

.. literalinclude:: dataprep/setup.py
   :language: python
   :lines: 1,4-9,13-14,41

Zudem könnt ihr sie dann auch in eure :doc:`Sphinx-Dokumentation
</document/start>` mit ``.. include:: ../../README.rst`` übernehmen.

``CHANGELOG.rst``
~~~~~~~~~~~~~~~~~

.. seealso::
    * `Keep a Changelog <https://keepachangelog.com/>`_
    * `Scriv <https://github.com/nedbat/scriv>`_
    * `changelog_manager <https://github.com/masukomi/changelog_manager>`_
    * `github-activity <https://github.com/executablebooks/github-activity>`_
    * `Dinghy <https://github.com/nedbat/dinghy>`_
    * `Python core-workflow blurb
      <https://github.com/python/core-workflow/tree/master/blurb>`_
    * `Release Drafter <https://github.com/release-drafter/release-drafter>`_
    * `towncrier <https://github.com/twisted/towncrier>`_

Build
-----

Der nächste Schritt besteht darin, Distributionspakete für das Paket zu
erstellen. Dies sind Archive, die in den :term:`Python Package Index`
(:term:`PyPI`) hochgeladen und von :term:`pip` installiert werden können.

Stellt sicher, dass ihr die neueste Version von ``build`` installiert habt:

Führt nun den Befehl in demselben Verzeichnis aus, in dem sich
:file:`pyproject.toml` befindet:

.. tab:: Linux/macOS

   .. code-block:: console

      $ python -m pip install build
      $ cd /PATH/TO/YOUR/DISTRIBUTION_PACKAGE
      $ rm -rf build dist
      $ python -m build

.. tab:: Windows

   .. code-block:: ps1

      > python -m pip install build
      > cd C:\PATH\TO\YOUR\DISTRIBUTION_PACKAGE
      > rm -rf build dist
      > python -m build

Die zweite Zeile stellt sicher, dass ein sauberes Build ohne Artefakte früherer
Builds erstellt wird. Die dritte Zeile sollte eine Menge Text ausgeben und nach
Abschluss zwei Dateien im ``dist``-Verzeichnis erzeugen:

.. code-block:: console

   dist
   ├── dataprep-0.1.0-py3-none-any.whl
   └── dataprep-0.1.0.tar.gz

``dataprep-0.1.0-py3-none-any.whl``
    ist eine Build-Distribution. Neuere pip-Versionen installieren bevorzugt
    Build-Distributionen, greifen aber bei Bedarf auf Source-Distributionen
    zurück. Ihr solltet immer eine Source-Distribution hochladen und
    Build-Distributionen für die Plattformen bereitstellen, mit denen euer
    Projekt kompatibel ist. In diesem Fall ist unser Beispielpaket mit Python
    auf jeder Plattform kompatibel, so dass nur eine Build-Distribution benötigt
    wird:

    ``dataprep``
        ist der normalisierte Paketname
    ``0.1.0``
        ist die Version des Distrubitionspakets
    ``py3``
        gibt die Python-Version und ggf. die C-`ABI
        <https://de.wikipedia.org/wiki/Bin%C3%A4rschnittstelle>`_ an
    ``none``
        gibt an, ob das :term:`Wheel`-Paket für jedes oder nur spezifische OS
        geeignet ist
    ``any``
        ``any`` eignet sich für jede Prozessorarchitektur, ``x86_64`` hingegen
        nur für Chips mit dem x86-Befehlssatz und einer 64-Bit-Architektur

``dataprep-0.1.0.tar.gz``
    ist eine Source-Distribution

.. seealso::
    Die Referenz für die Dateinamen findet ihr in :pep:`427`.

    Weitere Infos zu Source-Distributionen erhaltet ihr in `Creating a Source
    Distribution
    <https://docs.python.org/2/distutils/sourcedist.html#creating-a-source-distribution>`__.
    und :pep:`376`.

Testen
------

.. tab:: Linux/macOS

   .. code-block:: console

      $ mkdir test_env
      $ cd test_env
      $ python3 -m venv .
      $ source bin/activate
      $ python -m pip install dist/dataprep-0.1.0-py3-none-any.whl
      Processing ./dist/dataprep-0.1.0-py3-none-any.whl
      Collecting pandas
        Using cached pandas-1.3.4-cp39-cp39-macosx_10_9_x86_64.whl (11.6 MB)
      …
      Successfully installed dataprep-0.1.0 numpy-1.21.4 pandas-1.3.4 python-dateutil-2.8.2 pytz-2021.3 six-1.16.0

.. tab:: Windows

   .. code-block:: console

      > mkdir test_env
      > cd test_env
      > python -m venv .
      > Scripts\activate.bat
      > python -m pip install dist/dataprep-0.1.0-py3-none-any.whl
      Processing ./dist/dataprep-0.1.0-py3-none-any.whl
      Collecting pandas
        Using cached pandas-1.3.4-cp39-cp39-macosx_10_9_x86_64.whl (11.6 MB)
      …
      Successfully installed dataprep-0.1.0 numpy-1.21.4 pandas-1.3.4 python-dateutil-2.8.2 pytz-2021.3 six-1.16.0

Anschließend könnt ihr die :term:`Wheel`-Datei überprüfen mit:

.. code-block:: console

    $ python -m pip install check-wheel-contents
    $ check-wheel-contents dist/*.whl
    dist/dataprep-0.1.0-py3-none-any.whl: OK

Alternativ könnt ihr das Paket auch installieren:

.. code-block:: console

    $ python -m pip install dist/dataprep-0.1.0-py3-none-any.whl
    Processing ./dist/dataprep-0.1-py3-none-any.whl
    Collecting pandas
    …
    Installing collected packages: numpy, pytz, six, python-dateutil, pandas, dataprep
    Successfully installed dataprep-0.1 numpy-1.21.4 pandas-1.3.4 python-dateutil-2.8.2 pytz-2021.3 six-1.16.0

Anschließend könnt ihr Python aufrufen und euer ``loaders``-Modul importieren:

.. code-block:: python

    from dataprep import loaders

.. note::
   Es gibt immer noch viele Anleitungen, die einen Schritt zum Aufruf der
   :file:`setup.py` enthalten, :abbr:`z.B. (zum Beispiel)` :samp:`python
   setup.py sdist`. Dies wird jedoch heutzutage von Teilen der `Python Packaging
   Authority (PyPA) <https://github.com/pypa/>`_ als `Anti-Pattern
   <https://twitter.com/pganssle/status/1152695229105000453>`_ angesehen.
