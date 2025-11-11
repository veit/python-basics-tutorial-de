Dateien und Verzeichnisse
=========================

:mod:`python3:pathlib` implementiert Pfadoperationen mithilfe von
:class:`python3:pathlib.PurePath`- und :class:`python3:pathlib.Path`-Objekten.
Die :mod:`python3:os`- und :mod:`python3:os.path`-Module hingegen bieten
Funktionen, die low-level mit ``str``- und ``bytes`` arbeiten, was eher einem
prozeduralen Ansatz entspricht. Wir halten den objektorientierten Stil von
:mod:`python3:pathlib` für besser lesbar und stellen ihn deswegen hier
ausführlicher vor.

.. seealso::
   * :pep:`428`: The pathlib module – object-oriented filesystem paths
   * :ref:`os-comparison`
   * `Why you should be using pathlib <https://treyhunner.com/2018/12/why-you-should-be-using-pathlib>`_
   * `No really, pathlib is great <https://treyhunner.com/2019/01/no-really-pathlib-is-great>`_

Lesen und Schreiben von Dateien
-------------------------------

In Python öffnet und lest ihr eine Datei, indem ihr die Funktion
:meth:`python3:pathlib.Path.open` und verschiedene eingebaute Leseoperationen
verwendet. :meth:`python3:pathlib.Path.open` öffnet die Datei, auf die der Pfad
verweist, wie es die integrierte Funktion :func:`python3:open` tut. Das folgende
kurze Python-Programm liest eine Zeile aus einer Textdatei namens
:file:`myfile.txt` in :file:`docs/save-data/` ein:

.. code-block:: pycon
   :linenos:

   >>> from pathlib import Path
   >>> p = Path("docs", "save-data", "myfile.txt")
   >>> f = p.open()
   >>> headline = f.readline()

Zeile 2:
    Die Argumente von :class:`python3:pathlib.Path` sind Pfadsegmente, entweder
    als :class:`PosixPath <pathlib.PosixPath>` oder :class:`WindowsPath
    <pathlib.WindowsPath>`. Im vorigen Beispiel öffnet ihr eine Datei, von der
    ihr annehmt, dass sie sich relativ zu eurem Aufruf im
    :file:`docs/save-data/myfile.txt` befindet.

    Das folgende Beispiel öffnet eine Datei an einem absoluten Speicherort –
    :file:`C:\\My Documents\\myfile.txt`:

    .. code-block:: pycon
       :lineno-start: 2

       >>> p = Path("C:/", "Users", "Veit", "My Documents", "myfile.txt")
       >>> with p.open() as f:
       ...     f.readline()
       ...

    .. note::
       In diesem Beispiel wird das Schlüsselwort ``with`` verwendet, :abbr:`d.h.
       (das heißt)`, dass die Datei mit einem Kontextmanager geöffnet wird, der
       in :doc:`/control-flow/with` näher erläutert wird. Diese Art des Öffnens
       von Dateien verwaltet mögliche I/O-Fehler besser und sollte im
       Allgemeinen bevorzugt werden.

Zeile 3:
    :meth:`python3:pathlib.Path.open` liest nichts aus der Datei, sondern gibt
    ein Datei-Objekt zurück, mit dem ihr auf die geöffnete Datei zugreifen
    könnt. Es behält den Überblick über eine Datei und darüber, wie viel von der
    Datei gelesen oder geschrieben wurde. Alle Dateieingaben in Python werden
    mit Dateiobjekten und nicht mit Dateinamen durchgeführt.

Zeile 4:
    Der erste Aufruf von :meth:`readline() <codecs.StreamReader.readline>` gibt
    die erste Zeile des Datei-Objekts zurück, also alles bis einschließlich des
    ersten Zeilenumbruchs oder die gesamte Datei, wenn es keinen Zeilenumbruch
    in der Datei gibt; der nächste Aufruf von :func:`readline` gibt die zweite
    Zeile zurück, wenn sie existiert :abbr:`usw (und so weiter)`. Wenn es nichts
    mehr zu lesen gibt, gibt :meth:`readline() <codecs.StreamReader.readline>`
    einen leeren String zurück.

Dieses Verhalten von :meth:`readline() <codecs.StreamReader.readline>` macht es
einfach, :abbr:`z.B. (zum Beispiel)` die Anzahl der Zeilen in einer Datei zu
ermitteln:

