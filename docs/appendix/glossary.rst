Glossar
=======

.. glossary::

   Built Distribution
       Eine Struktur aus Dateien und Metadaten, die bei der Installation nur an
       den richtigen Speicherort auf dem Zielsystem verschoben werden müssen.
       :term:`Wheel` ist ein solches Format, nicht jedoch *distutil’s*
       :term:`Source Distributions <Source Distribution (sdist)>`, die einen
       Build-Schritt erfordern.

   conda
       Paketmanagement-Tool für die `Anaconda
       <https://docs.continuum.io/anaconda/index.html>`_-Distribution von
       `Continuum Analytics <https://www.anaconda.com/>`_. Sie ist speziell
       auf die wissenschaftliche Gemeinschaft ausgerichtet, insbesondere auf
       Windows, wo die Installation von binären Erweiterungen oft schwierig ist.

       Conda installiert keine Pakete von PyPI und kann nur von den offiziellen
       Continuum-Repositories oder von `anaconda.org <https://anaconda.org/>`_
       oder lokalen (z.B. Intranet-) Paketservern installieren. Beachtet jedoch,
       dass Pip in conda installiert werden und Seite an Seite arbeiten kann, um
       Distributionen von PyPI zu verwalten.

       .. seealso::
          * `Conda: Myths and Misconceptions
            <https://jakevdp.github.io/blog/2016/08/25/conda-myths-and-misconceptions/>`_
          * `Conda build variants
            <https://docs.conda.io/projects/conda-build/en/latest/resources/variants.html>`_

   devpi
       `devpi <https://devpi.net/>`_ ist ein leistungsstarker PyPI-kompatibler
       Server und ein PyPI-Proxy-Cache mit einem Befehlszeilenwerkzeug um
       Paketierungs-, Test- und Veröffentlichungsaktivitäten zu ermöglichen.

   Distribution Package
       Eine versionierte Archivdatei, die Python-:term:`Pakete <Import Package>`,
       -:term:`Module <Modul>` und andere Ressourcendateien enthält, die zum
       Verteilen eines :term:`Releases <Release>` verwendet werden.

   Egg
       Ein :term:`Built Distribution`-Format, das von :term:`Setuptools`
       eingeführt wurde und nun durch :term:`Wheel` ersetzt wird. Weitere
       Informationen findet ihr unter `The Internal Structure of Python Eggs
       <https://setuptools.readthedocs.io/en/latest/deprecated/python_eggs.html>`_
       und `Python Eggs <http://peak.telecommunity.com/DevCenter/PythonEggs>`_.

   Import Package
       Ein Python-Modul, das andere Module oder rekursiv andere Pakete enthalten
       kann.

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

   pip
       Ein Tool zum Installieren von Python-Paketen.

       `Docs <https://pip.pypa.io/>`__ |
       `GitHub <https://github.com/pypa/pip>`__ |
       `PyPI <https://pypi.org/project/pip/>`__ |

   pypi.org
       `pypi.org  <https://pypi.org/>`_ ist der Domainname für den
       Python Package Index (PyPI). Er löste 2017 den alten Index-Domain-Namen
       *pypi.python.org* ab. Er wird von :term:`warehouse` unterstützt.

   Python Package Index (PyPI)
       `PyPI <https://pypi.org/>`_ ist der Standard-Paket-Index für die
       Python-Community. Alle Python-Entwickler können ihre Distributionen
       nutzen und verteilen.

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

   Source Distribution (sdist)
        Ein Verteilungsformat (das normalerweise mithilfe von ``python setup.py
        sdist`` generiert wird).

        Es stellt Metadaten und die wesentlichen Quelldateien bereit, die für
        die Installation mit einem Tool wie :term:`Pip` oder zum Generieren
        von :term:`Built Distributions <Built Distribution>` benötigt werden.

   Virtuelle Umgebung
       Eine isolierte Python-Umgebung, die die Installation von Paketen für eine
       bestimmte Anwendung ermöglicht, anstatt sie systemweit zu installieren.

       `Docs <https://docs.python.org/3/library/venv.html>`_ |
       `Creating Virtual Environments
       <https://packaging.python.org/tutorials/installing-packages/#creating-and-using-virtual-environments>`_ |

   Warehouse
       Die aktuelle Codebasis, die den Python Package Index (PyPI) antreibt. Sie
       wird auf `pypi.org`_ gehostet.

   Wheel
       Distributionsformat, das mit :pep:`427` eingeführt wurde. Es soll das
       :term:`Egg`-Format ersetzen und wird von aktuellen
       :term:`Pip`-Installationen unterstützt.

       C-Erweiterungen können als plattformspezifische Wheels für Windows, macOS
       und Linux auf PyPI bereitgestellt werden. Dies hat für die Benutzer des
       Pakets den Vorteil, bei der Installation nicht kompilieren zu müssen.

       `Home <https://pythonwheels.com/>`__ |
       `Docs <https://wheel.readthedocs.io/>`__ |
       :pep:`427` |
       `GitHub <https://github.com/pypa/wheel>`__ |
       `PyPI <https://pypi.org/project/wheel/>`__ |
