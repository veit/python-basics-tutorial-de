Dateisystem
===========

Um mit Dateien zu arbeiten, müsst ihr häufig auch mit dem Dateisystem und den
unterschiedlichen Konventionen je nach Betriebssystem interagieren. Hierfür
zeige ich euch :doc:`os <python3:library/os>` und speziell :doc:`os.path
<python3:library/os.path>`.

Pfade und Pfadnamen
-------------------

Alle Betriebssysteme verweisen auf Dateien mit Zeichenketten, die als Pfadnamen
bezeichnet werden. Python bietet eine Reihe von Funktionen, die euch helfen,
einige Probleme zu lösen. Die Semantik von Pfadnamen ist auf allen
Betriebssystemen sehr ähnlich, da das Dateisystem meist als Baumstruktur
modelliert ist, wobei eine Festplatte die Wurzel und Ordner, Unterordner
:abbr:`usw. (und so weiter)` die Zweige und Unterzweige darstellen; :abbr:`d.h.
(das heißt)`, dass die meisten Betriebssysteme auf eine bestimmte Datei in sehr
ähnlicher Art verweisen.

Verschiedene Betriebssysteme haben jedoch unterschiedliche Konventionen für
Pfadnamen. Das Zeichen, das zur Trennung von aufeinanderfolgenden Datei- oder
Verzeichnisnamen in einem Linux/macOS-Pfadnamen verwendet wird, ist ``/``,
während es in einem Windows-Pfadnamen ``\`` ist. Außerdem hat das
Linux-Dateisystem ein einziges Stammverzeichnis auf das durch ein ``/``-Zeichen
als erstes Zeichen im Pfadnamen verwiesen wird, während das Windows-Dateisystem
für jedes Laufwerk ein eigenes Stammverzeichnis hat, das mit ``C:\`` :abbr:`usw.
(und so weiter)` bezeichnet wird. Aufgrund dieser Unterschiede haben die
Dateien auf den verschiedenen Betriebssystemen unterschiedliche Pfadnamen. Eine
Datei namens :samp:`C:\data\myfile` unter Windows könnte unter Linux und macOS
:samp:`/data/myfile` sein. Python bietet Funktionen und Konstanten, mit denen
ihr gängige Pfadnamenmanipulationen durchführen könnt, ohne sich um solche
syntaktischen Details kümmern zu müssen. Mit ein wenig Sorgfalt können ihr eure
Python-Programme so schreiben, dass sie unabhängig vom zugrunde liegenden
Dateisystem korrekt ausgeführt werden.

Absolute und relative Pfade
~~~~~~~~~~~~~~~~~~~~~~~~~~~

Diese Betriebssysteme erlauben zwei Arten von Pfadnamen:

Absolute Pfadnamen
    geben die genaue Position einer Datei im Dateisystem eindeutig an, indem sie
    den gesamten Pfad zu dieser Datei auflisten, beginnend mit dem
    Wurzelverzeichnis des Dateisystems.

    Als Beispiele seien hier zwei absolute Windows-Pfadnamen genannt:

    .. code-block:: console

        C:\Program Files\Python 3.13\
        D:\backup\2022\06\

    Und hier sind zwei absolute Linux-Pfadnamen und ein absoluter
    macOS-Pfadname:

    .. code-block:: console

        /bin/python3
        /cdrom/backup/2022/06/
        /Applications/Python\ 3.13/

