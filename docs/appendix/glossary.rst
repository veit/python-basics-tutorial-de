Glossar
=======

.. glossary::
   :sorted:

   Akzeptanztest
   User Acceptance Test
       Überprüfung, ob Software aus Sicht der User wie beabsichtigt funktioniert
       und diese die Software akzeptieren. Akzeptanztests werden vor allem beim
       :term:` Extreme Programming` verwendet.

   Argument
       Ein Wert, der einer :term:`Funktion` übergeben wird. Es gibt zwei Arten
       von Argumenten:

       Schlüsselwortargument
           ein Argument, dem ein Bezeichner (:abbr:`z.B. (zum Beispiel)`
           ``name=``) in einem Funktionsaufruf vorangestellt ist oder das als
           Wert in einem Wörterbuch übergeben wird, dem ``**`` vorangestellt
           ist.
       Positionsargument
           ein Argument, das kein Schlüsselwortargument ist. Positionsargumente
           können am Anfang einer Argumentliste stehen und/oder als Elemente
           einer Iteration mit vorangestelltem ``*`` übergeben werden.

   Ausnahme
   Ausnahmebehandlung
   Exception
       Eine Ausnahme (englisch *exception*) reicht bestimmte Programmzustände –
       meistens Fehlerzustände – an andere Programmebenen weiter. Sie ist eine
       anpassbare Form von :term:`assert`.

       .. seealso::
          * :doc:`/control-flow/exceptions`
          * `Ausnahmen (englisch exceptions) loggen
            <https://python-basics-tutorial.readthedocs.io/de/latest/logging/examples.html#Ausnahmen-(englisch-exceptions)-loggen>`_
          * :ref:`pytest_fail`
          * :class:`python3:Exception`

   Dekorator
   Decorator
       Eine Funktion, die eine andere Funktion zurückgibt, normalerweise als
       Funktionstransformation unter Verwendung der ``@wrapper``-Syntax
       angewandt. Übliche Beispiele für Dekoratoren sind :ref:`classmethod` und
       :ref:`staticmethod`.

       .. seealso::
          * :doc:`/functions/decorators`

   Docstring
       Ein :doc:`/types/strings/built-in-modules/string`-Literal, das als erster
       Ausdruck in einer Klasse, Funktion oder einem Modul erscheint. Es wird
       vom Python-Compiler erkannt und in das ``__doc__``-Attribut der
       umschließenden Klasse, Funktion oder des Moduls aufgenommen.

       .. seealso::
          * :doc:`/document/sphinx/docstrings`

   Duck-Typing
       Programmierstil, bei dem nicht der Typ eines Objekts untersucht wird, um
       festzustellen, ob es die richtige Schnittstelle hat, sondern stattdessen
       die Methode oder das Attribut einfach aufgerufen wird.

           „Wenn es wie eine Ente aussieht und wie eine Ente quakt, muss es eine
           Ente sein.“

       Durch die Betonung von Schnittstellen anstelle spezifischer Typen
       verbessert gut gestalteter Code seine Flexibilität, indem er polymorphe
       Substitution ermöglicht. Duck-Typing vermeidet Tests mit :class:`type`
       oder :func:`isinstance` und verwendet stattdessen typischerweise
       :func:`hasattr`-Tests oder :term:`EAFP`-Programmierung.

       .. seealso::
          * :ref:`duck-typing`

   EAFP
       Easier to ask for forgiveness than permission (englisch: Es ist
       einfacher, um Vergebung zu bitten als um Erlaubnis). Dieser gängige
       Python-Stil geht von der Existenz gültiger Schlüssel oder Attribute aus
       und fängt :term:`Ausnahmen <Ausnahme>` ab, wenn sich diese Annahme als
       falsch erweist. Er zeichnet sich durch viele :term:`try`- und
       :term:`except`-Anweisungen aus. Diese Technik steht im Gegensatz zum
       :term:`LBYL`-Stil, der in vielen anderen Sprachen wie C üblich ist.

   Extreme Programming
   XP
       Softwareentwicklungsmethodik, die darauf abzielt, die Softwarequalität
       und die Reaktionsfähigkeit auf sich ändernde Kundenanforderungen zu
       verbessern. Als eine Form der agilen Softwareentwicklung werden häufige
       Releases in kurzen Entwicklungszyklen befürwortet, um die Produktivität
       zu steigern und Kontrollpunkte einzuführen, an denen neue
       Anforderungen berücksichtigt werden können.

   F-String
       :doc:`String </types/strings/built-in-modules/string>`-Literal, denen ein
       ``f`` oder ``F`` vorangestellt ist.

       .. seealso::
          * :ref:`f-strings`
          * :pep:`498`

   Funktion
       Eine Reihe von Anweisungen, die einen Wert zurückgibt. Ihr können auch
       null oder mehr Argumente übergeben werden, die bei der Ausführung des
       Hauptteils verwendet werden können.

       .. seealso::
          * :doc:`/functions/index`

   Garbage Collection
       Prozess der Freigabe von Speicher, wenn dieser nicht mehr verwendet wird.

       .. seealso::
          * :py:mod:`gc`

   Konstante
       Python hat zwar :term:`unveränderliche <Unveränderlich>` Objekte, aber
       keine konstanten Variablen. Variablen verweisen auf Objekte, es gibt
       jedoch keine Möglichkeit, zu verhindern, dass eine neue Zuweisung
       erfolgt.

   Kontrollfluss
   Control flow
       Zeitliche Abfolge der einzelnen Befehle eines Computerprogramms.

       .. seealso::
          * :doc:`/control-flow/index`

   LBYL
       Look before you leap (englisch: Schaue, bevor du springst). Bei diesem
       Stil werden vor dem Aufruf explizit die Vorbedingungen geprüft. Dieser
       Stil steht im Gegensatz zum :term:`EAFP`-Ansatz und ist durch das
       Vorhandensein vieler ``if``-Anweisungen gekennzeichnet.

   Methode
       Eine :term:`Funktion`, die innerhalb einer Klasse definiert ist. Wenn sie
       als Attribut einer Instanz dieser Klasse aufgerufen wird, erhält die
       Methode das Instanzobjekt als erstes :term:`Argument` (das normalerweise
       ``self`` heißt).

   Parameter
       :term:`Argument` einer :term:`Funktions <Funktion>`- (oder
       :term:`Methoden <Methode>`-) Definition.

       .. seealso::
          * :doc:`/functions/params`

   Singleton-Objekt
       Eine Singleton-Klasse kann nur eine Instanz von sich selbst erzeugen.
       :doc:`../types/none` ist ein Beispiel für eine Singleton-Klasse in
       Python.

   Unveränderlich
   Immutable
       Ein Objekt, das nicht verändert (:abbr:`d.h. (das heißt)` mutiert) werden
       kann. Der Wert eines unveränderlichen Objekts kann sich nicht ändern.
       :doc:`Tupel <../types/sequences-sets/tuples>` sind Beispiele für
       unveränderliche Objekte.

   Zen of Python
       Auflistung von Python-Designprinzipien und -Philosophien, die für das
       Verständnis und die Verwendung der Sprache hilfreich sind. Die Liste kann
       durch Eingabe von ``import this`` ausgegeben werden.

   .. _start-packaging:

   build
       ``build`` ist ein :pep:`517`-kompatibler Python-Paket-Builder. Er bietet
       eine :abbr:`CLI (Command Line Interface)` zum Erstellen von Paketen
       sowie eine Python-:abbr:`API (Application Programming Interface)`.

       .. seealso::
          * `Docs <https://build.pypa.io/en/stable/index.html>`__
          * `GitHub <https://github.com/pypa/build>`__
          * `PyPI <https://pypi.org/project/build>`__

   Built Distribution
   bdist
       Eine Struktur aus Dateien und Metadaten, die bei der Installation nur an
       den richtigen Speicherort auf dem Zielsystem verschoben werden müssen.
       :term:`wheel` ist ein solches Format, nicht jedoch *distutil’s*
       :term:`Source Distribution`, die einen Build-Schritt erfordern.

   cibuildwheel
       :doc:`/packs/cibuildwheel` ist ein Python-Paket, das :term:`wheels
       <wheel>` für alle gängigen Plattformen und Python-Versionen auf den
       meisten :term:`CI`-Systemen erstellt.

       .. seealso::
          * :term:`multibuild`
          * `Docs <https://cibuildwheel.pypa.io/>`__
          * `GitHub <https://github.com/pypa/cibuildwheel>`__
          * `PyPI <https://pypi.org/project/cibuildwheel>`__

   conda
       Paketmanagement-Tool für die `Anaconda-Distribution
       <https://docs.anaconda.com/anaconda/index.html>`_. Sie ist speziell auf
       die wissenschaftliche Gemeinschaft ausgerichtet, insbesondere auf
       Windows, wo die Installation von binären Erweiterungen oft schwierig ist.

       Conda installiert keine Pakete von :term:`PyPI` und kann nur von den
       offiziellen Continuum-Repositories oder von `anaconda.org
       <https://anaconda.org/>`_ oder lokalen (:abbr:`z.B. (zum Beispiel)`
       Intranet-) Paketservern installieren.

       .. note::
          :term:`pip` kann in conda installiert werden und Seite an Seite
          arbeiten kann, um Distributionen von :term:`PyPI` zu verwalten.

       .. seealso::
          * `Conda: Myths and Misconceptions
            <https://jakevdp.github.io/blog/2016/08/25/conda-myths-and-misconceptions/>`_
          * `Conda build variants
            <https://docs.conda.io/projects/conda-build/en/latest/resources/variants.html>`_
          * `Docs <https://docs.conda.io/en/latest/>`__
          * `GitHub <https://github.com/conda/conda>`__

   devpi
       `devpi <https://www.devpi.net/>`_ ist ein leistungsstarker
       :term:`PyPI`-kompatibler Server und ein PyPI-Proxy-Cache mit einem
       Befehlszeilenwerkzeug um Paketierungs-, Test- und
       Veröffentlichungsaktivitäten zu ermöglichen.

       .. seealso::
          * `Docs <https://devpi.net/docs/>`__
          * `GitHub <https://github.com/devpi/devpi>`__
          * `PyPI <https://pypi.org/project/devpi>`__

   Distribution Package
       Eine versionierte Archivdatei, die Python-:term:`Pakete
       <Import Package>`, -:term:`Module <Modul>` und andere Ressourcendateien
       enthält, die zum Verteilen eines :term:`Releases <Release>` verwendet
       werden.

   distutils
       Paket der Python-Standardbibliothek, das Unterstützung für das
       Bootstrapping von :term:`pip` in eine bestehende Python-Installation oder
       :term:`virtuelle Umgebung` bietet.

       .. seealso::
          * :doc:`Docs <python3:library/ensurepip>`
          * `GitHub <https://github.com/pypa/distutils>`__

   Egg
       Ein :term:`Built Distribution`-Format, das von :term:`Setuptools`
       eingeführt wurde und nun durch :term:`wheel` ersetzt wird. Weitere
       Informationen findet ihr unter `The Internal Structure of Python Eggs
       <https://setuptools.pypa.io/en/latest/deprecated/python_eggs.html>`_
       und `Python Eggs <http://peak.telecommunity.com/DevCenter/PythonEggs>`_.

   enscons
       enscons ist ein Python-Paketierungswerkzeug, das auf `SCons
       <https://scons.org/>`_ basiert. Es erstellt :term:`pip`-kompatible
       :term:`Source Distributions <Source Distribution>` und :term:`wheels
       <wheel>` ohne Verwendung von :term:`distutils` oder :term:`setuptools`,
       einschließlich Distributionen mit C-Erweiterungen. enscons hat eine
       andere Architektur und Philosophie als :term:`distutils`, da es
       Python-Paketierung zu einem allgemeinen Build-System hinzufügt. enscons
       kann euch helfen, :term:`sdists <sdist>` und :term:`wheels <wheel>` zu
       bauen.

       .. seealso::
          * `GitHub <https://github.com/dholth/enscons>`__
          * `PyPI <https://pypi.org/project/enscons>`__

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

       .. seealso::
          * `Docs <https://flit.pypa.io>`__
          * `GitHub <https://github.com/pypa/flit>`__
          * `PyPI <https://pypi.org/project/flit>`__

   Hatch
       Hatch ist ein Kommandozeilenwerkzeug, das ihr zum Konfigurieren und
       Versionieren von Paketen, zum Spezifizieren von Abhängigkeiten genutzt
       werden kann. Das Plugin-System ermöglicht die einfache Erweiterung der
       Funktionalitäten.

       .. seealso::
          * `Docs <https://hatch.pypa.io/latest/>`__
          * `GitHub <https://github.com/pypa/hatch>`__
          * `PyPI <https://pypi.org/project/hatch>`__

   hatchling
       Build-Backend von :term:`Hatch`, das auch zum Veröffentlichen auf dem
       :term:`Python Package Index` genutzt werden kann.

   Import Package
       Ein Python-Modul, das andere Module oder rekursiv andere Pakete enthalten
       kann.

   maturin
       Vormals pyo3-pack, ist ein :pep:`621`-kompatibles Build-Tool für
       :doc:`binäre Erweiterungen <../packs/binary-extensions>` in Rust.

   meson-python
       Build-Backend, das das `Meson <https://mesonbuild.com>`_-Build-System
       verwendet. Es unterstützt eine Vielzahl von Sprachen, einschließlich C,
       und ist in der Lage, die Anforderungen der meisten komplexen
       Build-Konfigurationen zu erfüllen.

       .. seealso::
          * `Docs <https://mesonbuild.com/meson-python/>`__
          * `GitHub <https://github.com/mesonbuild/meson-python>`__
          * `PyPI <https://pypi.org/project/meson-python/>`__

   Modul
       Ein Objekt, das als organisatorische Einheit von Python-Code dient.
       Module haben einen :doc:`Namensraum </oop/namespaces>`, der beliebige
       Python-Objekte enthält. Sie werden durch Importieren in Python
       geladen.

       Python-Module können in zwei verschiedenen Varianten existieren:

       Pure Module
           Ein Modul, das in Python geschrieben wurde und in einer einzigen
           ``.py``-Datei enthalten ist (und möglicherweise zugehörigen
           ``.pyc``- und/oder ``.pyo``-Dateien).

       Extension Module
           In der Regel in eine einzelne dynamisch ladbare vorkompilierte
           Datei, :abbr:`z.B. (zum Beispiel)` einer gemeinsamen Objektdatei
           (``.so``).

       .. seealso::
          * :doc:`/libs/batteries`

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

       .. seealso::
          * `Docs <https://pdm.fming.dev/>`__
          * `GitHub <https://github.com/pdm-project/pdm/>`__
          * `PyPI <https://pypi.org/project/pdm>`__

   pex
       Bibliothek und Werkzeug zur Erzeugung von Python Executable
       (:file:`.pex`)-Dateien, die eigenständige Python-Umgebungen sind.
       :file:`.pex`-Dateien sind Zip-Dateien mit ``#!/usr/bin/env python`` und
       einer speziellen :file:`__main__.py`-Datei, die das Deployment von
       Python-Applikationen stark vereinfachen können.

       .. seealso::
          * `Docs <https://docs.pex-tool.org/>`__
          * `GitHub <https://github.com/pex-tool/pex>`__
          * `PyPI <https://pypi.org/project/pex>`__

   pip
       Beliebtes Werkzeug für die Installation von Python-Paketen, das in
       neuen Versionen von Python enthalten ist.

       Es bietet die wesentlichen Kernfunktionen zum Suchen, Herunterladen und
       Installieren von Paketen aus dem :term:`Python Package Index` und andere
       Python-Paketverzeichnissen und kann über eine Befehlszeilenschnittstelle
       (CLI) in eine Vielzahl von Entwicklungsabläufen eingebunden werden.

       .. seealso::
          * `Docs <https://pip.pypa.io/>`__
          * `GitHub <https://github.com/pypa/pip>`__
          * `PyPI <https://pypi.org/project/pip/>`__

   pip-tools
       Reihe von Werkzeugen, die eure Builds deterministisch halten und dennoch
       mit neuen Versionen eurer Abhängigkeiten auf dem Laufenden halten können.

       .. seealso::
          * `Docs <https://pip-tools.readthedocs.io/en/latest/>`__
          * `GitHub <https://github.com/jazzband/pip-tools/>`__
          * `PyPI <https://pypi.org/project/pip-tools/>`__

   Pipenv
       Pipenv bündelt :term:`Pipfile`, :term:`pip` und :term:`virtualenv` in
       einer einzigen Toolchain. Es kann die ``requirements.txt`` automatisch
       importieren und mithilfe von `safety <https://safetycli.com>`_ die
       Umgebung auch auf CVEs prüfen. Schließlich erleichtert es auch die
       Deinstallation von Paketen und deren Abhängigkeiten.

       .. seealso::
          * `Docs <https://pipenv.pypa.io/en/latest/>`__
          * `GitHub <https://github.com/pypa/pipenv>`__
          * `PyPI <https://pypi.org/project/pipenv>`__

   Pipfile
   Pipfile.lock
       ``Pipfile`` und ``Pipfile.lock`` sind eine übergeordnete,
       anwendungsorientierte Alternative zu :term:`pip`’s
       ``requirements.txt``-Datei. Die :pep:`PEP 508 Environment Markers
       <508#environment-markers>` werden ebenfalls unterstützt.

       .. seealso::
          * `Docs <https://pipenv.pypa.io/en/latest/pipfile.html>`__
          * `GitHub <https://github.com/pypa/pipfile>`__

   pipx
       pipx unterstützt euch, Abhängigkeitskonflikte mit anderen auf dem System
       installierten Paketen zu vermeiden.

       .. seealso::
          * `Docs <https://pipx.pypa.io/stable/>`__
          * `GitHub <https://github.com/pypa/pipx>`__
          * `PyPI <https://pypi.org/project/pipx/>`__

   piwheels
       Website und zugrundeliegende Software, die
       :term:`Source Distribution`-Pakete von :term:`PyPI` holt und sie in
       binäre :term:`wheels <wheel>` kompiliert, die für die Installation auf
       Raspberry Pis optimiert sind.

       .. seealso::
          * `Home <https://www.piwheels.org/>`__
          * `Docs <https://piwheels.readthedocs.io/en/latest/index.html>`__
          * `GitHub <https://github.com/piwheels/piwheels/>`__

   poetry
       Eine All-in-One-Lösung für reine Python-Projekte. Es ersetzt
       :term:`setuptools`, :term:`venv`/:term:`pipenv`, :term:`pip`,
       :term:`wheel` und :term:`twine`. Sie macht jedoch einige schlechte
       Standardannahmen für Bibliotheken und die
       :term:`pyproject.toml`-Konfiguration ist nicht standardkonform.

       .. seealso::
          * `Docs <https://python-poetry.org/>`__
          * `GitHub <https://github.com/python-poetry/poetry>`__
          * `PyPI <https://pypi.org/project/poetry/>`__

   pybind11
       Dies ist :term:`setuptools`, aber mit einer C++-Erweiterung und von
       :term:`cibuildwheel` generierten :term:`wheels <wheel>`.

       .. seealso::
          * `Docs <https://pybind11.readthedocs.io/en/stable/>`__
          * `GitHub <https://github.com/pybind/pybind11>`__
          * `PyPI <https://pypi.org/project/pybind11/>`__

   pypi.org
       `pypi.org  <https://pypi.org/>`_ ist der Domain-Name für den
       :term:`Python Package Index` (:term:`PyPI`). Er löste 2017 den alten
       Index-Domain-Namen ``pypi.python.org`` ab. Er wird von :term:`warehouse`
       unterstützt.

   pyproject.toml
       Werkzeugunabhängige Datei zur Spezifikation von Projekten, die in
       :pep:`518` definiert ist.

       .. seealso::
          * :ref:`pyproject-toml`
          * `Docs
            <https://pip.pypa.io/en/stable/reference/build-system/pyproject-toml/>`__

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

       .. seealso::
          * `GitHub <https://github.com/pypa/readme_renderer/>`__
          * `PyPI <https://pypi.org/project/readme-renderer/>`__

   Release
       Der Snapshot eines Projekts zu einem bestimmten Zeitpunkt, gekennzeichnet
       durch eine Versionskennung.

       Eine Veröffentlichung kann mehrere :term:`Built Distributions
       <Built Distribution>` zur Folge haben.

   scikit-build
       Build-System-Generator für ``C``-, ``C++``-, ``Fortran``- und
       ``Cython``-Erweiterungen, der :term:`setuptools`, :term:`wheel` und
       :term:`pip` integriert. Er verwendet intern ``CMake``, um eine bessere
       Unterstützung für zusätzliche Compiler, Build-Systeme, Cross-Compilation
       und das Auffinden von Abhängigkeiten und deren zugehörigen
       Build-Anforderungen zu bieten. Um die Erstellung großer Projekte zu
       beschleunigen und zu parallelisieren, kann zusätzlich `Ninja
       <https://ninja-build.org>`_ installiert werden.

       .. seealso::
          * `Docs <https://scikit-build.readthedocs.io/en/latest/>`__
          * `GitHub <https://github.com/scikit-build/scikit-build>`__
          * `PyPI <https://pypi.org/project/scikit-build>`__

   setuptools
       setuptools sind das klassische Build-System, das sehr leistungsfähig ist,
       aber mit steiler Lernkurve und hohem Konfigurationsaufwand. Ab Version
       61.0.0 unterstützen die setuptools auch :term:`pyproject.toml`-Dateien.

       .. seealso::
          * `Docs <https://setuptools.readthedocs.io/>`__
          * `GitHub <https://github.com/pypa/setuptools>`__
          * `PyPI <https://pypi.org/project/setuptools>`__
          * `Packaging and distributing projects
            <https://packaging.python.org/en/latest/guides/distributing-packages-using-setuptools/>`_

   shiv
       Kommandozeilenprogramm zur Erstellung von Python-Zip-Apps, wie sie in
       :pep:`441` beschrieben sind, aber zusätzlich mit allen Abhängigkeiten.

       .. seealso::
          * `Docs <https://shiv.readthedocs.io/en/latest/>`__
          * `GitHub <https://github.com/linkedin/shiv>`__
          * `PyPI <https://pypi.org/project/shiv/>`__

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

       .. seealso::
          * :doc:`Python4DataScience:productive/envs/spack/index`
          * `Docs <https://spack.readthedocs.io/en/latest/index.html>`__
          * `GitHub <https://github.com/spack/spack>`__

   trove-classifiers
       trove-classifiers sind zum einen Klassifikatoren, die im :term:`Python
       Package Index` verwendet werden, um Projekte systematisch zu beschreiben
       und besser auffindbar zu machen. Zum anderen sind sie ein Paket, das eine
       Liste gültiger und veralteter Klassifikatoren enthält, das zur
       Überprüfung verwendet werden kann.

       .. seealso::
          * `Docs <https://pypi.org/classifiers/>`__
          * `GitHub <https://github.com/pypa/trove-classifiers>`__
          * `PyPI <https://pypi.org/project/trove-classifiers/>`__

   twine
       Kommandozeilenprogramm, das Programmdateien und Metadaten an eine
       Web-API übergibt. Damit lassen sich Python-Pakete auf den :term:`Python
       Package Index` hochladen.

       .. seealso::
          * `Docs <https://twine.readthedocs.io/en/latest/>`__
          * `GitHub <https://github.com/pypa/twine>`__
          * `PyPI <https://pypi.org/project/twine>`__

   uv
       Ein extrem schneller Python-Paket- und Projektmanager, geschrieben in
       `Rust <https://www.rust-lang.org>`_.

       uv vereinfacht Entwicklung und Deployment von Python-Projekten erheblich:

       * :ref:`Installation <uv>`
       * :ref:`Pakete erstellen <uv-package-structure>` und auf :doc:`PyPI
         <../packs/publish>` oder :doc:`GitLab <../packs/gitlab>`
         veröffentlichen
       * :doc:`Entwickeln von Anwendungen <../packs/apps>`
       * Testen von Bibliotheken mit verschiedenen :ref:`Python-Versionen
         <various-python-versions>` und :ref:`tox_uv`
       * :ref:`Reproduzieren <reproduce-virtual-env>` und :ref:`aktualisieren
         <update-uv-lock>` der Python-Umgebung,
         :abbr:`ggf. (gegebenenfalls)` auch mit einem
         :doc:`Python4DataScience:productive/envs/uv/dependency-bot`
       * :doc:`Python4DataScience:productive/envs/uv/cicd`
       * :doc:`Python4DataScience:productive/envs/uv/claude-cursor`
       * :doc:`Python4DataScience:productive/envs/uv/docker`
       * Schwachstellen überprüfen mit :ref:`uv-secure <check-vulnerabilities>`

       .. seealso::
          * `Docs <https://docs.astral.sh/uv/>`__
          * `GitHub <https://github.com/astral-sh/uv>`__
          * `PyPI <https://pypi.org/project/uv/>`__

   venv
       Paket, das ab Python ≥ 3.3 in der Python-Standardbibliothek ist und zur
       Erstellung :term:`virtueller Umgebungen <Virtuelle Umgebung>` gedacht
       ist.

       .. seealso::
          * :doc:`Docs <python3:library/venv>`
          * `GitHub <https://github.com/python/cpython/tree/main/Lib/venv>`__

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
          * :ref:`venv`
          * `Creating Virtual Environments
            <https://packaging.python.org/en/latest/tutorials/installing-packages/#creating-virtual-environments>`_

   Warehouse
       Die aktuelle Codebasis, die den :term:`Python Package Index`
       (:term:`PyPI`) antreibt. Sie wird auf :term:`pypi.org` gehostet.

       .. seealso::
          * `Docs <https://warehouse.pypa.io/>`__
          * `GitHub <https://github.com/pypi/warehouse>`__

   wheel
       Distributionsformat, das mit :pep:`427` eingeführt wurde. Es soll das
       :term:`Egg`-Format ersetzen und wird von aktuellen
       :term:`pip`-Installationen unterstützt.

       C-Erweiterungen können als plattformspezifische wheels für Windows, macOS
       und Linux auf dem :term:`PyPI` bereitgestellt werden. Dies hat für euch
       den Vorteil, dass ihr bei der Installation des Pakets dieses nicht
       kompilieren zu müssen.

       .. seealso::
          * `Home <https://pythonwheels.com/>`__
          * `Docs <https://wheel.readthedocs.io/>`__
          * :pep:`427`
          * `GitHub <https://github.com/pypa/wheel>`__
          * `PyPI <https://pypi.org/project/wheel/>`__

       .. seealso::
          * :ref:`wheels`

   whey
       Einfacher Python-:term:`wheel`-Builder mit Automatisierungsoptionen für
       :term:`trove-classifiers`.

   .. _end-packaging:

   .. _start-test-procedures:

   Statische Testverfahren
       werden verwendet um den Quellcode zu überprüfen, wobei dieser jedoch
       nicht ausgeführt wird. Sie unterteilen sich in

       * :ref:`Reviews <code_reviews>` und
       * `Statische Code-Analyse
         <https://de.wikipedia.org/wiki/Statische_Code-Analyse>`_

         Es gibt diverse Python-Pakete, die euch bei der statischen Code-Analyse
         unterstützen können, :abbr:`u.a. (unter anderem)`
         :doc:`Python4DataScience:productive/qa/flake8`,
         :doc:`Python4DataScience:productive/qa/pysa` und
         :doc:`Python4DataScience:productive/qa/wily`.

   Dynamische Testverfahren
       dienen dem Auffinden von Fehlern beim Ausführen des Quellcodes. Dabei
       wird zwischen :term:`Whitebox- <Whitebox-Test>` und :term:`Blackbox-Tests
       <Blackbox-Test>` unterschieden.

   .. _end-test-procedures:

   .. _start-test:

   Whitebox-Test
       wird unter Kenntnis des Quellcodes und der Software-Struktur entwickelt.

       In Python stehen euch verschiedene Module zur Verfügung:

       :doc:`/test/unittest`
           unterstützt euch bei der Automatisierung von Tests.
       :doc:`/test/mock`
           erlaubt euch das Erstellen und Verwenden von Mock-Objekten.
       :doc:`../document/doctest`
           ermöglicht das Testen von in Python :term:`Docstrings <Docstring>`
           geschriebenen Tests.
       :doc:`/test/tox`
           ermöglicht das Testen in verschiedenen Umgebungen.

   Blackbox-Test
       wird ohne Kenntnis des Quellcodes entwickelt. Neben :doc:`/test/unittest`
       kann in Python auch :doc:`/test/hypothesis` für solche Tests verwendet
       werden.

   ``assert``
       Ein Schlüsselwort, das die Codeausführung anhält, wenn sein Argument
       falsch ist.

   Continuous Integration
   CI
   Kontinuierliche Integration
       Automatisches Überprüfen des Erstellungs- und Testprozesses auf
       verschiedenen Plattformen.

   Dummy
       Objekt, das herumgereicht, aber nie wirklich benutzt wird. Normalerweise
       werden Dummies nur zum Füllen von Parameter-Listen verwendet.

   ``except``
       Schlüsselwort, das verwendet wird, um eine :term:`Exception` abzufangen
       und sorgfältig zu behandeln.

   Fake
       Objekt, das eine tatsächlich funktionierende Implementierung hat, in der
       Regel aber eine Abkürzung nimmt, die es nicht für die Produktion geeignet
       macht.

   Integrationstest
       Tests, die überprüfen, ob die verschiedenen Teile der Software wie
       erwartet zusammenarbeiten.

   Mock
       Objekte, die mit :term:`Exception` programmiert sind, die eine
       Spezifikation der Aufrufe bilden, die ihr voraussichtlich erhalten
       werdet.

       .. seealso::
          * `Mock-Objekt <https://de.wikipedia.org/wiki/Mock-Objekt>`_

   pytest
       Ein Python-Paket mit Test-Utilities.

       .. seealso::
          * :doc:`/test/pytest/index`

   Regressionstest
       Tests zum Schutz vor neuen Fehlern oder Regressionen, die durch neue
       Software und Updates auftreten können.

   Stubs
       liefern vorgefertigte Antworten auf Aufrufe, die während des Tests
       getätigt werden, und reagieren in der Regel überhaupt nicht auf
       irgendetwas, das nicht für den Test programmiert wurde.

   Test-driven development
   TDD
   Testgetriebene Entwicklung
       Technik zur Erstellung von Software, die die Softwareentwicklung durch
       das Schreiben von Tests führt. Sie wurde Ende der 1990er Jahre von Kent
       Beck als Teil von :term:` Extreme Programming` entwickelt. Im
       Wesentlichen folgen dabei wiederholt drei einfache Schritte:

       #. Schreiben eines Tests für die nächste Funktion, die hinzugefügt werden
          soll.
       #. Schreiben des Funktionscode, bis der Test bestanden ist.
       #. Überarbeiten sowohl den neuen als auch den alten Code, um ihn gut zu
          strukturieren.

       Obwohl diese drei Schritte, die oft als *„Red – Green – Refactor“*
       zusammengefasst werden, das Herzstück des Prozesses bilden, gibt es auch
       einen wichtigen ersten Schritt, bei dem zunächst eine Liste mit
       Testfällen erstellt wird. Anschließend wird einer dieser Tests
       ausgewählt, *„Red – Green – Refactor“* darauf angewendet  und dann der
       nächste Test ausgewählt. Während des Prozesses werden weitere Tests
       dieser Liste hinzugefügt.

       .. seealso::
          * Kent Beck: `Canon TDD <https://tidyfirst.substack.com/p/canon-tdd>`_
          * Kent Beck: `Test-driven development by example
            <https://archive.org/details/est-driven-development-by-example/test-driven-development-by-example/>`_

   ``try``
       Ein Schlüsselwort, das einen Teil des Codes schützt, der eine
       :term:`Exception` auslösen kann.

   .. _end-test:
