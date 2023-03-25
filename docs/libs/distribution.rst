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

``pyproject.toml``
------------------

`PEP 517 <https://peps.python.org/pep-0517/>`_ und `PEP 518
<https://peps.python.org/pep-0518/>`_ brachten erweiterbare Build-Backends,
isolierte Builds und `pyproject.toml
<https://pip.pypa.io/en/stable/reference/build-system/pyproject-toml/>`_ im
:doc:`jupyter-tutorial:data-processing/serialisation-formats/toml/index`-Format.

``pyproject.toml`` teilt :abbr:`u.a. (unter anderem)` :term:`pip` und
``build`` mit, welches *Backend*-Werkzeug verwendet werden soll, um
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

   * `setuptools <https://packaging.python.org/key_projects/#setuptools>`_
   * `flit <https://packaging.python.org/key_projects/#flit>`_
   * `poetry <https://github.com/python-poetry/poetry>`_
   * `pypackaging-native <https://pypackaging-native.github.io>`_

Metadaten
~~~~~~~~~

In ``pyproject.toml`` könnt ihr auch Metadaten zu eurem Paket angeben, wie
:abbr:`z.B. (zum Beispiel)`:

.. literalinclude:: dataprep/pyproject.toml
   :language: toml
   :lines: 5-19, 21-23, 40-
   :lineno-start: 5

``name``
    ist der Distributionsname eures Pakets. Dies kann ein beliebiger Name sein,
    solange er nur Buchstaben, Zahlen, ``.``, ``_`` und ``-`` enthält. Er sollte
    auch nicht bereits auf dem :term:`Python Package Index (PyPI)` vergeben
    sein.
``version``
    ist die Version des Pakets.

    .. seealso::
       * `PEP 440 – Version Identification and Dependency Specification
         <https://peps.python.org/pep-0440/>`_
       * `Semantic Versioning <https://semver.org>`_
       * `Calendar Versioning <https://calver.org>`_
       * `ZeroVer <https://0ver.org/>`_
       * `bump2version <https://pypi.org/project/bump2version/>`_

    Einige Build-Backends erlauben es, die Version auf andere Weise anzugeben,
    :abbr:`z.B. (zum Beispiel)` durch eine Datei oder
    :doc:`jupyter-tutorial:productive/git/tag`.

``authors``
    wird verwendet, um die Autoren des Pakets anahnd ihrer Namen und
    E-Mail-Adressen zu identifizieren.

    Ihr könnt auch ``maintainers`` im selben Format auflisten.

``description``
    ist eine kurze Zusammenfassung des Pakets, die aus einem Satz besteht.
``readme``
    ist ein Pfad zu einer Datei, die eine detaillierte Beschreibung des Pakets
    enthält. Diese wird auf der Paketdetailseite auf :term:`Python Package Index
    (PyPI)` angezeigt. In diesem Fall wird die Beschreibung aus ``README.rst``
    geladen.
``requires-python``
    gibt die Versionen von Python an, die von eurem Projekt unterstützt werden.
    Dabei werden Installationsprogramme wie :term:`pip` ältere Versionen von
    Paketen durchsuchen, bis sie eines finden, das eine passende Python-Version
    hat.
``classifiers``
    gibt dem :term:`Python Package Index (PyPI)` und :term:`pip` einige
    zusätzliche Metadaten über euer Paket. In diesem Fall ist das Paket nur mit
    Python 3 kompatibel, steht unter der BSD-Lizenz und ist OS-unabhängig. Ihr
    solltet immer zumindest die Versionen von Python angeben, unter denen euer
    Paket läuft, unter welcher Lizenz euer Paket verfügbar ist und auf welchen
    Betriebssystemen euer Paket läuft. Eine vollständige Liste der
    Klassifizierer findet ihr unter https://pypi.org/classifiers/.

    Außerdem haben sie eine nützliche Zusatzfunktion: der :term:`Python Package
    Index (PyPI)` lehnt Pakete mit unbekannten *Classifiers* ab, sodass damit
    auch ein versehentlicher Upload vermieden werden kann.

    .. seealso::
       `Add invalid classifier for non open source license to avoid upload to…
       <https://github.com/veit/cookiecutter-namespace-template/commit/f4fff8ee8595ae2e59e5feb92211c8e3f1252461>`_

