Verteilungspaket erstellen
==========================

Verteilungspakete (engl.: :term:`Distribution Packages <Distribution Package>`
sind Archive, die in einen Paket-Index wie :abbr:`z.B. (zum Beispiel)`
:term:`pypi.org` hochgeladen und mit :term:`pip` installiert werden können.

.. tip::
   `cusy Seminar: Fortgeschrittenes Python
   <https://cusy.io/de/our-training-courses/advanced-python.html>`_

Struktur
--------

Ein minimales *Distribution Package* kann :abbr:`z.B. (zum Beispiel)` so
aussehen:

.. code-block:: console

    dataprep
    ├── pyproject.toml
    └── src
        └── dataprep
            ├── __init__.py
            └── loaders.py

.. _pyproject-toml:

:file:`pyproject.toml`
----------------------

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
   requires = ["hatchling>=1.27"]
   build-backend = "hatchling.build"

``build-system``
    definiert einen Abschnitt, der das Build-System beschreibt
``requires``
    definiert eine Liste von Abhängigkeiten, die installiert sein müssen, damit
    das Build-System funktioniert, in unserem Fall ``hatchling>=1.27`` für die
    Unterstützung von :pep:`639`.

    .. note::
       Versionsnummern von Abhängigkeiten sollten üblicherweise jedoch nicht
       hier festgeschrieben werden sondern in der `constraints.txt
       <https://pip.pypa.io/en/latest/user_guide/#constraints-files>`_-Datei.

``build-backend``
    identifiziert den Einstiegspunkt für das Build-Backend-Objekt als
    gepunkteten Pfad. Das ``hatchling``-Backend-Objekt ist unter
    ``hatchling.build`` verfügbar.

    .. note::
       Für Python-Pakete, die :doc:`binäre Erweiterungen <binary-extensions>`
       mit ``Cython``, ``C``-, ``C++``-, ``Fortran``- oder ``Rust`` enthalten,
       ist das :term:`hatchling`-Backend jedoch nicht geeignet. Hier sollte
       eines der folgenden Backends verwendet werden:

       * :term:`setuptools`
       * :term:`scikit-build`
       * :term:`maturin`

       Doch damit nicht genug – es gibt noch weitere Backends:

       * :term:`Flit`
       * :term:`whey`
       * :term:`poetry`
       * :term:`pybind11`
       * :term:`meson-python`

    .. seealso::
       * `pypackaging-native <https://pypackaging-native.github.io>`_

.. note::
   Mit `check-toml
   <https://github.com/pre-commit/pre-commit-hooks?tab=readme-ov-file#check-toml>`_,
   `pyproject-fmt
   <https://pyproject-fmt.readthedocs.io/en/latest/>`_ und `validate-pyproject
   <https://validate-pyproject.readthedocs.io/en/latest/>`_ könnt ihr die
   :file:`pyproject.toml`-Datei formattieren und überprüfen.

Metadaten
~~~~~~~~~

In :file:`pyproject.toml` könnt ihr auch Metadaten zu eurem Paket angeben, wie
:abbr:`z.B. (zum Beispiel)`:

.. literalinclude:: dataprep/pyproject.toml
   :language: toml
   :lines: 5-20, 22-24, 41-
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
    auf :pep:`440`.

    Wenn dies nicht der Art entspricht, wie ihr Versionen speichern wollt,
    könnt ihr mit der Option :samp:`pattern` auch einen anderen regulären
    Ausdruck definieren.

    .. seealso::
       * `Calendar Versioning <https://calver.org>`_
       * `ZeroVer <https://0ver.org/>`_

    Es gibt jedoch noch weitere Versionsschema-Plugins, wie :abbr:`z.B. (zum
    Beispiel)` `hatch-semver <https://github.com/fleetingbytes/hatch-semver>`_
    für `Semantic Versioning <https://semver.org>`_.

    Mit dem Version-Source-Plugin `hatch-vcs
    <https://github.com/ofek/hatch-vcs>`_ könnt ihr auch
    :doc:`Python4DataScience:productive/git/tag` verwenden:

    .. code-block:: toml

       [build-system]
       requires = ["hatchling>=1.27", "hatch-vcs"]
       ...
       [tool.hatch.version]
       source = "vcs"
       raw-options = { local_scheme = "no-local-version" }

    Auch das setuptools-Backend erlaubt dynamische Versionierung:

    .. code-block:: toml

       [build-system]
       requires = ["setuptools>=77.0", "setuptools-scm"]
       build-backend = "setuptools.build_meta"

       [project]
       ...
       dynamic = ["version"]

       [tool.setuptools.dynamic]
       version = {attr = "dataprep.VERSION"}

    Wollt ihr diese Version nun in eurem Paket zugänglich machen, könnt ihr
    folgenden Code verwenden:

    .. code-block:: python
       :caption: src/dataprep/__init__.py

       import importlib.metadata

       try:
           __version__ = importlib.metadata.version(__name__)
       except importlib.metadata.PackageNotFoundError:
           __version__ = "0.1.dev0"  # Fallback for development mode

    .. tip::
       Wenn die Version in mehreren Textdateien steht, kann sich die Verwendung
       von `Bump My Version
       <https://github.com/callowayproject/bump-my-version>`_ empfehlen.

       Die Konfigurationsdatei :file:`.bumpversion.toml` kann :abbr:`z.B. (zum
       Beispiel)` so aussehen:

       .. code-block:: toml

          [tool.bumpversion]
          current_version = "0.1.0"
          parse = "(?P<major>\\d+)\\.(?P<minor>\\d+)\\.(?P<patch>\\d+)"
          serialize = ["{major}.{minor}.{patch}"]
          search = "{current_version}"
          replace = "{new_version}"
          regex = false
          ignore_missing_version = false
          tag = false
          sign_tags = false
          tag_name = "v{new_version}"
          tag_message = "Bump version: {current_version} → {new_version}"
          allow_dirty = false
          commit = false
          message = "Bump version: {current_version} → {new_version}"
          commit_args = ""

          [[tool.bumpversion.files]]
          filename = "src/dataprep/__init__.py"

          [[tool.bumpversion.files]]
          filename = "docs/conf.py"

    .. seealso::
       * `Configuring setuptools using pyproject.toml files: Dynamic Metadata
         <https://setuptools.pypa.io/en/latest/userguide/pyproject_config.html#dynamic-metadata>`_

``authors``
    wird verwendet, um die Autoren des Pakets anhand ihrer Namen und
    E-Mail-Adressen zu identifizieren.

    Ihr könnt auch ``maintainers`` im selben Format auflisten.

``description``
    ist eine kurze Zusammenfassung des Pakets, die aus einem Satz besteht.
``readme``
    ist ein Pfad zu einer Datei, die eine detaillierte Beschreibung des Pakets
    enthält. Diese wird auf der Paket-Detailseite auf :term:`Python Package
    Index` (:term:`PyPI`) angezeigt. In diesem Fall wird die Beschreibung aus
    der :file:`README.rst`-Datei geladen.

.. _license-expression:

``license-expression``
    enthält valide `SPDX license expressions
    <https://spdx.github.io/spdx-spec/v2.2.2/SPDX-license-expressions/>`_.

    .. seealso::
       * `License-Expression
         <https://packaging.python.org/en/latest/specifications/core-metadata/#license-expression>`_
       * `Add License-Expression field
         <https://peps.python.org/pep-0639/#add-license-expression-field>`_

``license-files``
    gibt eine Liste von Dateien mit Lizenzinformationen an.

    .. seealso::
       * `License-File (multiple use)
         <https://packaging.python.org/en/latest/specifications/core-metadata/#license-file-multiple-use>`_

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
    Klassifizierer ``"Private :: Do Not Upload"``. :term:`PyPI` wird immer
    Pakete ablehnen, deren Klassifizierer mit ``"Private ::"`` beginnt.

``dependencies``
    gibt die Abhängigkeiten für euer Paket in einem Array an.

    .. seealso::
       :pep:`631`

``urls``
    lässt euch eine beliebige Anzahl von zusätzlichen Links auflisten, die auf
    dem :term:`Python Package Index` (:term:`PyPI`) angezeigt werden. Im
    Allgemeinen könnte dies zum Quellcode, zur Dokumentation, zu
    Aufgabenverwaltungen :abbr:`usw. (und so weiter)` führen.

.. error::
   Wenn ihr in uv den Fehler :samp:`error: No \`project\` table found in:
   \`{/PATH/TO/}pyproject.toml\`` erhaltet, habt ihr vermutlich keinen
   ``[project]``-Abschnitt definiert. Dies kann in Repositorys auftreten, die
   die :file:`pyproject.toml`-Datei nur für die Konfiguration von Tools wie
   Black und Ruff verwenden, das Projekt selbst jedoch nicht definieren.

   Um das Problem zu beheben, könnt ihr einen ``[project]``-Abschnitt einfügen,
   der zumindest ``name`` und ``version`` enthalten muss. Alternativ könnt ihr
   auch ``uv run`` mit der Option ``--no-project`` verwenden.

.. seealso::
   * `Declaring project metadata
     <https://packaging.python.org/en/latest/specifications/pyproject-toml/#declaring-project-metadata-the-project-table>`_
   * :pep:`345`

Abhängigkeitsgruppen
~~~~~~~~~~~~~~~~~~~~

``dependency-groups``
    erlaubt euch, Abhängigkeitsgruppen für euer Paket anzugeben. Dabei könnt ihr
    auch zwischen verschiedenen Sets unterscheiden:

    .. literalinclude:: dataprep/pyproject.toml
       :language: toml
       :lines: 33, 39-44
       :lineno-start: 33

Auch rekursive Abhängigkeitsgruppen sind möglich. So könnt ihr beispielsweise
für ``dev`` neben ``pre-commit`` auch alle Abhängigkeiten aus ``docs`` und
``test`` übernehmen:

.. literalinclude:: dataprep/pyproject.toml
   :language: toml
   :lines: 34-38
   :lineno-start: 34

Ihr könnt diese Abhängigkeitsgruppen installieren, :abbr:`z.B. (zum Beispiel)`
mit:

.. tab:: Linux/macOS

   .. code-block:: console

      $ cd /PATH/TO/YOUR/DISTRIBUTION_PACKAGE
      $ python3 -m venv .venv
      $ . .venv/bin/activate
      $ python -m pip install --upgrade pip
      $ python -m pip install --group dev

.. tab:: Windows

   .. code-block:: ps1

      > cd C:\PATH\TO\YOUR\DISTRIBUTION_PACKAGE
      > python3 -m venv .venv
      > .venv\Scripts\activate.bat
      > python -m pip install --upgrade pip
      > python -m pip install --group dev

.. seealso::
   * :pep:`735`
   * `Dependency Groups
     <https://packaging.python.org/en/latest/specifications/dependency-groups/>`_

:file:`src`-Package
-------------------

Wenn ihr ein neues Paket erstellt, solltet ihr kein flaches sondern das
:file:`src`-Layout verwenden, das auch in `Packaging Python Projects
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

:file:`dataprep`
    ist das Verzeichnis, das die Python-Dateien enthält. Der Name sollte mit dem
    Projektnamen übereinstimmen um die Konfiguration zu vereinfachen und für
    diejenigen, die das Paket installieren, besser erkennbar zu sein.
:file:`__init__.py`
    ist erforderlich, um das Verzeichnis als Paket zu importieren. Dies erlaubt
    euch folgende Importe:

    .. code-block:: python

       import dataprep.loaders

    oder

    .. code-block:: python

       from dataprep import loaders

    Obwohl :file:`__init__.py`-Dateien oft leer sind, können sie auch Code
    enthalten.

    .. seealso::
       * :ref:`python3:tut-packages`

:file:`loaders.py`
    ist ein Beispiel für ein Modul innerhalb des Pakets, das die Logik
    (Funktionen, Klassen, Variablen, :abbr:`etc. (et cetera)`) eures Pakets
    enthalten könnte.

Andere Dateien
--------------

.. _changelog:

:file:`CHANGELOG`
~~~~~~~~~~~~~~~~~

In der :file:`CHANGELOG`-Datei sollten alle wesentlichen Änderungen eines
Projekts dokumentiert werden. `Keep a Changelog
<https://keepachangelog.com/en/1.1.0/>`_ empfiehlt hierfür folgendes Format:

.. code-block:: rest

   [Unreleased]
   ============

   Added
   -----

   …

   Changed
   -------

   …

   Removed
   -------

   …

   [x.y.z] - YYYY-MM-DD
   ====================

   Added
   -----

   …

.. seealso::
   * `Keep a Changelog <https://keepachangelog.com/>`__

Es gibt auch etliche Python-Bibliotheken, die euch beim Erstellen der
:file:`CHANGELOG`-Datei unterstützen:

`Release Drafter <https://github.com/release-drafter/release-drafter>`_
    erstellt Entwürfe für eure nächsten Versionshinweise, sobald Pull-Requests
    in den Hauptzweig integriert werden. Release Drafter wurde entwickelt mit
    `Probot <https://github.com/probot/probot>`_, einem Framework zum Erstellen
    von GitHub-Apps zur Automatisierung und Verbesserung eurer Workflows.

    .. image:: https://raster.shields.io/github/stars/release-drafter/release-drafter
       :alt: Stars
       :target: https://github.com/release-drafter/release-drafter

    .. image:: https://raster.shields.io/github/contributors/release-drafter/release-drafter
       :alt: Contributors
       :target: https://github.com/release-drafter/release-drafter/graphs/contributors

    .. image:: https://raster.shields.io/github/commit-activity/y/release-drafter/release-drafter
       :alt: Commit activity
       :target: https://github.com/release-drafter/release-drafter/graphs/commit-activity

    .. image:: https://raster.shields.io/github/license/release-drafter/release-drafter
       :alt: Lizenz

`towncrier <https://github.com/twisted/towncrier>`_
    ist ein Dienstprogramm zur Erstellung nützlicher, zusammengefasster
    Nachrichten-Dateien für euer Projekt.

    .. image:: https://raster.shields.io/github/stars/twisted/towncrier
       :alt: Stars
       :target: https://github.com/twisted/towncrier

    .. image:: https://raster.shields.io/github/contributors/twisted/towncrier
       :alt: Contributors
       :target: https://github.com/twisted/towncrier/graphs/contributors

    .. image:: https://raster.shields.io/github/commit-activity/y/twisted/towncrier
       :alt: Commit activity
       :target: https://github.com/twisted/towncrier/graphs/commit-activity

    .. image:: https://raster.shields.io/github/license/twisted/towncrier
       :alt: Lizenz

`Scriv <https://github.com/nedbat/scriv>`_
    ist ein Kommandozeilen-Tool, das Entwicklern hilft, nützliche
    Änderungsprotokolle zu führen. Es verwaltet ein Verzeichnis mit Fragmenten
    von Änderungsprotokollen. Diese werden zu Einträgen in einer
    :file:`CHANGELOG.rst`-Datei zusammengefasst.

    .. image:: https://raster.shields.io/github/stars/nedbat/scriv
       :alt: Stars
       :target: https://github.com/nedbat/scriv

    .. image:: https://raster.shields.io/github/contributors/nedbat/scriv
       :alt: Contributors
       :target: https://github.com/nedbat/scriv/graphs/contributors

    .. image:: https://raster.shields.io/github/commit-activity/y/nedbat/scriv
       :alt: Commit activity
       :target: https://github.com/nedbat/scriv/graphs/commit-activity

    .. image:: https://raster.shields.io/github/license/nedbat/scriv
       :alt: Lizenz

`Dinghy <https://github.com/nedbat/dinghy>`_
    nutzt die GitHub GraphQL API, um aktuelle Aktivitäten zu Releases, Issues
    und Pull Requests zu finden, und erstellt daraus einen kompakten
    HTML-Überblick.

    .. image:: https://raster.shields.io/github/stars/nedbat/dinghy
       :alt: Stars
       :target: https://github.com/nedbat/dinghy

    .. image:: https://raster.shields.io/github/contributors/nedbat/dinghy
       :alt: Contributors
       :target: https://github.com/nedbat/dinghy/graphs/contributors

    .. image:: https://raster.shields.io/github/commit-activity/y/nedbat/dinghy
       :alt: Commit activity
       :target: https://github.com/nedbat/dinghy/graphs/commit-activity

    .. image:: https://raster.shields.io/github/license/nedbat/dinghy
       :alt: Lizenz

`github-activity <https://github.com/executablebooks/github-activity>`_
    generiert Markdown-Änderungsprotokolle für GitHub-Repositorys und bietet
    dabei mehr Kontrolle über die Arten von Beiträgen und Metadaten, die zur
    Erstellung der Änderungsprotokolle verwendet werden.

    .. image:: https://raster.shields.io/github/stars/executablebooks/github-activity
       :alt: Stars
       :target: https://github.com/executablebooks/github-activity

    .. image:: https://raster.shields.io/github/contributors/executablebooks/github-activity
       :alt: Contributors
       :target: https://github.com/executablebooks/github-activity/graphs/contributors

    .. image:: https://raster.shields.io/github/commit-activity/y/executablebooks/github-activity
       :alt: Commit activity
       :target: https://github.com/executablebooks/github-activity/graphs/commit-activity

    .. image:: https://raster.shields.io/github/license/executablebooks/github-activity
       :alt: Lizenz

`changelog_manager <https://github.com/masukomi/changelog_manager>`_
    hilft euch, eine :file:`CHANGELOG.md`-Datei für euer Git-Repo zu erstellen,
    die dem `Keep A Changelog <https://keepachangelog.com/en/1.1.0/>`_-Standard
    entspricht.

    .. image:: https://raster.shields.io/github/stars/masukomi/changelog_manager
       :alt: Stars
       :target: https://github.com/masukomi/changelog_manager

    .. image:: https://raster.shields.io/github/contributors/masukomi/changelog_manager
       :alt: Contributors
       :target: https://github.com/masukomi/changelog_manager/graphs/contributors

    .. image:: https://raster.shields.io/github/commit-activity/y/masukomi/changelog_manager
       :alt: Commit activity
       :target: https://github.com/masukomi/changelog_manager/graphs/commit-activity

    .. image:: https://raster.shields.io/github/license/masukomi/changelog_manager
       :alt: Lizenz

`blurb <https://github.com/python/blurb>`_
    ist ein Tool, um die CPython-Entwicklung von den lästigen
    Konflikten in `cpython/Misc/NEWS.d/
    <https://github.com/python/cpython/tree/main/Misc/NEWS.d>`_ zu befreien.

    .. image:: https://raster.shields.io/github/stars/python/blurb
       :alt: Stars
       :target: https://github.com/python/blurb

    .. image:: https://raster.shields.io/github/contributors/python/blurb
       :alt: Contributors
       :target: https://github.com/python/blurb/graphs/contributors

    .. image:: https://raster.shields.io/github/commit-activity/y/python/blurb
       :alt: Commit activity
       :target: https://github.com/python/blurb/graphs/commit-activity

    .. image:: https://raster.shields.io/github/license/python/blurb
       :alt: Lizenz

.. _coc:

:file:`CODE_OF_CONDUCT`
~~~~~~~~~~~~~~~~~~~~~~~

Wenn ihr Hinweise geben wollt, wie Fragen an euch gestellt werden können oder
wie andere zum Projekt beitragen können, kann eine :file:`CODE_OF_CONDUCT`-Datei
hilfreich sein. Damit könnt ihr festlegen, welche Art der Interaktion ihr
erwartet. Er legt auch Regeln fest, um euch und andere vor unerwünschten
Verhaltensweisen zu schützen.

.. seealso::
   * `SciPy Code of Conduct
     <https://docs.scipy.org/doc/scipy/dev/conduct/code_of_conduct.html>`_

.. _contributing:

:file:`CONTRIBUTING`
~~~~~~~~~~~~~~~~~~~~

.. seealso::
   * `Python Developer’s Guide <https://devguide.python.org/#contributing>`_
   * `Contributing to pandas
     <https://pandas.pydata.org/docs/development/contributing.html>`_

:file:`CONTRIBUTORS`
~~~~~~~~~~~~~~~~~~~~

.. seealso::
   * `All contributors <https://allcontributors.org/>`_

.. _license:

:file:`LICENSE`
~~~~~~~~~~~~~~~

Ausführliche Informationen hierzu findet ihr im Abschnitt
:doc:`Python4DataScience:productive/licensing`.

.. _readme:

:file:`README`
~~~~~~~~~~~~~~

Diese Datei teilt denjenigen, die sich für das Paket interessieren, in kurzer
Form mit, wie sie es nutzen können.

.. seealso::
   * `Make a README <https://www.makeareadme.com>`_
   * `readme.so <https://readme.so>`_

Wenn ihr das Dokument in :doc:`/document/sphinx/rest` schreibt, könnt ihr die
Inhalte auch als ausführliche Beschreibung in euer Paket übernehmen:

.. literalinclude:: dataprep/pyproject.toml
   :language: toml
   :emphasize-lines: 5
   :lineno-start: 5
   :lines: 5-12

Zudem könnt ihr sie dann auch in eure :doc:`Sphinx-Dokumentation
</document/sphinx/start>` mit ``.. include:: ../../README.rst`` übernehmen.

.. _security:

:file:`SECURITY`
~~~~~~~~~~~~~~~~

Diese Datei sollte Informationen enthalten,

* wie eine Sicherheitslücke gemeldet werden kann ohne dass sie öffentlich
  sichtbar wird,
* über den Ablauf und den Zeitplan für die Offenlegung der Schwachstelle,
* zu Links, :abbr:`z. B. (zum Beispiel)` URLs und E-Mails, unter denen
  Unterstützung angefragt werden kann.

.. seealso::
   * GitHub-Dokumentation: `Hinzufügen einer Sicherheitsrichtlinie für dein
     Repository
     <https://docs.github.com/de/code-security/how-tos/report-and-fix-vulnerabilities/configure-vulnerability-reporting/adding-a-security-policy-to-your-repository>`_
   * `github.com/veit/items/security
     <https://github.com/veit/items/security>`_

Historische oder für binäre Erweiterungen benötigte Dateien
-----------------------------------------------------------

Bevor die mit :pep:`518` eingeführte :file:`pyproject.toml`-Datei zum Standard
wurde, benötigte ``setuptools`` :file:`setup.py`, :file:`setup.cfg` und
:file:`MANIFEST.in`. Heute werden die Dateien jedoch bestenfalls noch für
:doc:`binäre Erweiterungen <binary-extensions>` benötigt.

Wenn ihr diese Dateien in euren Paketen ersetzen wollt, könnt ihr dies mit
``hatch new --init`` oder `ini2toml
<https://ini2toml.readthedocs.io/en/latest/setuptools_pep621.html>`_.

:file:`setup.py`
~~~~~~~~~~~~~~~~

Eine minimale und dennoch funktionale :download:`dataprep/setup.py` kann
:abbr:`z.B. (zum Beispiel)` so aussehen:

.. literalinclude:: dataprep/setup.py
   :language: python
   :linenos:

`package_dir
<https://docs.python.org/3.11/distutils/setupscript.html#listing-whole-packages>`_
verweist auf das Verzeichnis :file:`src`, in dem sich ein oder mehrere Pakete
befinden können. Anschließend könnt ihr mit setuptools’s `find_packages()
<https://setuptools.pypa.io/en/latest/userguide/package_discovery.html#finding-simple-packages>`_
alle Pakete in diesem Verzeichnis finden.

.. note::
    ``find_packages()`` ohne ``src/``-Verzeichnis würde alle Verzeichnisse mit
    einer ``__init__.py``-Datei paketieren, also auch ``tests/``-Verzeichnisse.

:file:`setup.cfg`
~~~~~~~~~~~~~~~~~

Diese Datei wird nicht mehr benötigt, zumindest nicht für die Paketierung.
``wheel`` sammelt heutzutage alle erforderlichen Lizenzdateien automatisch und
``setuptools`` kann mit dem ``options``-Keyword-Argument universelle
``wheel``-Pakete bauen, :abbr:`z.B. (zum Beispiel)`
:file:`dataprep-0.1.0-py3-none-any.whl`.

.. _manifest-in:

:file:`MANIFEST.in`
~~~~~~~~~~~~~~~~~~~

Die Datei enthält alle Dateien und Verzeichnisse, die nicht bereits mit
``packages`` oder ``py_module`` erfasst werden. Sie kann :abbr:`z.B. (zum
Beispiel)` so aussehen: :download:`dataprep/MANIFEST.in`:

.. literalinclude:: dataprep/MANIFEST.in
   :linenos:

Weitere Anweisungen in ``Manifest.in`` findet ihr in `MANIFEST.in commands
<https://packaging.python.org/en/latest/guides/using-manifest-in/>`__.

.. note::
   Häufig wird die Aktualisierung der :file:`Manifest.in`-Datei vergessen. Um
   dies zu vermeiden, könnt ihr `check-manifest
   <https://pypi.org/project/check-manifest/>`_ in einem :doc:`Git pre-commit
   Hook <Python4DataScience:productive/git/advanced/hooks/index>` verwenden.

.. note::
   Wenn Dateien und Verzeichnisse aus :file:`MANIFEST.in` auch installiert
   werden sollen, :abbr:`z.B. (zum Beispiel)` wenn es sich um laufzeitrelevante
   Daten handelt, könnt ihr dies mit ``include_package_data=True`` in eurem
   :func:`setup`-Aufruf angeben.

.. _uv-package-structure:

Paketstruktur erstellen
-----------------------

Mit :samp:`uv init --package {MYPACK}` lässt sich einfach eine initiale
Dateistruktur für Pakete erstellen:

.. code-block:: console

   $ uv init --package mypack
   $  tree mypack -a
   mypack
   ├── .git
   │   └── ...
   ├── .gitignore
   ├── .python-version
   ├── README.md
   ├── pyproject.toml
   └── src
       └── mypack
           └── __init__.py

:file:`.python-version`
    gibt an, welche Python-Version für die Entwicklung des Projekts verwendet
    werden soll.

    .. error::
       Wenn ihr die folgende Fehlermeldung erhaltet :samp:`error: The Python
       request from \`.python-version\` resolved to Python {U.V.W}, which is
       incompatible with the project's Python requirement: \`>={X.Y}\`. Use \`uv
       python pin\` to update the \`.python-version\` file to a compatible
       version.`, weist dies auf einen Konflikt zwischen der Versionsangabe in
       der :file:`.python-version`-Datei und der ``requires-python``-Angabe in
       der :file:`pyproject.toml`-Datei hin. Nun habt ihr drei verschiedene
       Möglichkeiten:

       * Aktualisiert eure :file:`.python-version`-Datei mit :samp:`uv python
         pin {X.Y.Z}`.
       * Überschreibt die Python-Version für einen einzelnen Befehl mit
         :samp:`uv run --python {X.Y} {COMMAND}`.
       * Aktualisiert ``requires-python``.

:file:`mypack/pyproject.toml`
    Die Datei :file:`pyproject.toml` enthält einen ``scripts``-Einstiegspunkt
    ``mypack:main``:

    .. literalinclude:: mypack/pyproject.toml
       :caption: mypack/pyproject.toml
       :emphasize-lines: 21

:file:`mypack/src/mypack/__init__.py`
    Das Modul definiert eine CLI-Funktion :func:`main`:

    .. literalinclude:: mypack/src/mypack/__init__.py
       :caption: mypack/src/mypack/__init__.py

    Sie kann mit ``uv run`` aufgerufen werden:

    .. code-block:: console

       $ uv run mypack
       Hello from mypack!

    .. note::
       :abbr:`Ggf. (Gegebenenfalls)` erstellt ``uv run`` eine :ref:`virtuelle
       Python-Umgebung <venv>` im Ordner :file:`.venv` bevor :func:`main`
       ausgeführt wird.

.. _uv-build:

Build
-----

Der nächste Schritt besteht darin, Distributionspakete für das Paket zu
erstellen. Dies sind Archive, die in den :term:`Python Package Index`
(:term:`PyPI`) hochgeladen und von :term:`pip` installiert werden können.

Führt nun den Befehl in demselben Verzeichnis aus, in dem sich
:file:`pyproject.toml` befindet:

.. tab:: Linux/macOS

   .. code-block:: console

      $ uv build
      Building source distribution...
      Building wheel from source distribution...
        Successfully built dist/mypack-0.1.0.tar.gz and dist/mypack-0.1.0-py3-none-any.whl

.. tab:: Windows

   .. code-block:: ps1

      > uv build
      Building source distribution...
      Building wheel from source distribution...
        Successfully built dist/mypack-0.1.0.tar.gz and dist/mypack-0.1.0-py3-none-any.whl

:file:`dist/mypack-0.1.0-py3-none-any.whl`
    ist eine Build-Distribution. :term:`pip` installiert bevorzugt
    Build-Distributionen und greift lediglich auf die Source-Distributionen
    zurück, wenn keine passende Build-Distribution vorhanden ist. Ihr solltet
    immer eine Source-Distribution hochladen und Build-Distributionen für die
    Plattformen bereitstellen, mit denen euer Projekt kompatibel ist. In diesem
    Fall ist unser Beispiel-Paket mit Python auf jeder Plattform kompatibel, so
    dass nur eine Build-Distribution benötigt wird:

    ``mypack``
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

    .. seealso::
       * :pep:`427`

:file:`mypack-0.1.0.tar.gz`
    ist eine :term:`Source Distribution`

.. seealso::
   * `Core metadata specifications
     <https://packaging.python.org/en/latest/specifications/core-metadata/#core-metadata>`_
   * `PyPA specifications
     <https://packaging.python.org/en/latest/specifications/>`_

Testen
------

Anschließend könnt ihr die :term:`Wheel`-Datei überprüfen mit:

.. code-block:: console

    $ uv add --dev check-wheel-contents
    Resolved 17 packages in 8ms
       Built mypack @ file:///Users/veit/sandbox/mypack
    Prepared 1 package in 442ms
    Uninstalled 1 package in 0.89ms
    Installed 10 packages in 5ms
     + annotated-types==0.7.0
     + attrs==24.2.0
     + check-wheel-contents==0.6.0
     + click==8.1.7
     ~ mypack==0.1.0 (from file:///Users/veit/sandbox/mypack)
     + packaging==24.1
     + pydantic==2.9.2
     + pydantic-core==2.23.4
     + typing-extensions==4.12.2
     + wheel-filename==1.4.1
    $ uv run check-wheel-contents dist/*.whl
    dist/dataprep-0.1.0-py3-none-any.whl: OK

Alternativ könnt ihr das Paket auch in einem neuen Projekt installieren,
:abbr:`z.B. (zum Beispiel)` in :samp:`myapp`:

.. code-block:: console

  $ uv init --app myapp
  $ cd myapp
  $ uv add ../mypack/dist/mypack-0.1.0-py3-none-any.whl
  Resolved 8 packages in 130ms
  Installed 1 package in 3ms
   + mypack==0.1.0 (from file:///Users/veit/sandbox/mypack/dist/mypack-0.1.0-py3-none-any.whl)

Anschließend könnt ihr ``mypack`` mit ``uv run`` aufrufen:

.. code-block:: console

    $ uv run mypack
    Hello from mypack!

.. seealso::
   * `Troubleshooting build failures
     <https://docs.astral.sh/uv/reference/troubleshooting/build-failures/>`_

.. note::
   Es gibt immer noch viele Anleitungen, die einen Schritt zum Aufruf der
   :file:`setup.py` enthalten, :abbr:`z.B. (zum Beispiel)` :samp:`python
   setup.py sdist`. Dies wird jedoch heutzutage von Teilen der `Python Packaging
   Authority (PyPA) <https://github.com/pypa/>`_ als `Anti-Pattern
   <https://x.com/pganssle/status/1152695229105000453>`_ angesehen.

Checks
------

* Wenn ihr ein Paket für eine Aufgabenverwaltung erstellen wollt, das die
  Aufgaben in eine Datenbank schreibt und über ein Python-:abbr:`API (engl.:
  Application Programming Interface)` und eine Befehlszeilenschnittstelle
  (:abbr:`CLI (engl.: Command-Line Interface)` bereitstellt, wie würdet ihr die
  Dateien strukturieren?

* Überlegt euch, wie ihr die oben genannten Aufgaben erledigen wollt. Welche
  Bibliotheken und Module fallen euch ein, die diese Aufgabe erfüllen könnten?
  Skizziert den Code für die Module der Python-API, der
  Befehlszeilenschnittstelle und der Datenbankanbindung.
