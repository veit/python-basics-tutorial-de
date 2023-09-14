Glossar
=======

.. glossary::

   build
       ``build`` ist ein :pep:`517`-kompatibler Python-Paket-Builder. Er bietet
       eine CLI zum Erstellen von Paketen sowie eine Python-API.

       `Docs <https://pypa-build.readthedocs.io/en/stable/index.html>`__ |
       `GitHub <https://github.com/pypa/build>`__ |
       `PyPI <https://pypi.org/project/build>`__

   Built Distribution
   bdist
       Eine Struktur aus Dateien und Metadaten, die bei der Installation nur an
       den richtigen Speicherort auf dem Zielsystem verschoben werden müssen.
       :term:`wheel` ist ein solches Format, nicht jedoch *distutil’s*
       :term:`Source Distribution`, die einen Build-Schritt erfordern.

   cibuildwheel
       :doc:`/libs/cibuildwheel` ist ein Python-Paket, das :term:`wheels
       <wheel>` für alle gängigen Plattformen und Python-Versionen auf den
       meisten CI-Systemen erstellt.

       `Docs <https://cibuildwheel.readthedocs.io/>`__ |
       `GitHub <https://github.com/pypa/cibuildwheel>`__ |
       `PyPI <https://pypi.org/project/cibuildwheel>`__

       .. seealso::
          :term:`multibuild`

   conda
       Paketmanagement-Tool für die `Anaconda
       <https://docs.continuum.io/anaconda/index.html>`_-Distribution von
       `Continuum Analytics <https://www.anaconda.com/>`_. Sie ist speziell
       auf die wissenschaftliche Gemeinschaft ausgerichtet, insbesondere auf
       Windows, wo die Installation von binären Erweiterungen oft schwierig ist.

       Conda installiert keine Pakete von :term:`PyPI` und kann nur von den
       offiziellen Continuum-Repositories oder von `anaconda.org
       <https://anaconda.org>`_ oder lokalen (:abbr:`z.B. (zum Beispiel)`
       Intranet-) Paketservern installieren. Beachtet jedoch, dass :term:`pip`
       in conda installiert werden und Seite an Seite arbeiten kann, um
       Distributionen von :term:`PyPI` zu verwalten.

       .. seealso::
          * `Conda: Myths and Misconceptions
            <https://jakevdp.github.io/blog/2016/08/25/conda-myths-and-misconceptions/>`_
          * `Conda build variants
            <https://docs.conda.io/projects/conda-build/en/latest/resources/variants.html>`_

   devpi
       `devpi <https://devpi.net/>`_ ist ein leistungsstarker
       :term:`PyPI`-kompatibler Server und ein PyPI-Proxy-Cache mit einem
       Befehlszeilenwerkzeug um Paketierungs-, Test- und
       Veröffentlichungsaktivitäten zu ermöglichen.

       `Docs <http://doc.devpi.net/latest/>`__ |
       `GitHub <https://github.com/devpi/devpi>`__ |
       `PyPI <https://pypi.org/project/devpi>`__

   Distribution Package
       Eine versionierte Archivdatei, die Python-:term:`Pakete
       <Import Package>`, -:term:`Module <Modul>` und andere Ressourcendateien
       enthält, die zum Verteilen eines :term:`Releases <Release>` verwendet
       werden.

   distutils
       Paket der Python-Standardbibliothek, das Unterstützung für das
       Bootstrapping von :term:`pip` in eine bestehende Python-Installation oder
       :term:`virtuelle Umgebung` bietet.

       `Docs <https://docs.python.org/3/library/ensurepip.html>`__ |
       `GitHub <https://github.com/pypa/distutils>`__

   Egg
       Ein :term:`Built Distribution`-Format, das von :term:`Setuptools`
       eingeführt wurde und nun durch :term:`wheel` ersetzt wird. Weitere
       Informationen findet ihr unter `The Internal Structure of Python Eggs
       <https://setuptools.readthedocs.io/en/latest/deprecated/python_eggs.html>`_
       und `Python Eggs <http://peak.telecommunity.com/DevCenter/PythonEggs>`_.

   enscons
       enscons ist ein Python-Paketierungswerkzeug, das auf `SCons
       <http://scons.org/>`_ basiert. Es erstellt :term:`pip`-kompatible
       :term:`Source Distributions <Source Distribution>` und :term:`wheels
       <wheel>` ohne Verwendung von :term:`distutils` oder :term:`setuptools`,
       einschließlich Distributionen mit C-Erweiterungen. enscons hat eine
       andere Architektur und Philosophie als :term:`distutils`, da es
       Python-Paketierung zu einem allgemeinen Build-System hinzufügt. enscons
       kann euch helfen, :term:`sdists <sdist>` und :term:`wheels <wheel>` zu
       bauen.

       `GitHub <https://github.com/dholth/enscons>`__ |
       `PyPI <https://pypi.org/project/enscons>`__

   Flit
       Flit bietet eine einfache Möglichkeit, reine Python-Pakete und -Module zu
       erstellen und auf den :term:`Python Package Index` hochzuladen. Flit kann
       eine Konfigurationsdatei generieren, um schnell ein Projekt einzurichten,
       eine :term:`Source Distribution` und ein :term:`wheel` zu erstellen und
       sie zu PyPI hochzuladen.

       Flit verwendet :term:`pyproject.toml`, um ein Projekt zu konfigurieren.
       Flit ist nicht auf Werkzeuge wie :term:`setuptools` angewiesen, um
       Distributionen zu erstellen, oder auf :term:`twine`, um sie auf
       :term:`PyPI` hochzuladen.

       `Docs <https://flit.readthedocs.io/en/latest/>`__ |
       `GitHub <https://github.com/pypa/flit>`__ |
       `PyPI <https://pypi.org/project/flit>`__

   Hatch
       Hatch ist ein Kommandozeilenwerkzeug, das ihr zum Konfigurieren und
       Versionieren von Paketen, zum Spezifizieren von Abhängigkeiten und mit
       dem Build-Backend Hatchling auch zum Veröffentlichen auf dem
       :term:`Python Package Index` nutzen könnt. Das Plugin-System ermöglicht
       die einfache Erweiterung der Funktionalitäten.

       `Docs <https://hatch.pypa.io/latest/>`__ |
       `GitHub <https://github.com/pypa/hatch>`__ |
       `PyPI <https://pypi.org/project/hatch>`__

   Import Package
       Ein Python-Modul, das andere Module oder rekursiv andere Pakete enthalten
       kann.

   meson-python
       Build-Backend, das das `Meson <https://mesonbuild.com>`_-Build-System
       verwendet. Es unterstützt eine Vielzahl von Sprachen, einschließlich C,
       und ist in der Lage, die Anforderungen der meisten komplexen
       Build-Konfigurationen zu erfüllen.

       `Docs <https://meson-python.readthedocs.io/en/latest/>`__ |
       `GitHub <https://github.com/mesonbuild/meson-python>`__ |
       `PyPI <https://pypi.org/project/meson-python/>`__

   Modul
       Die Grundeinheit der Wiederverwendbarkeit von Code in Python, die in
       einem von zwei Typen existiert:

       Pure Module
           Ein Modul, das in Python geschrieben wurde und in einer einzigen
           ``.py``-Datei enthalten ist (und möglicherweise zugehörigen
           ``.pyc``- und/oder ``.pyo``-Dateien).

       Extension Module
           In der Regel in eine einzelne dynamisch ladbare vorkompilierte
           Datei, z. B. einer gemeinsamen Objektdatei (``.so``).

   multibuild
       ``multibuild`` ist ein Satz von CI-Skripten zum Erstellen und Testen von
       Python-:term:`wheels <wheel>` für Linux, macOS und Windows.

       .. seealso::
          :term:`cibuildwheel`

   pdm
       Python-Paketmanager mit :pep:`582`-Unterstützung. Er installiert und
       verwaltet Pakete ohne dass eine :term:`virtuelle Umgebung <Virtuelle
       Umgebung>` erstellt werden muss. Er verwendet auch
       :term:`pyproject.toml`, um Projekt-Metadaten zu speichern, wie in
       :pep:`621` definiert.

       `Docs <https://pdm.fming.dev/>`__ |
       `GitHub <https://github.com/pdm-project/pdm/>`__ |
       `PyPI <https://pypi.org/project/pdm>`__

   pex
       Bibliothek und Werkzeug zur Erzeugung von Python EXecutable
       (:file:`.pex`)-Dateien, die eigenständige Python-Umgebungen sind.
       .pex-Dateien sind Zip-Dateien mit ``#!/usr/bin/env python`` und einer
       speziellen :file:`__main__.py`-Datei, die das Deployment von
       Python-Applikationen stark vereinfachen können.

       `Docs <https://pex.readthedocs.io/en/latest/>`__ |
       `GitHub <https://github.com/pantsbuild/pex/>`__ |
       `PyPI <https://pypi.org/project/pex>`__

   pip
       Beliebtes Werkzeug für die Installation von Python-Paketen, das in
       neuen Versionen von Python enthalten ist.

       Es bietet die wesentlichen Kernfunktionen zum Suchen, Herunterladen und
       Installieren von Paketen aus dem :term:`Python Package Index` und andere
       Python-Paketverzeichnissen und kann über eine Befehlszeilenschnittstelle
       (CLI) in eine Vielzahl von Entwicklungsabläufen eingebunden werden.

       `Docs <https://pip.pypa.io/>`__ |
       `GitHub <https://github.com/pypa/pip>`__ |
       `PyPI <https://pypi.org/project/pip/>`__

   pip-tools
       Reihe von Werkzeugen, die eure Builds deterministisch halten und dennoch
       mit neuen Versionen eurer Abhängigkeiten auf dem Laufenden halten können.

       `Docs <https://pip-tools.readthedocs.io/en/latest/>`__ |
       `GitHub <https://github.com/jazzband/pip-tools/>`__ |
       `PyPI <https://pypi.org/project/pip-tools/>`__

   Pipenv
       Pipenv bündelt :term:`Pipfile`, :term:`pip` und :term:`virtualenv` in
       einer einzigen Toolchain. Es kann die ``requirements.txt`` automatisch
       importieren und mithilfe von `safety <https://pyup.io/safety>`_ die
       Umgebung auch auf CVEs prüfen. Schließlich erleichtert es auch die
       Deinstallation von Paketen und deren Abhängigkeiten.

       `Docs <https://pipenv.pypa.io/en/latest/>`__ |
       `GitHub <https://github.com/pypa/pipenv>`__ |
       `PyPI <https://pypi.org/project/pipenv>`__

   Pipfile
   Pipfile.lock
       ``Pipfile`` und ``Pipfile.lock`` sind eine übergeordnete,
       anwendungsorientierte Alternative zu :term:`pip`’s
       ``requirements.txt``-Datei. Die :pep:`PEP 508 Environment Markers
       <508#environment-markers>` werden ebenfalls unterstützt.

       `Docs <https://pipenv.pypa.io/en/latest/pipfile/>`__ |
       `GitHub <https://github.com/pypa/pipfile>`__

   pipx
       pipx untertüzt euch, Abhängigkeitskonflikte mit anderen auf dem System
       installierten Paketen zu vermeiden.

       `Docs <https://pypa.github.io/pipx/>`__ |
       `GitHub <https://github.com/pypa/pipx>`__ |
       `PyPI <https://pypi.org/project/pipx/>`__

   piwheels
       Website und zugrundeliegende Software, die
       :term:`Source Distribution`-Pakete von :term:`PyPI` holt und sie in
       binäre :term:`wheels <wheel>` kompiliert, die für die Installation auf
       Raspberry Pis optimiert sind.

       `Home <https://www.piwheels.org/>`__ |
       `Docs <https://piwheels.readthedocs.io/en/latest/index.html>`__ |
       `GitHub <https://github.com/piwheels/piwheels/>`__

   poetry
       Kommandozeilenwerkzeug, das die Installation und Isolierung von
       Abhängigkeiten sowie die Erstellung und Paketierung von Python-Paketen
       übernimmt. Es verwendet :term:`pyproject.toml` und bietet statt der
       Resolver-Funktionalität von :term:`pip` einen eigenen
       Dependency-Resolver.

       `Docs <https://python-poetry.org/>`__ |
       `GitHub <https://github.com/python-poetry/poetry>`__ |
       `PyPI <https://pypi.org/project/poetry/>`__

   pypi.org
       `pypi.org  <https://pypi.org/>`_ ist der Domainname für den
       :term:`Python Package Index` (:term:`PyPI`). Er löste 2017 den alten
       Index-Domain-Namen ``pypi.python.org`` ab. Er wird von :term:`warehouse`
       unterstützt.

   pyproject.toml
       Werkzeugunabhängige Datei zur Spezifikation von Projekten, die in
       :pep:`518` definiert ist.

       `Docs
       <https://pip.pypa.io/en/stable/reference/build-system/pyproject-toml/>`__

       .. seealso::
          * :ref:`pyproject-toml`

   Python Package Index
   PyPI
       :term:`pypi.org` ist der Standard-Paket-Index für die Python-Community.
       Alle Python-Entwickler können ihre Distributionen nutzen und verteilen.

   Python Packaging Authority
   PyPA
       Die `Python Packaging Authority <https://www.pypa.io/en/latest/>`_ ist
       eine Arbeitsgruppe, die mehrere Softwareprojekte für die Paketierung,
       Verteilung und Installation von Python-Bibliotheken verwaltet. Die in
       `PyPA Goals <https://www.pypa.io/en/latest/future/>`_ genannten Ziele
       sind jedoch noch während der Diskussionen um :pep:`516`, :pep:`517` und
       :pep:`518` entstanden, die mit dem :term:`pyproject.toml`-basierten
       Build-System konkurrierende Workflows erlaubten, die nicht interoperabel
       sein müssen.

   readme_renderer
       ``readme_renderer`` ist eine Bibliothek, die verwendet wird, um
       Dokumentation aus Auszeichnungssprachen wie Markdown oder
       reStructuredText in HTML zu rendern. Ihr könnt sie verwenden, um zu
       prüfen, ob eure Paketbeschreibungen auf :term:`PyPI` korrekt angezeigt
       werden.

       `GitHub <https://github.com/pypa/readme_renderer/>`__ |
       `PyPI <https://pypi.org/project/readme-renderer/>`__

   Release
       Der Snapshot eines Projekts zu einem bestimmten Zeitpunkt, gekennzeichnet
       durch eine Versionskennung.

       Eine Veröffentlichung kann mehrere :term:`Built Distributions
       <Built Distribution>` zur Folge haben.

   setuptools
       setuptools (und ``easy_install``) ist eine Sammlung von Verbesserungen
       der Python-Distutils, mit denen ihr Python-Distributionen einfacher
       erstellen und verteilen könnt, insbesondere solche, die Abhängigkeiten
       von anderen Paketen haben.

       `Docs <https://setuptools.readthedocs.io/en/latest/>`__ |
       `GitHub <https://github.com/pypa/setuptools>`__ |
       `PyPI <https://pypi.org/project/setuptools>`__

       .. seealso::
           `Packaging and distributing projects
           <https://packaging.python.org/guides/distributing-packages-using-setuptools/>`_

   scikit-build
       Build-System-Generator für ``C``-, ``C++``-, ``Fortran``- und
       ``Cython``-Erweiterungen, der :term:`setuptools`, :term:`wheel` und
       :term:`pip` integriert. Er verwendet intern ``CMake``, um eine bessere
       Unterstützung für zusätzliche Compiler, Build-Systeme, Cross-Compilation
       und das Auffinden von Abhängigkeiten und deren zugehörigen
       Build-Anforderungen zu bieten. Um die Erstellung großer Projekte zu
       beschleunigen und zu parallelisieren, kann zusätzlich Ninja installiert
       werden.

       `Docs <https://scikit-build.readthedocs.io/en/latest/>`__ |
       `GitHub <https://github.com/scikit-build/scikit-build/>`__ |
       `PyPI <https://pypi.org/project/scikit-build>`__

   shiv
       Kommandozeilenprogramm zur Erstellung von Python-Zip-Apps, wie sie in
       :pep:`441` beschrieben sind, aber zusätzlich mit allen Abhängigkeiten.

       `Docs <https://shiv.readthedocs.io/en/latest/>`__ |
       `GitHub <https://github.com/linkedin/shiv>`__ |
       `PyPI <https://pypi.org/project/shiv/>`__

   Source Distribution
   sdist
       Ein Verteilungsformat (das normalerweise mithilfe von ``python setup.py
       sdist`` generiert wird).

       Es stellt Metadaten und die wesentlichen Quelldateien bereit, die für die
       Installation mit einem Tool wie :term:`Pip` oder zum Generieren von
       :term:`Built Distributions <Built Distribution>` benötigt werden.

   Spack
       Flexibler Paketmanager, der mehrere Versionen, Konfigurationen,
       Plattformen und Compiler unterstützt. Beliebig viele Versionen von
       Paketen können auf demselben System koexistieren. Spack wurde für die
       schnelle Erstellung von wissenschaftlichen Hochleistungsanwendungen auf
       Clustern und Supercomputern entwickelt.

       `Docs <https://spack.readthedocs.io/en/latest/index.html>`__ |
       `GitHub <https://github.com/spack/spack>`__

       .. seealso::
          * :doc:`Python4DataScience:productive/envs/spack/index`

   trove-classifiers
       trove-classifiers sind zum einen Klassifikatoren, die im :term:`Python
       Package Index` verwendet werden, um Projekte systematisch zu beschreiben
       und besser auffindbar zu machen. Zum anderen sind sie ein Paket, das eine
       Liste gültiger und veralteter Klassifikatoren enthält, das zur
       Überprüfung verwendet werden kann.

       `Docs <https://pypi.org/classifiers/>`__ |
       `GitHub <https://github.com/pypa/trove-classifiers>`__ |
       `PyPI <https://pypi.org/project/trove-classifiers/>`__

   twine
       Kommandozeilenprogramm, das Programmdateien und Metadaten an eine
       Web-API übergibt. Damit lassen sich Python-Pakete auf den :term:`Python
       Package Index` hochladen.

       `Docs <https://twine.readthedocs.io/en/latest/>`__ |
       `GitHub <https://github.com/pypa/twine>`__ |
       `PyPI <https://pypi.org/project/twine>`__

   venv
       Paket, das ab Python ≥ 3.3 in der Python-Standardbibliothek ist und zur
       Erstellung :term:`virtueller Umgebungen <Virtuelle Umgebung>` gedacht
       ist.

       `Docs <https://docs.python.org/3/library/venv.html>`_ |
       `GitHub <https://github.com/python/cpython/tree/main/Lib/venv>`__

   virtualenv
       Werkzeug, das die Befehlszeilen-Umgebungsvariable ``path`` verwendet, um
       isolierte :term:`virtuelle Python-Umgebungen <Virtuelle Umgebung>` zu
       erstellen, ähnlich wie :term:`venv`. Es bietet jedoch zusätzliche
       Funktionalität für die Konfiguration, Wartung, Duplizierung und
       Fehlerbehebung.

       Ab Version 20.22.0 unterstützt virtualenv nicht mehr die Python-Versionen
       2.7, 3.5 und 3.6.

   Virtuelle Umgebung
       Eine isolierte Python-Umgebung, die die Installation von Paketen für eine
       bestimmte Anwendung ermöglicht, anstatt sie systemweit zu installieren.

       .. seealso::
          * :ref:`virtuelle-umgebungen`
          * `Creating Virtual Environments
            <https://packaging.python.org/tutorials/installing-packages/#creating-and-using-virtual-environments>`_

   Warehouse
       Die aktuelle Codebasis, die den :term:`Python Package Index`
       (:term:`PyPI`) antreibt. Sie wird auf :term:`pypi.org` gehostet.

       `Docs <https://warehouse.pypa.io/>`__ |
       `GitHub <https://github.com/pypa/warehouse>`__

   wheel
       Distributionsformat, das mit :pep:`427` eingeführt wurde. Es soll das
       :term:`Egg`-Format ersetzen und wird von aktuellen
       :term:`pip`-Installationen unterstützt.

       C-Erweiterungen können als plattformspezifische wheels für Windows, macOS
       und Linux auf dem :term:`PyPI` bereitgestellt werden. Dies hat für euch
       den Vorteil, dass ihr bei der Installation des Pakets dieses nicht
       kompilieren zu müssen.

       `Home <https://pythonwheels.com/>`__ |
       `Docs <https://wheel.readthedocs.io/>`__ |
       :pep:`427` |
       `GitHub <https://github.com/pypa/wheel>`__ |
       `PyPI <https://pypi.org/project/wheel/>`__

       .. seealso::
          * :ref:`wheels`
