Binäre Erweiterungen
====================

Eine der Funktionen des CPython-Interpreters besteht darin, dass neben der
Ausführung von Python-Code auch eine reichhaltige C-API für die Verwendung
durch andere Software verfügbar ist. Eine der häufigsten Anwendungen dieser
C-API besteht darin, importierbare C-Erweiterungen zu erstellen, die Dinge
ermöglichen, die im reinen Python-Code nur schwer zu erreichen sind.

Anwendungsfälle
---------------

Die typischen Anwendungsfälle für Binäre Erweiterungen lassen sich in drei
Kategorien unterteilen:

Beschleunigungsmodule
    Diese Module sind eigenständig und werden nur erstellt, um schneller zu
    laufen als der entsprechende reine Python-Code. Im Idealfall haben die
    Accelerator-Module immer ein Python-Äquivalent, das als Fallback verwendet
    werden kann, wenn die beschleunigte Version auf einem bestimmten System
    nicht verfügbar ist.

    Die CPython-Standardbibliothek verwendet viele Accelerator-Module.

Wrapper-Module
    Diese Module werden erstellt, um vorhandene C-Interfaces in Python verfügbar
    zu machen. Sie können entweder die zugrunde liegenden C-Interfaces direkt
    verfügbar oder eine *Pythonic*-API bereitgestellt werden, die
    Features von Python verwendet, um die API einfacher zu verwenden.

    Die CPython-Standardbibliothek verwendet umfangreiche Wrapper-Module.

Systemzugriffe auf niedriger Ebene
    Diese Module werden erstellt, um auf Funktionen der
    CPython-Laufzeitumgebung, des Betriebssystems oder der
    zugrundeliegenden Hardware zuzugreifen. Durch plattformspezifischen Code
    können mit solchen Erweiterungsmodulen Dinge erreicht werden, die mit reinem
    Python-Code nicht möglich wären.

    Eine Reihe von CPython-Standard-Bibliotheksmodulen sind in C geschrieben, um
    auf Interpreter-Interna zuzugreifen, die nicht auf der Sprachebene verfügbar
    sind.

    Eine besonders bemerkenswerte Eigenschaft von C-Erweiterungen ist, dass sie,
    den Global Interpreter Lock (GIL) von CPython bei lang andauernden
    Operationen freigeben können, unabhängig davon, ob diese Operationen CPU-
    oder IO-gebunden sind.

Nicht alle Erweiterungsmodule passen genau in die oben genannten Kategorien. So
umfassen z.B. die in `NumPy <https://numpy.org/>`_ enthaltenen
Erweiterungsmodule alle drei Anwendungsfälle:

* Sie verschieben innere Schleifen aus Geschwindigkeitsgründen auf C,
* umschließen externe Bibliotheken in C, FORTRAN und anderen Sprachen und
* verwenden Systemschnittstellen auf niedriger Ebene für CPython und das
  zugrunde liegende Betriebssystem, um die gleichzeitige Ausführung von
  vektorisierten Operationen zu unterstützen und das Speicherlayout von
  erstellten Objekten genau zu steuern.

Nachteile
---------

Früher war der Hauptnachteil bei der Verwendung von Beschleunigungsmodulen, dass
dadurch die Distribution der Software erschwert wurde. Heute ist dieser Nachteil
durch :term:`wheel` kaum noch vorhanden. Einige Nachteile bleiben dennoch:

* Die Installation aus den Sourcen bleibt weiterhin kompliziert.
* Ggf. gibt es kein passendes :term:`wheel` für den verwendeten Build des
  CPython-Interpreters oder alternativen Interpretern wie `PyPy
  <https://www.pypy.org/>`__, `IronPython <https://ironpython.net/>`_ oder
  `Jython <https://www.jython.org/>`_.
