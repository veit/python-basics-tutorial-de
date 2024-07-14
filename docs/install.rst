Installation
============

Die Installation von Python kann einfach sein. Der erste Schritt besteht darin,
die aktuelle Version von `www.python.org/downloads
<https://www.python.org/downloads/>`_ herunterzuladen. Das Tutorial basiert auf
Python 3.12.4, falls ihr jedoch Python 3.8 oder neuer installiert habt, sollte
das auch kein Problem sein.

.. tab:: Linux

   Python ist bereits in der `Linux Standard Base
   <https://wiki.linuxfoundation.org/lsb/start>`_ enthalten. Die meisten
   Linux-Distributionen wollen jedoch nichts mit dem sprachspezifischen
   Paketmanager zu tun haben, sondern alles über ``rpm``-/``deb``- :abbr:`o.ä.
   (oder ähnliche)` Paketmanager etc. verwalten. Sie wollen auch nicht, dass
   ihre Pakete für andere Dinge als Systemzwecke verwendet werden. Daher solltet
   ihr euch euer eigenes Python installieren. Unter Ubuntu geht dies :abbr:`z.B.
   (zum Beispiel)` mit:

   .. code-block:: console

      $ wget https://www.python.org/ftp/python/3.12.4/Python-3.12.4.tgz
      $ tar xf Python-3.12.4.tgz
      $ cd Python-3.12.4
      $ ./configure --enable-optimizations
      $ sudo make altinstall

   .. warning::
      Ein Nachteil ist, dass ihr regelmäßig auf die Website zurückkehren müsst,
      um nach Sicherheitsupdates zu suchen, da es keinen integrierten
      Auto-Updater gibt.

   Werden ältere Python-Versionen benötigt, :abbr:`z.B. (zum Beispiel)` um
   Bibliotheken mit :doc:`test/tox` zu testen, verwende ich `deadsnakes
   <https://launchpad.net/~deadsnakes/+archive/ubuntu/ppa>`_.

.. tab:: macOS

   Ihr könnt Python direkt von https://www.python.org/downloads/macos/ beziehen.
   Die ``universal2``-Installer sind auch auf Intel-basierten Umgebungen
   lauffähig.

   Ein Nachteil ist, dass ihr regelmäßig auf die Website zurückkehren müsst, um
   nach Sicherheitsupdates zu suchen, da es keinen integrierten Auto-Updater
   gibt. Alternativ könnt ihr auch `MOPUp <https://pypi.org/project/MOPUp/>`_
   installieren mit ``python -m pip install MOPUp`` und anschließend regelmäßig
   ``mopup`` aufrufen, um die aktuellste Version eurer Python-Installation zu
   erhalten.

   Werden ältere Python-Versionen benötigt, :abbr:`z.B. (zum Beispiel)` um
   Bibliotheken mit :doc:`test/tox` zu testen, kann  `python-build-standalone
   <https://gregoryszorc.com/docs/python-build-standalone/main/building.html#macos>`_
   verwendet werden.

.. tab:: Windows

   Python kann für die meisten Windows-Versionen nach Windows 7 mit dem
   Python-Installationsprogramm in drei Schritten installiert werden:

   #. Ladet das aktuelle Installationsprogramm von `Python Releases for Windows
      <https://www.python.org/downloads/windows/>`_ herunter, :abbr:`z.B. (zum
      Beispiel)` `Windows installer (64-bit)
      <https://www.python.org/ftp/python/3.12.4/python-3.12.4-amd64.exe>`_.
   #. Startet das Installationsprogramm. Sofern ihr die notwendigen
      Berechtigungen habt, installiert Python mit der Option *Install launcher
      for all users*. Dies sollte Python in
      :file:`C:\\Program Files\\Python312-64` installieren. Außerdem sollte *Add
      Python 3.12 to PATH* aktiviert sein damit dieser Pfad zur
      Python-Installation auch in der Liste der ``PATH``-Umgebungsvariablen
      eingetragen wird.
   #. Schließlich könnt ihr die Installation nun überprüfen, indem ihr in der
      Eingabeaufforderung folgendes eingebt:

      .. code-block:: ps1con

         C:\> python -V
         Python 3.12.4

   .. warning::
      Ein Nachteil ist, dass ihr regelmäßig auf die Website zurückkehren müsst,
      um nach Sicherheitsupdates zu suchen, da es keinen integrierten
      Auto-Updater gibt.