Relative Pfadnamen
    geben die Position einer Datei relativ zu einem anderen Punkt im Dateisystem
    an, und dieser andere Punkt wird nicht im relativen Pfadnamen selbst
    angegeben.

    Als Beispiel sei hier ein relativer Windows-Pfadnamen genannt:

    .. code-block:: console

        save-data\filesystem.rst

    … und hier ein relativer Linux/macOS-Pfadname:

    .. code-block:: console

        save-data/filesystem.rst

    Relative Pfade benötigen also einen Kontext, in dem sie verankert sind.
    Dieser Kontext wird in der Regel auf eine der beiden folgenden Arten
    bereitgestellt:

    * Der relative Pfad wird an einen vorhandenen absoluten Pfad anzuhängt,
      wodurch ein neuer absoluter Pfad entsteht. Wenn ihr einen relativen
      Windows-Pfad :samp:`{Start Menu\\Programs\\Python 3.13}` und einen
      absoluten Pfad :samp:`{C:\\Users\\Veit}` habt, dann kann durch Anhängen
      des relativen Pfads ein neuer absoluter Pfad:
      :samp:`C:\\Users\\Veit\\Start Menu\\Programs\\Python 3.13` erstellt
      werden. Wenn ihr denselben relativen Pfad an einen anderen absoluten Pfad
      anhängt (:abbr:`z.B. (zum Beispiel)` an :samp:`C:\\Users\\Tim`, so
      erhaltet ihr einen neuen Pfad, der sich auf ein anderes
      :samp:`HOME`-Verzeichnis (:samp:`{Tim}`) bezieht.
    * Relative Pfade können auch einen Kontext erhalten durch den impliziten
      Verweis auf das aktuelle Arbeitsverzeichnis, also das Verzeichnis, in dem
      sich ein Python-Programm zum Zeitpunkt seiner Ausführung, befindet.
      Python-Befehle können implizit auf das aktuelle Arbeitsverzeichnis
      zurückgreifen, wenn ihnen ein relativer Pfad als Argument übergeben wird.
      Wenn ihr :abbr:`z.B. (zum Beispiel)` den Befehl
      :samp:`os.listdir('{RELATIVE/PATH}')` mit einem relativen Pfadargument
      verwendet, ist der Anker für diesen relativen Pfad das aktuelle
      Arbeitsverzeichnis, und das Ergebnis des Befehls ist eine Liste der
      Dateinamen in dem Verzeichnis, dessen Pfad durch Anhängen des aktuellen
      Arbeitsverzeichnisses an das relative Pfadargument gebildet wird.

      Das Verzeichnis, in dem sich eine Python-Datei befindet, wird als
      *current working directory* (:abbr:`engl. (englisch)`: aktuelles
      Arbeitsverzeichnis) bezeichnet. Dieses Verzeichnis wird sich meist von dem
      Verzeichnis unterscheiden, in dem sich der Python-Interpreter befindet. Um
      dies zu verdeutlichen, starten wir Python und verwenden den Befehl
      :func:`python3:os.getcwd`, um das aktuelle Arbeitsverzeichnis von Python
      zu ermitteln:

      .. code-block:: pycon

         >>> import os
         >>> os.getcwd()
         '/home/veit'

      .. note::
         ``os.getcwd()`` wird als Funktionsaufruf ohne Argumente verwendet um zu
         verdeutlichen, dass der zurückgegebene Wert keine Konstante ist,
         sondern sich ändert, wenn ihr den Wert des aktuellen
         Arbeitsverzeichnisses ändert. Im obigen Beispiel ist das Ergebnis das
         Home-Verzeichnis auf einem meiner Linux-Rechner. Auf Windows-Rechnern
         würden zusätzliche Backslashes in den Pfad eingefügt:
         ``C:\\Users\\Veit``, da Windows den Backslash ``\`` als Pfadseparator
         verwendet, der in :doc:`/types/strings/index` jedoch eine andere
         Bedeutung hat.

      Um euch die Inhalte des aktuellen Verzeichnisses anzeigen zu lassen,
      könnt ihr folgendes eingeben:

      .. code-block:: pycon

         >>> os.listdir(os.curdir)
         ['.gnupg', '.bashrc', '.local', '.bash_history', '.ssh', '.bash_logout', '.profile', '.idlerc', '.viminfo', '.config', 'Downloads', 'Documents', '.python_history']

      Ihr könnt jedoch auch in ein anderes Verzeichnis wechseln und euch dann
      das aktuelle Arbeitsverzeichnis ausgeben lassen:

      .. code-block:: pycon

         >>> os.chdir("Downloads")
         >>> os.getcwd()
         '/home/veit/Downloads'

Pfadnamen ändern
~~~~~~~~~~~~~~~~

Python bietet einige Möglichkeiten zum Ändern der Pfadnamen mit dem Submodul
:doc:`os.path <python3:library/os.path>`, ohne explizit eine
betriebssystemspezifische Syntax verwenden zu müssen.

:func:`python3:os.path.join`
    konstruiert Pfadnamen für verschiedene Betriebssysteme, :abbr:`z.B. (zum
    Beispiel)` unter Windows:

    .. code-block:: pycon

       >>> import os
       >>> print(os.path.join("save-data", "filesystem.rst"))
       save-data\filesystem.rst

    Dabei werden die Argumente interpretiert als eine Reihe von
    Verzeichnis- oder Dateinamen, die zu einer einzigen Zeichenkette verbunden
    werden sollen, die vom zugrunde liegenden Betriebssystem als relativer Pfad
    verstanden wird. Unter Windows bedeutet dies, dass die Namen der
    Pfadkomponenten mit Backslashes (``\``) verbunden werden.

    Wenn ihr das Gleiche unter Linux/macOS ausführt, erhaltet ihr hingegen als
    Separator ``/``:

    .. code-block:: pycon

       >>> import os
       >>> print(os.path.join("save-data", "filesystem.rst"))
       save-data/filesystem.rst

    Ihr könnt mit dieser Methode also Dateipfade unabhängig vom Betriebssystem,
    auf dem euer Programm läuft, erstellen.

    Die Argumente müssen auch nicht unbedingt einzelne Verzeichnis- oder
    Dateinamen sein; sie können auch Unterpfade sein, die dann zu einem längeren
    Pfadnamen zusammengefügt werden. Das folgende Beispiel veranschaulicht dies
    unter Windows, wobei entweder Schrägstriche (``/``) oder doppelte
    Backslashes (``\\``) in den Zeichenketten verwendet werden können:

    .. code-block:: pycon

       >>> import os
       >>> print(
       ...     os.path.join(
       ...         "python-basics-tutorial-de\\docs", "save-data\\filesystem.rst"
       ...     )
       ... )
       python-basics-tutorial-de\docs\save-data\filesystem.rst

:func:`os.path.split`
    gibt ein Tupel mit zwei Elementen zurück, das den Basisnamen eines Pfades
    vom Rest des Pfades trennt, :abbr:`z.B. (zum Beispiel)` unter macOS:

    .. code-block:: pycon

       >>> import os
       >>> print(os.path.split(os.getcwd()))
       ('/home/veit/python-basics-tutorial-de', 'docs')

:func:`python3:os.path.basename`
    gibt nur den Basisnamen des Pfades zurück:

    .. code-block:: pycon

       >>> import os
       >>> print(os.path.basename(os.getcwd()))
       docs

:func:`python3:os.path.dirname`
    gibt den Pfad bis zum Basisnamen zurück:

    .. code-block:: pycon

       >>> import os
       >>> print(os.path.dirname(os.getcwd()))
       /home/veit/python-basics-tutorial-de

:func:`python3:os.path.splitext`
    gibt die gepunktete Erweiterungsnotation aus, die von den meisten
    Dateisystemen verwendet wird, um den Dateityp anzugeben:

    .. code-block:: pycon

       >>> import os
       >>> print(os.path.splitext("filesystem.rst"))
       ('filesystem', '.rst')

    Das letzte Element des zurückgegebenen Tupels enthält die gepunktete
    Erweiterung der angegebenen Datei.

:func:`python3:os.path.commonpath`
    ist eine spezialisiertere Funktionen, um Pfadnamen zu manipulieren. Sie
    findet den gemeinsamen Pfad für eine Gruppe von Pfaden und ist so gut
    geeignet um das Verzeichnis der untersten Ebene zu finden, das jede Datei
    in einer Gruppe von Dateien enthält:

    .. code-block:: pycon

       >>> import os
       >>> print(os.path.commonpath(["save-data/filesystem.rst", "save-data/index.rst"]))
       save-data

:func:`python3:os.path.expandvars`
    erweitert Umgebungsvariablen in Pfaden:

    .. code-block:: pycon

       >>> os.path.expandvars("$HOME/python-basics-tutorial-de")
       '/home/veit/python-basics-tutorial-de'

Nützliche Konstanten und Funktionen
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

:data:`python3:os.name`
    gibt den Namen des Python-Moduls zurück, das importiert wurde, um die
    betriebssystemspezifischen Details zu handhaben, :abbr:`z.B. (zum
    Beispiel)`:

    .. code-block:: pycon

       >>> import os
       >>> os.name
       'nt'

    .. note::
       Die meisten Versionen von Windows, mit Ausnahme von Windows CE, werden
       als ``nt`` identifiziert.

    Auf macOS und Linux lautet die Antwort ``posix``. Je nach Plattform könnt
    ihr mit dieser Antwort spezielle Operationen durchführen:

    .. code-block:: pycon

       >>> import os
       >>> if os.name == "posix":
       ...     root_dir = "/"
       ... elif os.name == "nt":
       ...     root_dir = "C:\\"
       ... else:
       ...     print("The operating system was not recognised!")
       ...

Informationen über Dateien erhalten
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Dateipfade zeigen Dateien und Verzeichnisse auf eurer Festplatte an. Um mehr
über sie zu erfahren, gibt es verschiedene Python-Funktionen, :abbr:`u.a. (unter
anderem)`

:func:`python3:os.path.exists`
    gibt ``True`` zurück, wenn sein Argument ein Pfad ist, der mit einem im
    Dateisystem existierenden Pfad übereinstimmt.
:func:`python3:os.path.isfile`
    gibt ``True`` zurück, wenn und nur wenn der angegebene Pfad auf eine Datei
    hinweist, und gibt andernfalls ``False`` zurück, einschließlich der
    Möglichkeit, dass das Pfadargument auf nichts im Dateisystem hinweist.
:func:`python3:os.path.isdir`
    gibt ``True`` zurück, wenn und nur wenn sein Pfadargument auf ein
    Verzeichnis hinweist; andernfalls gibt es ``False`` zurück.

Weitere ähnliche Funktionen stellen speziellere Abfragen bereit:

:func:`python3:os.path.islink`
    gibt ``True`` zurück, wenn ein Pfad eine Datei angibt, die ein Link ist.
    Windows-Verknüfungsdateien mit der Endung ``.lnk`` sind jedoch in diesem
    Sinne keine echten Links und geben ``False`` zurück. Nur mit ``mklink()``
    erstellte Links geben ebenfalls ``True`` zurück.
:func:`python3:os.path.ismount`
    gibt unter ``possix``-Dateisystemen ``True`` zurück, wenn der Pfad ein
    :abbr:`sog. (sogenannter)` *Mount Point* oder Einhängepunkt ist.
:func:`python3:os.path.samefile`
    gibt ``True`` zurück, wenn die beiden Pfadargumente auf dieselbe Datei
    zeigen.
:func:`python3:os.path.isabs`
    gibt ``True`` zurück, wenn sein Argument ein absoluter Pfad ist; andernfalls
    wird ``False`` zurückgegeben.
:func:`python3:os.path.getsize`
    gibt die Größe der Datei oder des Verzeichnisses an.
:func:`python3:os.path.getmtime`
    gibt das Änderungsdatum der Datei oder des Verzeichnisses an.
:func:`python3:os.path.getatime`
    gibt de letzte Zugriffszeit für eine Datei oder ein Verzeichnis an.

Weitere Dateisystem-Operationen
-------------------------------

Python verfügt über weitere, sehr nützlicher Befehle im :mod:`python3:os`-Modul:
Im Folgenden beschreibe ich nur einige betriebssystemübergreifende Operationen,
es werden jedoch auch spezifischere Dateisystem-Funktionen bereitgestellt.

:func:`os.rename`
    benennt oder verschiebt eine Datei oder ein Verzeichnis, :abbr:`z.B. (zum
    Beispiel)`

    .. code-block:: pycon

       >>> os.rename("filesystem.rst", "save-data/filesystem.rst")

:func:`os.remove`
    löscht Dateien, :abbr:`z.B. (zum Beispiel)`

    .. code-block:: pycon

       >>> os.remove("filesystem.rst")

:func:`os.rmdir`
    löscht ein leeres Verzeichnis. Um nicht leere Verzeichnisse zu entfernen,
    verwendet :func:`shutil.rmtree`; diese Funktion entfernt rekursiv alle
    Dateien in einem Verzeichnisbaum.

:func:`os.makedirs`
    erstellt ein Verzeichnis mit allen notwendigen Zwischenverzeichnissen, :abbr:`z.B. (zum Beispiel)`

    .. code-block:: pycon

       >>> os.makedirs("save-data/filesystem")

Verarbeitung aller Dateien in einem Verzeichnis
-----------------------------------------------

Eine nützliche Funktion zum rekursiven Durchlaufen von Verzeichnisstrukturen ist
die Funktion :func:`os.walk`. Mit ihr könnt ihr einen ganzen Verzeichnisbaum
durchlaufen und für jedes Verzeichnis den Pfad dieses Verzeichnisses, eine Liste
seiner Unterverzeichnisse und eine Liste seiner Dateien zurückgeben. Dabei kann
sie drei optionale Argumente haben: ``os.walk(directory, topdown=True,
onerror=None, followlinks= False)``.

``directory``
    ist der Pfad des Startverzeichnisses
``topdown``
    auf ``True`` oder nicht vorhanden, verarbeitet die Dateien in jedem
    Verzeichnis vor den Unterverzeichnissen, was zu einer Auflistung führt, die
    oben beginnt und nach unten geht;

    auf ``False`` werden die Unterverzeichnisse jedes Verzeichnisses zuerst
    verarbeitet, was eine Durchquerung des Baums von unten nach oben ergibt.

``onerror``
    kann auf eine Funktion gesetzt werden, um Fehler zu behandeln, die aus
    Aufrufen von :func:`os.listdir` resultieren, die standardmäßig ignoriert
    werden. Üblicherweise wird symbolische Links nicht gefolgt, es sei denn, ihr
    gebt den Parameter ``follow-links=True`` an.

.. code-block:: pycon
    :linenos:

    >>> import os
    >>> for root, dirs, files in os.walk(os.curdir):
    ...     print("{0} has {1} files".format(root, len(files)))
    ...     if ".ipynb_checkpoints" in dirs:
    ...         dirs.remove(".ipynb_checkpoints")
    ...
    . has 13 files
    ./control-flows has 13 files
    ./save-data has 30 files
    ./test has 15 files
    ./test/coverage has 3 files
    …

Zeile 4
    prüft auf ein Verzeichnis namens ``.ipynb_checkpoints``.
Zeile 5
    entfernt ``.ipynb_checkpoints`` aus der Verzeichnisliste.

:func:`shutil.copytree` erstellt rekursiv Kopien aller Dateien eines
Verzeichnisses und all seiner Unterverzeichnisse, wobei die Informationen über
den Zugriffsmodus und den Status (:abbr:`d.h. (das heißt)` die
Zugriffs- und Änderungszeiten) erhalten bleiben. :mod:`shutil` verfügt auch über
die bereits erwähnte Funktion :func:`shutil.rmtree` zum Entfernen eines
Verzeichnisses und aller seiner Unterverzeichnisse sowie über mehrere Funktionen
zum Erstellen von Kopien einzelner Dateien.