.. code-block:: pycon

   >>> with p.open() as f:
   ...     lc = 0
   ...     while f.readline() != "":
   ...         lc = lc + 1
   ...     print(lc)
   ...
   2

Ein kürzerer Weg, alle Zeilen zu zählen, gibt es mit der ebenfalls eingebauten
:meth:`readlines() <codecs.StreamReader.readlines>`-Methode, die alle Zeilen
einer Datei liest und sie als Liste von Strings mit einen String pro Zeile
zurückgibt:

.. code-block:: pycon

   >>> with p.open() as f:
   ...     print(len(f.readlines()))
   ...
   2

Wenn ihr alle Zeilen einer großen Datei zählt, kann diese Methode jedoch dazu
führen, dass der Speicher vollläuft, weil die gesamte Datei auf einmal gelesen
wird. Es ist auch möglich, dass der Speicher mit :meth:`readline()
<codecs.StreamReader.readline>` überläuft, wenn ihr versucht, eine Zeile aus
einer großen Datei zu lesen, die keine Zeilenumbruchzeichen enthält. Um mit
solchen Situationen besser umgehen zu können, haben beide Methoden ein
optionales Argument, das die Menge der zu einem Zeitpunkt gelesenen Daten
beeinflusst. Eine andere Möglichkeit, über alle Zeilen einer Datei zu iterieren,
besteht darin, das Dateiobjekt als Iterator in einer :ref:`for-loop` zu
behandeln:

.. code-block:: pycon

   >>> with p.open() as f:
   ...     lc = 0
   ...     for l in f:
   ...         lc = lc + 1
   ...     print(lc)
   ...
   2

Diese Methode hat den Vorteil, dass die Zeilen je nach Bedarf in den Speicher
eingelesen werden, so dass selbst bei großen Dateien kein Speicherplatzmangel zu
befürchten ist. Der andere Vorteil dieser Methode ist, dass sie einfacher und
lesbarer ist.

Ein mögliches Problem mit der Lesemethode kann jedoch entstehen, wenn auf
Windows- und macOS Übersetzungen im Textmodus erfolgen, wenn ihr den Befehl
:func:`open` im Textmodus verwenden, :abbr:`d.h. (das heißt)` ohne ein ``b``
anzuhängen. Im Textmodus wird auf macOS jedes ``\r`` in ``\n`` umgewandelt,
während unter Windows ``\r\n``-Paare in ``\n`` umgewandelt werden. Ihr könnt die
Behandlung von Zeilenumbrüchen festlegen, indem ihr beim Öffnen der Datei den
Parameter ``newline`` verwendet und ``newline="\n"``, ``\r`` oder ``\r\n``
angebt, wodurch nur diese Zeichenfolge als Zeilenumbruch verwendet wird:

.. code-block:: pycon

   >>> with p.open(newline="\r\n") as f:
   ...     lc = 0
   ...

In diesem Beispiel wird nur ``\n`` als Zeilenumbruch gewertet. Wenn die Datei
jedoch im Binärmodus geöffnet wurde, ist der Parameter ``newline`` nicht
erforderlich, da alle Bytes genau so zurückgegeben werden, wie sie in der Datei
stehen.

:meth:`python3:pathlib.Path.read_text`
    gibt den dekodierten Inhalt der angegebenen Datei als Zeichenkette zurück:

    .. code-block:: pycon

       >>> p.read_text()
       'This is the first line of myfile.\nAnd this is another line.\n'

:meth:`python3:pathlib.Path.write_text`
    öffnet die angegebene Datei im Textmodus, schreibt Daten in sie und schließt
    die Datei:

    .. code-block:: pycon

       >>> p.write_text("New content")
       11
       >>> p.read_text()
       'New content'

    Eine vorhandene Datei mit demselben Namen wird überschrieben.

Verzeichnisse lesen
-------------------

:meth:`python3:pathlib.Path.iterdir`
    Wenn der Pfad auf ein Verzeichnis verweist, werden die Pfadobjekte des
    Verzeichnisinhalts zurückgegeben:

    .. code-block:: pycon

       >>> p = Path("docs", "save-data")
       >>> for child in p.iterdir():
       ...     child
       ...
       PosixPath('docs/save-data/index.rst')
       PosixPath('docs/save-data/minidom_example.py')
       PosixPath('docs/save-data/pickle.rst')
       PosixPath('docs/save-data/xml.rst')
       PosixPath('docs/save-data/books.xml')
       PosixPath('docs/save-data/files.rst')

