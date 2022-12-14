Editoren
========

Interaktive Shell
-----------------

Mit der interaktiven Shell könnt ihr einfach die meisten Beispiele in diesem
Tutorial ausführen. Später lernt ihr auch, wie ihr Code, der in eine Datei
geschrieben wurde, einfach als Modul eingebunden werden kann.

.. tab:: Linux

   Gebt ``python3`` im Terminal ein:

   .. code-block:: console

      $ python3
      Python 3.10.4 (default, Mar 23 2022, 17:29:05)
      [GCC 9.4.0] on linux
      Type "help", "copyright", "credits" or "license" for more information.
      >>> 

.. tab:: macOS

   Öffnet ein Terminal-Fenster und gebt dort ``python3`` ein:

   .. code-block:: console


      $ python3
      Python 3.10.4 (v3.10.4:9d38120e33, Mar 23 2022, 17:29:05) [Clang 13.0.0 (clang-1300.0.29.30)] on darwin
      Type "help", "copyright", "credits" or "license" for more information.
      >>>

   .. note::
      Wenn ihr die Fehlermeldung *Kommando nicht gefunden.* erhaltet, könnt ihr
      :file:`Update Shell Profile` ausführen, das sich in
      :file:`Programme/Python3.{10}` befindet.

.. tab:: Windows

   Die interaktive Python-Shell könnt ihr starten in :menuselection:`Start -->
   Programme --> Python 3.10`

   Alternativ könnt ihr auch die direkt ausführbare Datei :file:`Python.exe`
   suchen, :abbr:`z.B. (zum Beispiel)` in
   :file:`C:\\Users\\VEIT\\AppData\\Local\\Programme\\Python\\Python310-64`
   und dann doppelklicken.

Mit den Pfeiltasten, :kbd:`Home`, :kbd:`End`, :kbd:`Page up` und
:kbd:`Page down` könnt ihr durch frühere Eingaben blättern und mit der
Eingabetaste wiederholen.

Beenden der interaktiven Shell
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Um die interaktive Shell zu beenden, könnt ihr einfach :kbd:`Ctrl-d` unter Linux
und macOS verwenden oder :kbd:`Ctrl-z` unter Windows. Alternativ könnt ihr auch
``exit()`` eingeben.

.. _idle:

IDLE
----

IDLE ist das Akronym für eine integrierte Entwicklungsumgebung (engl.:
integrated development environment) und kombiniert einen interaktiven
Interpreter mit Werkzeugen zur Code-Bearbeitung und Fehlersuche. Das Ausführen
ist sehr einfach auf den verschiedenen Plattformen:

.. tab:: Linux/macOS

   Gebt folgendes in eurem Terminal ein:

   .. code-block:: console

      $ idle-python3.10

.. tab:: Windows

   Ihr könnt IDLE starten in :menuselection:`Windows --> Alle Apps --> IDLE
   (Python GUI)`

Ihr könnt mit den Tasten :kbd:`alt-p` und :kbd:`alt-n` durch die Historie der
vorherigen Befehle blättern.