* Die Wartung und Pflege der Pakete ist aufwändiger da die Maintainer nicht nur
  mit Python sondern auch mit einer anderen Sprache und der CPython C-API
  vertraut sein müssen. Zudem erhöht sich die Komplexität, wenn neben dem
  Beschleunigungsmodul auch eine Python-Fallback-Implementierung bereitgestellt
  wird.
* Schließlich funktionieren häufig auch Importmechanismen, wie der direkte
  Import aus ZIP-Dateien, nicht für Extensions-Module.

Alternativen
------------

… zu Beschleunigungsmodulen
~~~~~~~~~~~~~~~~~~~~~~~~~~~

Wenn Extensions-Module nur verwendet werden, um Code schneller auszuführen,
sollten auch eine Reihe anderer Alternativen in Betracht gezogen werden:

* Sucht nach vorhandenen optimierten Alternativen. Die CPython-Standardbibliothek
  enthält eine Reihe optimierter Datenstrukturen und Algorithmen, insbesondere in
  den builtins und den Modulen ``collections`` und ``itertools``.

  Gelegentlich bietet auch der :term:`Python Package Index` (:term:`PyPI`)
  zusätzliche Alternativen. Manchmal kann ein Modul eines Drittanbieters die
  Notwendigkeit vermeiden, ein eigenes Accelerator-Modul zu erstellen.

* Für lange laufende Anwendungen kann der JIT-kompilierte `PyPy
  <https://pypi.org/>`_-Interpreter eine geeignete Alternative zum
  Standard-CPython sein. Die Hauptschwierigkeit bei der Übernahme von PyPy
  besteht typischerweise in der Abhängigkeit von anderen Beschleunigungsmodulen.
  Während PyPy die CPython C API emuliert, verursachen Module, die darauf
  angewiesen sind, Probleme für das PyPy JIT, und die Emulation legt oft Defekte
  in Beschleunigungsmodulen offen, die CPython toleriert. (häufig bei Reference
  Counting Errors).

* `Cython <https://cython.org/>`__ ist ein ausgereifter statischer Compiler, der
  den meisten Python-Code zu C-Extensions-Modulen kompilieren kann. Die
  anfängliche Kompilierung bietet einige Geschwindigkeitssteigerungen (durch
  Umgehung der CPython-Interpreter-Ebene), und Cythons optionale statische
  Typisierungsfunktionen können zusätzliche Möglichkeiten für
  Geschwindigkeitssteigerungen bieten. Für Python-Programmierer bietet Cython
  eine niedrigere Eintrittshürde relativ zu anderen Sprachen wie C oder C ++).

  Die Verwendung von Cython hat jedoch den Nachteil, die Komplexität der
  Verteilung der resultierenden Anwendung zu erhöhen.

* `Numba <http://numba.pydata.org/>`__ ist ein neueres Tool, das die `LLVM
  Compiler-Infrastruktur <https://llvm.org/>`_ nutzt, um während der Laufzeit
  selektiv Teile einer Python-Anwendung auf nativen Maschinencode zu
  kompilieren. Es erfordert, dass LLVM auf dem System verfügbar ist, auf dem der
  Code ausgeführt wird. Es kann, insbesondere bei vektorisierbaren Vorgängen
  zu erheblichen Geschwindigkeitssteigerungen führen.

… zu Wrapper-Modulen
~~~~~~~~~~~~~~~~~~~~

Die C-ABI (`Application Binary Interface
<https://de.wikipedia.org/wiki/Bin%C3%A4rschnittstelle>`_) ist ein Standard für
die gemeinsame Nutzung von Funktionen zwischen mehreren Anwendungen. Eine der
Stärken der CPython C-API (`Application Programming Interface
<https://de.wikipedia.org/wiki/Programmierschnittstelle>`_) ist es, dass
Python-Benutzer diese Funktionalität nutzen können. Das manuelle Wrapping von
Modulen ist jedoch sehr mühsam, so dass eine Reihe anderer Alternativen in
Betracht gezogen werden sollten.