Die Kind-Objekte werden in beliebiger Reihenfolge zurückgegeben, und die
speziellen Einträge ``.`` und ``..`` sind nicht enthalten. Wenn der Pfad kein
Verzeichnis oder anderweitig nicht zugänglich ist, wird ein
:exc:`python3:OSError` ausgelöst.

:meth:`python3:pathlib.Path.glob`
    findet das angegebene relative Muster im Verzeichnis, das durch diesen Pfad
    dargestellt wird, und gibt alle übereinstimmenden Dateien aus:

    .. code-block:: pycon

       >>> sorted(p.glob("*.rst"))
       [PosixPath('docs/save-data/files.rst'), PosixPath('docs/save-data/index.rst'), PosixPath('docs/save-data/pickle.rst'), PosixPath('docs/save-data/xml.rst')]

    .. seealso::
       :ref:`python3:pathlib-pattern-language`

:meth:`python3:pathlib.Path.rglob`
    findet das angegebene relative Muster rekursiv. Dies entspricht dem Aufruf
    mit ``**/`` vor dem Muster.

:meth:`python3:pathlib.Path.walk`
    generiert die Dateinamen in einer Verzeichnisstruktur, indem es die Struktur
    entweder von oben nach unten oder von unten nach oben durchläuft.
    Zurückgegeben wird ein 3-Tupel aus ``(dirpath, dirnames, filenames)``.

    Bei der Standardeinstellung des optionalen Arguments ``top_down=True`` wird
    das Triple für ein Verzeichnis vor den Triples für seine Unterverzeichnisse
    generiert.

    Bei ``follow_symlinks=True`` werden Symlinks aufgelöst und sie entsprechend
    ihren Zielen in ``dirnames`` und ``filenames`` platziert.

    Das folgende Beispiel zeigt die Größe der Dateien in einem Verzeichnis an,
    wobei :file:`__pycache__`-Verzeichnisse ignoriert werden:

    .. code-block:: pycon

       >>> for root, dirs, files in p.walk():
       ...     print(
       ...         root,
       ...         "consumes",
       ...         sum((root / file).stat().st_size for file in files),
       ...         "bytes in",
       ...         len(files),
       ...         "non-directory files",
       ...     )
       ...     if "__pycache__" in dirs:
       ...         dirs.remove("__pycache__")
       ...
       docs/save-data consumes 88417 bytes in 13 non-directory files
       docs/save-data/sqlite consumes 35187 bytes in 19 non-directory files

    Das nächste Beispiel ist eine einfache Implementierung von
    :func:`python3:shutil.rmtree`, wobei der Verzeichnisbaum von unten nach oben
    durchlaufen werden muss, da :meth:`python3:pathlib.Path.rmdir` das Löschen
    eines Verzeichnisses erst zulässt, wenn es leer ist:

    .. code-block:: pycon

       >>> for root, dirs, files in p.walk(top_down=False):
       ...     for name in files:
       ...         (root / name).unlink()
       ...     for name in dirs:
       ...         (root / name).rmdir()
       ...

    .. versionadded:: 3.12

Erstellen von Dateien und Verzeichnissen
----------------------------------------

:meth:`python3:pathlib.Path.touch`
    erstellt eine Datei unter dem angegebenen Pfad. Mit ``mode`` lassen sich
    Dateimodus und die Zugriffsflags angeben Wenn die Datei bereits existiert,
    wird die Änderungszeit auf die aktuelle Zeit aktualisiert sofern
    ``exist_ok=True`` ist, andernfalls wird ein :class:`python3:FileExistsError`
    ausgelöst.

    .. note::
       Um Dateien zu erstellen wird auch häufig
       :meth:`python3:pathlib.Path.open` oder :meth:`pathlib.Path.write_text`
       verwendet.

:meth:`python3:pathlib.Path.mkdir`
    erstellt ein neues Verzeichnis unter dem angegebenen Pfad. Die Parameter
    ``mode`` und ``exist_ok`` funktionieren wie in
    :meth:`python3:pathlib.Path.touch` angegeben.

    Wenn ``parents=True``, werden fehlende übergeordnete Verzeichnisse des Pfads
    nach Bedarf mit den Standardberechtigungen erstellt. Mit der
    Standardeinstellung ``parents=False`` wird hingegen
    :class:`python3:FileNotFoundError` ausgelöst.

