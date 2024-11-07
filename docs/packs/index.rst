Pakete und Programme
====================

.. _wheels:

Pakete
------

Das derzeitige Standardformat zur Verteilung von Python-Bibliotheken und
-Programmen ist die Verwendung von :term:`wheels <wheel>`. wheels wurden
entwickelt, um die Installation von Python-Code zuverlässiger zu machen und die
Verwaltung von Abhängigkeiten zu erleichtern. Die Details zur Erstellung von
wheels würden jedoch den Rahmen dieses Abschnitts sprengen, aber alle Details zu
den Anforderungen und dem Prozess zur Erstellung von wheels findet ihr in
:doc:`distribution`.

.. seealso::
   * Pradyun Gedam: `Thoughts on the Python packaging ecosystem
     <https://pradyunsg.me/blog/2023/01/21/thoughts-on-python-packaging/>`_
   * `Python Package Build Tools
     <https://www.pyopensci.org/python-package-guide/package-structure-code/python-package-build-tools.html>`_

.. toctree::
   :titlesonly:
   :hidden:

   distribution
   templating/index
   upload-install
   gitlab
   cibuildwheel
   binary-extensions
   apps

Programme
---------

``py2exe`` und ``py2app``
~~~~~~~~~~~~~~~~~~~~~~~~~

`py2exe <https://www.py2exe.org/>`_ erstellt eigenständige Windows-Programme und
`py2app <https://py2app.readthedocs.io/en/latest/>`_ dasselbe für macOS. In
beiden Fällen handelt es sich um einzelne ausführbare Dateien, die auch auf
Rechnern laufen können, auf denen Python nicht installiert ist. In vielerlei
Hinsicht sind jedoch eigenständige ausführbare Dateien nicht ideal, da sie
tendenziell größer und weniger flexibel sind als native Python-Anwendungen, aber
in manchen Situationen können sie auch die beste oder einzige Lösung sein.

``freeze``
~~~~~~~~~~

Auch das ``freeze``-Tool erstellt ein ausführbares Python-Programm, das auf
Rechnern läuft, auf denen Python nicht installiert ist. Wenn ihr das
``freeze``-Tool verwenden möchtet, müsst ihr wahrscheinlich den
Python-Quellcode herunterladen.

Beim *Einfrieren* eines Python-Programms werden C-Dateien erstellt , die dann
mit einem C-Compiler kompiliert und gelinkt werden, den ihr auf eurem System
installiert haben müsst. Die so eingefrorene Anwendung läuft nur auf
Plattformen, für die der verwendete C-Compiler seine ausführbaren Dateien
bereitstellt.

.. seealso::

    * `Tools/freeze <https://github.com/python/cpython/tree/main/Tools/freeze>`_

PyInstaller and PyOxidizer
~~~~~~~~~~~~~~~~~~~~~~~~~~

`PyInstaller <https://pyinstaller.org/en/stable/index.html#>`_ und `PyOxidizer
<https://pyoxidizer.readthedocs.io/en/pyoxidizer-0.17/index.html>`_ bündeln eine
Python-Anwendung und alle ihre Abhängigkeiten in einem einzigen Paket.

.. _briefcase:

Briefcase
~~~~~~~~~

`Briefcase <https://beeware.org/project/projects/tools/briefcase/>`__ ist ein
Werkzeug zur Konvertierung eines Python-Projekts in eine eigenständige native
Anwendung für Mac, Windows, Linux, iPhone/iPad und Android.

.. _beeware:

BeeWare
~~~~~~~

`BeeWare <https://beeware.org>`__ konvertiert euer Python-Projekt in eine
eigenständige iOS, Android, Windows, MacOS, Linux, Web und tvOS-App.