Die unten beschriebenen Ansätze vereinfachen nicht die Distribution, aber sie
können den Wartungsaufwand im Vergleich zu Wrapper-Modulen deutlich reduzieren.

* `Cython <http://cython.org/>`__ eignet sich nicht nur zum Erstellen von
  Accelerator-Modulen, sondern auch zum Erstellen von Wrapper-Modulen. Da das
  Wrapping der API immer noch von Hand erfolgen muss, ist es keine gute Wahl beim
  Wrapping großer APIs.

* `cffi <https://cffi.readthedocs.io/>`_ ist das Projekt einiger Personen aus
  dem `PyPy <https://pypy.org/>`__-Entwicklungsteam, um C-Module einfacher für
  Python-Anwendungen verfügbar zu machen. Es macht das Wrapping eines C-Moduls
  basierend auf seinen  Header-Dateien relativ einfach, auch wenn man sich mit C
  selbst nicht auskennt.

  Einer der Hauptvorteile von cffi besteht darin, dass es mit dem PyPy-JIT
  kompatibel ist, sodass CFFI-Wrapper-Module vollständig von den
  PyPy-Tracing-JIT-Optimierungen partizipieren können.

* `SWIG <http://www.swig.org/>`_ ist ein Wrapper Interface Generator, der eine
  Vielzahl von Programmiersprachen, einschließlich Python, mit C- und C++-Code
  verbindet.

* Das ``ctypes``-Modul der Standardbibliothek ist zwar nützlich um Zugriff auf
  C-Schnittstellen zu erhalten, wenn die Header-Informationen jedoch nicht
  verfügbar sind, leidet es jedoch daran, dass es nur auf der C ABI-Ebene
  arbeitet und somit keine automatische Konsistenzprüfung zwischen der
  exportierten Schnittstelle und dem Python-Code macht. Im Gegensatz dazu
  können die obigen Alternativen alle auf der C-API arbeiten und
  C-Header-Dateien verwenden, um die Konsistenz zu gewährleisten.

* `pythoncapi_compat <https://github.com/python/pythoncapi_compat>`_ kann
  verwendet werden, um eine C-Erweiterung zu schreiben, die mehrere
  Python-Versionen mit einer einzigen Codebasis unterstützt. Es besteht aus der
  Header-Datei ``pythoncapi_compat.h`` und dem Skript ``upgrade_pythoncapi.py``.

… für den Systemzugriff auf niedriger Ebene
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Für Anwendungen, die Low Level System Access benötigen, ist ein
Beschleunigungsmodul oft ist der beste Weg. Dies gilt insbesondere für den Low
Level Access auf die CPython-Runtime, da einige Operationen (wie das Freigeben
des Global Interpreter Lock (GIL) nicht zulässig sind, wenn der Interpreter den
Code selbst ausführt, gerade auch wenn Module wie ``ctypes`` oder ``cffi``
verwendet werden, um Zugriff auf das relevanten C-API-Interfaces zu erhalten.

In Fällen, in denen das Erweiterungsmodul das zugrunde liegende Betriebssystem
oder die Hardware (statt der CPython-Runtime) manipuliert, ist es manchmal
besser, eine normale C-Bibliothek (oder eine Bibliothek in einer anderen
Programmiersprache wie C ++ oder Rust) zu schreiben, die eine C-kompatible ABI),
bereitstellt und anschließend eine der oben beschriebenen Wrapping-Techniken zu
verwenden um das Interface als importierbares Python-Modul verfügbar zu machen.

Implementierung
---------------

Wir wollen nun unser ``dataprep``-Paket erweitern und einigen C-Code
integrieren. Hierfür verwenden wir `Cython <https://cython.org/>`__, um den
Python-Code aus :download:`dataprep/src/dataprep/cymean.pyx` in optimierten
C-Code während des Build-Prozesses zu übersetzen. Cython-Dateien haben den
Suffix ``pyx`` und können sowohl Python- also auch C-Code enthalten.

