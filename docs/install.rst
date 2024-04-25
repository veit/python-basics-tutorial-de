Installation
============

Die Installation von Python ist einfach. Der erste Schritt besteht darin, die
aktuelle Version von `www.python.org/downloads
<https://www.python.org/downloads/>`_ herunterzuladen. Das Tutorial basiert auf
Python 3.10, falls ihr jedoch Python 3.7 oder 3.8 installiert habt, ist das auch
kein Problem.

.. tab:: Linux

   Bei den meisten Linux-Distributionen ist Python bereits installiert. Wenn
   eine vorkompilierte Version von Python in eurer Linux-Distribution existiert,
   empfehle ich euch, diese zu verwenden.

   Wenn ihr dennoch eine aktuellere Python-Version installieren wollt, könnt ihr
   dies :abbr:`z.B. (zum Beispiel)` für Debian oder Ubuntu wie folgt tun:

   .. code-block:: console

      $ wget https://www.python.org/ftp/python/3.12.3/Python-3.12.3.tgz
      $ tar xf Python-3.12.3.tgz
      $ cd Python-3.12.3
      $ ./configure --enable-optimizations
      $ sudo make altinstall

.. tab:: macOS

   Ihr benötigt eine Python-Version, die zu eurem macOS und eurem Prozessor
   passt. Wenn ihr die richtige Variante ermittelt habt, könnt ihr die
   Image-Datei herunterladen und mit einem Doppelklick mounten und anschließend
   das darin enthaltene Installationsprogramm starten. Anschließend befindet
   sich Python im :file:`Programme`-Ordner.

   Wenn ihr `Homebrew <https://brew.sh/>`_ verwendet, könnt ihr Python auch
   einfach im Terminal installieren mit:

   .. code-block:: console

      $ brew install python3

.. tab:: Windows

   Python kann für die meisten Windows-Versionen nach Windows 7 mit dem
   Python-Installationsprogramm in drei Schritten installiert werden:

   #. Ladet das aktuelle Installationsprogramm von `Python Releases for Windows
      <https://www.python.org/downloads/windows/>`_ herunter, :abbr:`z.B. (zum
      Beispiel)` `Windows installer (64-bit)
      <https://www.python.org/ftp/python/3.10.6/python-3.10.6-amd64.exe>`_.
   #. Startet das Installationsprogramm. Sofern ihr die notwendigen
      Berechtigungen habt, installiert Python mit der Option *Install launcher
      for all users*. Dies sollte Python in
      :file:`C:\\Program Files\\Python310-64` installieren. Außerdem sollte *Add
      Python 3.10 to PATH* aktiviert sein damit dieser Pfad zur
      Python-Installation auch in der Liste der ``PATH``-Umgebungsvariablen
      eingetragen wird.
   #. Schließlich könnt ihr die Installation nun überprüfen, indem ihr in der
      Eingabeaufforderung folgendes eingebt:

      .. code-block:: ps1con

         C:\> python -V
         Python 3.10.6

.. note::
   Wenn auf eurem System bereits Python installiert ist, könnt ihr problemlos
   euer eigenes Python installieren. Eine neue Version ersetzt nicht die alte
   sondern wird an einem anderen Ort installiert.