``dependencies``
    gibt die Abhängigkeiten für euer Paket in einem Array an.

    .. seealso::
       :pep:`631`

``urls``
    lässt euch eine beliebige Anzahl von zusätzlichen Links auflisten, die auf
    dem :term:`Python Package Index (PyPI)` angezeigt werden. Im Allgemeinen
    könnte dies zum Quellcode, zur Dokumentation, zu Aufgabenverwaltungen
    :abbr:`usw. (und so weiter)` führen.

.. seealso::
   * `Declaring project metadata
     <https://packaging.python.org/en/latest/specifications/declaring-project-metadata/#declaring-project-metadata>`_
   * `PEP 345 – Metadata for Python Software Packages 1.2
     <https://peps.python.org/pep-0345/>`_

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

Eine minimale und dennoch funktionale :download:`dataprep/setup.py` sieht dann
:abbr:`z.B. (zum Beispiel)` so aus:

.. literalinclude:: dataprep/setup.py
   :language: python
   :lines: 4-5, 9-12, 15-21,41
   :lineno-start: 1

`package_dir
<https://docs.python.org/3/distutils/setupscript.html#listing-whole-packages>`_
verweist auf das Verzeichnis ``src``, in dem sich ein oder mehrere Pakete
befinden können. Anschließend könnt ihr mit setuptools’s `find_packages()
<https://setuptools.readthedocs.io/en/latest/userguide/package_discovery.html#using-find-or-find-packages>`_
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
``wheel``-Pakete bauen, z.B. ``dataprep-0.1.0-py3-none-any.whl``.

``CONTRIBUTORS.rst``
~~~~~~~~~~~~~~~~~~~~

.. seealso::
    * `All contributors <https://allcontributors.org/>`_

``LICENSE``
~~~~~~~~~~~

Ausführliche Informationen hierzu findet ihr im Abschnitt
:doc:`jupyter-tutorial:productive/licensing`.

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
erstellen. Dies sind Archive, die in den :term:`Python Package Index (PyPI)`
hochgeladen und von :term:`pip` installiert werden können.

Stellt sicher, dass ihr die neueste Version von ``build`` installiert habt:

Führt nun den Befehl in demselben Verzeichnis aus, in dem sich
``pyproject.toml`` befindet:

.. tab:: Linux/macOS

   .. code-block:: console

      $ python -m pip install build
      $ cd /PATH/TO/YOUR/DISTRIBUTION_PACKAGE
      $ rm -rf build dist
      $ python -m build

.. tab:: Windows

   .. code-block:: ps1

      > python -m pip install build
      > cd /PATH/TO/YOUR/DISTRIBUTION_PACKAGE
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
    Die Referenz für die Dateinamen findet ihr in `File name convention
    <https://www.python.org/dev/peps/pep-0427/#file-name-convention>`_.

    Weitere Infos zu Source-Distributionen erhaltet ihr in `Creating a Source
    Distribution
    <https://docs.python.org/2/distutils/sourcedist.html#creating-a-source-distribution>`__.
    und `PEP 376 <https://www.python.org/dev/peps/pep-0376/>`_.

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
      > source bin/activate
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
    ``setup.py`` enthalten, :abbr:`z.B. (zum Beispiel)` ``python setup.py
    sdist``. Dies wird jedoch heutzutage von Teilen der `Python Packaging
    Authority (PyPA) <https://github.com/pypa/>`_ als `Anti-Pattern
    <https://twitter.com/pganssle/status/1152695229105000453>`_ angesehen.