Als Build-Backend können wir jedoch aktuell noch nicht ``hatchling.build``
verwenden, sondern müssen auf ``build_meta`` der :term:`setuptools`
zurückgreifen:

.. literalinclude:: dataprep/pyproject.toml
   :language: toml
   :lines: 1-3
   :lineno-start: 1

.. note::
   Mit `extensionlib <https://github.com/ofek/extensionlib>`_ gibt es ein
   Toolkit für Extensions-Module, das aktuell jedoch noch kein
   ``hatchling``-Plugin enthält.

.. seealso::
   * :term:`Meson <meson-python>`: `Cython Support
     <https://mesonbuild.com/Cython.html>`_
   * :term:`scikit-build`: `C Runtime, Compiler and Build System Generator
     <https://scikit-build.readthedocs.io/en/latest/generators.html>`_

Da Cython selbst ein Python-Paket ist, kann es einfach in der
:download:`dataprep/pyproject.toml`-Datei in die Liste der Abhängigkeiten
aufgenommen werden:

.. literalinclude:: dataprep/pyproject.toml
   :language: toml
   :lines: 19-22
   :lineno-start: 19
   :emphasize-lines: 2

Die Setuptools nutzen :download:`dataprep/setup.py`, um auch
Nicht-Python-Dateien in ein Paket aufzunehmen.

.. literalinclude:: dataprep/setup.py
    :language: python
    :lines: 3-5,9,40-

Nun könnt ihr den Build-Prozess mit dem Befehl ``pyproject-build`` ausführen und
überprüfen, ob die Cython-Datei auch wie erwartet im Paket landet:

.. code-block:: console

    $ pyproject-build .
    * Creating venv isolated environment...
    * Installing packages in isolated environment... (cython, setuptools>=40.6.0, wheel)
    * Getting dependencies for sdist...
    Compiling src/dataprep/cymean.pyx because it changed.
    [1/1] Cythonizing src/dataprep/cymean.pyx
    …
    copying src/dataprep/cymean.c -> dataprep-0.1.0/src/dataprep
    copying src/dataprep/cymean.pyx -> dataprep-0.1.0/src/dataprep
    …
    running build_ext
    building 'dataprep.cymean' extension
    …
    Successfully built dataprep-0.1.0.tar.gz and dataprep-0.1.0-cp39-cp39-macosx_10_9_x86_64.whl

Schließlich können wir unser Paket überprüfen mit ``check-wheel-contents``:

.. code-block:: console

    $ check-wheel-contents dataprep/dist/*.whl
    dataprep/dist/dataprep-0.1.0-cp39-cp39-macosx_10_9_x86_64.whl: OK

Alternativ könnt ihr auch unser ``dataprep``-Paket installieren und ``mean``
verwenden:

.. code-block:: console

    $ python -m pip install dataprep/dist/dataprep-0.1.0-cp39-cp39-macosx_10_9_x86_64.whl
    $ python

.. code-block:: python

    >>> from dataprep.mean import mean
    >>> from random import randint
    >>> nums = [randint(1, 1_000) for _ in range(1_000_000)]
    >>> mean(nums)
    500.296087

Dabei wurde mit der ``random.randint``-Funktion eine Liste von einer Million
Zufallszahlen mit Werten zwischen 1 und 1000 erstellt.

.. seealso::
   Der `CPython Extending and Embedding guide
   <https://docs.python.org/3/extending/>`_ enthält eine Einführung in das
   Schreiben eigener Extension-Module in C: `Extending Python with C or C++
   <https://docs.python.org/3/extending/extending.html>`_. Beachtet jedoch
   bitte, dass diese Einführung nur die grundlegenden Tools zum Erstellen von
   Erweiterungen beshreibt, die im Rahmen von CPython bereitgestellt werden.
   Third-Party-Tools wie `Cython <http://cython.org/>`__, `cffi
   <https://cffi.readthedocs.io/>`_, `SWIG <http://www.swig.org/>`_ und `Numba
   <https://numba.pydata.org/>`__ bieten sowohl einfachere als auch
   ausgeklügeltere Ansätze zum Erstellen von C- und C ++- Erweiterungen für
   Python.

   `Python Packaging User Guide: Binary Extensions
   <https://packaging.python.org/guides/packaging-binary-extensions/>`_
   behandelt nicht nur verschiedene verfügbare Tools, die die Erstellung von
   Beschleunigungsmodulen vereinfachen, sondern erläutert auch die verschiedenen
   Gründe, warum das Erstellen eines Extension Module wünschenswert sein
   könnte.

Erstellen von Beschleunigungsmodulen
------------------------------------

Beschleunigungsmodule für Windows
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Bevor ihr ein Beschleunigungsmodul erstellen könnt, müsst ihr sicherstellen,
dass ihr einen geeigneten Compiler zur Verfügung habt. Unter Windows wird
Visual C zum Erstellen des offiziellen CPython-Interpreters verwendet und er
sollte auch zum Erstellen kompatibler Beschleunigungsmodule verwendet werden:


Für Python ≥ 3.5 installiert `Visual Studio Code
<https://code.visualstudio.com/>`_ mit `Python Extension
<https://marketplace.visualstudio.com/items?itemName=ms-python.python>`_

Das Erstellen mit dem empfohlenen Compiler unter Windows stellt sicher, dass
eine kompatible C-Bibliothek im gesamten Python-Prozess verwendet wird.

Beschleunigungsmodule für Linux
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Linux-Binaries müssen eine ausreichend alte glibc verwenden, um mit älteren
Distributionen kompatibel zu sein. `Distrowatch <https://distrowatch.com/>`_
bereitet in tabellarischer Form auf, welche Versionen der Distributionen welche
Bibliothek liefern:

* `Red Hat Enterprise Linux <https://distrowatch.com/table.php?distribution=redhat>`_
* `Debian <https://distrowatch.com/table.php?distribution=debian>`_
* `Ubuntu <https://distrowatch.com/table.php?distribution=ubuntu>`_
* …

Das `PYPA/Manylinux <https://github.com/pypa/manylinux>`_-Projekt erleichtert
die Distribution von Beschleunigungsmodulen als :term:`Wheels <wheel>` für die
meisten Linux-Plattformen. Hieraus ging auch :pep:`513` hervor, das die
``manylinux1_x86_64``- und ``manylinux1_i686``-Plattform-Tags definiert.

Beschleunigungsmodule für macOS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Die Binärkompatibilität auf macOS wird durch das Zielsystem für die minimale
Implementierung bestimmt, z. B. *10.9* , das in der Umgebungsvariable
``MACOSX_DEPLOYMENT_TARGET`` definiert wird. Beim Erstellen mit
setuptools/distutils wird das Deployment-Ziel mit dem Flag ``--plat-name``
angegeben, z.B. ``macosx-10.9-x86_64``. Weitere Informationen zu
Deployment-Zielen für Mac OS Python-Distributionen findet ihr im `MacPython
Spinning Wheels-Wiki <https://github.com/MacPython/wiki/wiki/Spinning-wheels>`_.

Deployment von Beschleunigungsmodulen
-------------------------------------

Im Folgenden soll das Deployment auf dem :term:`Python Package Index`
(:term:`PyPI`) oder einem anderen Index beschrieben werden.

.. note::
   Bei Deployments auf Linux-Distributionen sollte beachtet werden, dass diese
   Anforderungen an das spezifische Build-System stellen. Daher sollten neben
   :term:`Wheels <wheel>` immer auch :term:`Source Distributions (sdist)
   <Source Distribution>` bereitgestellt werden.

.. seealso::
   * `Deploying Python applications
     <https://packaging.python.org/discussions/deploying-python-applications/>`_
   * `Supporting Windows using Appveyor
     <https://packaging.python.org/guides/supporting-windows-using-appveyor/>`_