Verschieben, Kopieren und Löschen
---------------------------------

:meth:`python3:pathlib.Path.rename`
    benennt die Datei oder das Verzeichnis in das angegebene Ziel um und gibt
    eine neue :class:`python3:pathlib.Path`-Instanz zurück, die auf das Ziel
    verweist. Unter Unix wird das Ziel, sofern es existiert und eine Datei ist,
    einfach ersetzt, unter Windows wird ein :class:`python3:FileExistsError`
    ausgelöst.

    .. code-block:: pycon

       >>> myfile = Path("docs", "save-data", "myfile.txt")
       >>> newfile = Path("docs", "newdir", "newfile.txt")
       >>> myfile.rename(newfile)
       PosixPath('docs/newdir/newfile.txt')

.. versionadded:: 3.14
   In Python 3.14 kommen die Methoden :meth:`pathlib.Path.copy`,
   :meth:`pathlib.Path.copy_into`, :meth:`pathlib.Path.move` und
   :meth:`pathlib.Path.move_into` hinzu.

   .. seealso::
      `Python 3.14 Changelog
      <https://docs.python.org/3.14/whatsnew/3.14.html#pathlib>`_

Berechtigungen und Eigentum
---------------------------

:meth:`python3:pathlib.Path.owner`
    gibt den Namen der Person zurück, der die Datei gehört. Normalerweise wird
    Symlinks gefolgt, wenn ihr jedoch die Person ermitteln wollt, der der
    Symlink gehört, fügt ``follow_symlinks=False`` hinzu. Wenn die User-ID (UID)
    der Datei nicht gefunden wird, wird :class:`python3:KeyError` ausgelöst.

:meth:`python3:pathlib.Path.group`
    gibt den Namen der Gruppe zurück, der die Datei gehört. Das Verhalten bei
    Symlinks entspricht dem vom :meth:`python3:pathlib.Path.owner`. Und wenn die
    Group-ID (GID) der Datei nicht gefunden wird, wird ebenfalls
    :class:`python3:KeyError` ausgelöst.

:meth:`python3:pathlib.Path.chmod`
    ändert den Dateimodus und die Berechtigungen.Dabei wird normalerweise
    Symlinks gefolgt. Für das Ändern der Symlink-Berechtigungen, könnt ihr
    ``follow_symlinks=False`` oder :meth:`python3:pathlib.Path.lchmod`
    verwenden.

.. _os-comparison:

Vergleich mit ``os`` und ``os.path``
------------------------------------

* :mod:`pathlib` implementiert mit :class:`pathlib.PurePath` und
  :class:`pathlib.Path` Objekte, während :mod:`os` und :mod:`os.path` eher
  prozedural mit low-level ``str``- und ``Bytes`` arbeiten.
* Viele Funktionen in :mod:`os` und :mod:`os.path` unterstützen  Pfade relativ
  zu Verzeichnisdeskriptoren. Diese Funktionen sind in :mod:`pathlib` nicht
  verfügbar.
* ``str`` und ``bytes`` sowie Teile von :mod:`:mod:`python3:os` und
  :mod:`python3:`os.path` sind in C geschrieben und sehr schnell.
  :mod:`pathlib` ist hingegen in Python geschrieben und oft langsamer, aber
  nicht immer spielt dies eine Rolle.

Trotz der Unterschiede lassen sich viele :mod:`python3:os`-Funktionen in
entsprechende :class:`python3:pathlib.Path`- oder
:class:`python3:pathlib.PurePath`-Funktionen übersetzen:

=====================================   ==============================================================
:mod:`os`                               :mod:`pathlib`
=====================================   ==============================================================
:func:`os.chmod`                        :meth:`pathlib.Path.chmod`
:func:`os.lstat`                        :meth:`pathlib.Path.lstat`
:func:`os.listdir`                      :meth:`pathlib.Path.iterdir`
:func:`os.getcwd`                       :meth:`pathlib.Path.cwd`
:func:`os.lchmod`                       :meth:`pathlib.Path.lchmod`
:func:`os.link`                         :meth:`pathlib.Path.hardlink_to`
:func:`os.mkdir`, :func:`os.makedirs`   :meth:`pathlib.Path.mkdir`
:func:`os.path.abspath`                 :meth:`pathlib.Path.absolute`
:func:`os.path.basename`                :attr:`pathlib.PurePath.name`
:func:`os.path.dirname`                 :attr:`pathlib.PurePath.parent`
:func:`os.path.exists`                  :meth:`pathlib.Path.exists`
:func:`os.path.expanduser`              :meth:`pathlib.Path.expanduser`
:func:`os.path.isabs`                   :meth:`pathlib.PurePath.is_absolute`
:func:`os.path.isdir`                   :meth:`pathlib.Path.is_dir`
:func:`os.path.isfile`                  :meth:`pathlib.Path.is_file`
:func:`os.path.isjunction`              :meth:`pathlib.Path.is_junction`
:func:`os.path.islink`                  :meth:`pathlib.Path.is_symlink`
:func:`os.path.ismount`                 :meth:`pathlib.Path.is_mount`
:func:`os.path.join`                    :meth:`pathlib.PurePath.joinpath`
:func:`os.path.realpath`                :meth:`pathlib.Path.resolve`
:func:`os.path.relpath`                 :meth:`pathlib.PurePath.relative_to`
:func:`os.path.samefile`                :meth:`pathlib.Path.samefile`
:func:`os.path.splitext`                :attr:`pathlib.PurePath.stem`, :attr:`pathlib.PurePath.suffix`
:func:`os.readlink`                     :meth:`pathlib.Path.readlink`
:func:`os.remove`, :func:`os.unlink`    :meth:`pathlib.Path.unlink`
:func:`os.rename`                       :meth:`pathlib.Path.rename`
:func:`os.replace`                      :meth:`pathlib.Path.replace`
:func:`os.rmdir`                        :meth:`pathlib.Path.rmdir`
:func:`os.stat`                         :meth:`pathlib.Path.stat`
:func:`os.symlink`                      :meth:`pathlib.Path.symlink_to`
:func:`os.walk`                         :meth:`pathlib.Path.walk`
=====================================   ==============================================================

Checks
------

* Verwendet die Funktionen des :mod:`python3:pathlib`-Moduls, um einen Pfad zu
  einer Datei namens :file:`instance.log` zu nehmen und einen neuen Dateipfad im
  selben Verzeichnis für eine Datei namens :file:`instance.log1` zu erstellen.

* Öffnet eine Datei :file:`my_file.txt` und fügt zusätzlichen Text am Ende der
  Datei ein. Welchen Befehl würdet ihr verwenden, um :file:`my_file.txt` zu
  öffnen? Welchen Befehl würdet ihr verwenden, um die Datei erneut zu öffnen und
  von Anfang an zu lesen?

* Wenn ihr euch die `Manpage für das wc-Dienstprogramm
  <https://linux.die.net/man/1/wc>`_ anseht, seht ihr zwei
  Befehlszeilenoptionen:

  ``-c``
      zählt die Bytes in der Datei
  ``-m``
      zählt die Zeichen, die im Falle einiger Unicode-Zeichen zwei oder mehr
      Bytes lang sein können

  Außerdem sollte unser Modul, wenn eine Datei angegeben wird, aus dieser Datei
  lesen und sie verarbeiten, aber wenn keine Datei angegeben wird, sollte es aus
  ``stdin`` lesen und verarbeiten.

* Wenn ein Kontext-Manager in einem Skript verwendet wird, das mehrere Dateien
  liest und/oder schreibt, welche der folgenden Ansätze wäre eurer Meinung nach
  am besten?

  #. Legt das gesamte Skript in einen Block, der von einer ``with``-Anweisung
     verwaltet wird.
  #. Verwendet eine ``with``-Anweisung für alle Lesevorgänge und eine weitere
     für alle Schreibvorgänge.
  #. Verwendet jedes Mal eine ``with``-Anweisung, wenn ihr eine Datei lest oder
     schreibt, :abbr:`d.h. (das heißt)` für jede Zeile.
  #. Verwendet für jede Datei, die ihr lest oder schreibt, eine
     ``with``-Anweisung.

* Archiviert :file:`*.txt`-Dateien aus dem aktuellen Verzeichnis im Verzeichnis
  :file:`archive` als :file:`*.zip`-Dateien mit dem aktuellen Datum als
  Dateiname.

  * Welche Module benötigt ihr hierfür?
  * Schreibt eine mögliche Lösung.
