Konfiguration
=============

Mit Konfigurationsdateien könnt ihr den Ablauf von pytest beeinflussen. Wenn ihr
immer wieder bestimmte Optionen in euren Tests verwendet, wie :samp:`--verbose`
oder :samp:`--strict-markers`, könnt ihr diese in einer Konfigurationsdatei
ablegen und müsst sie nicht immer wieder eingeben. Zusätzlich zu den
Konfigurationsdateien gibt es eine Handvoll anderer Dateien, die bei der
Verwendung von pytest nützlich sind, um die Arbeit beim Schreiben und Ausführen
von Tests zu erleichtern:

:file:`pytest.ini`
    Dies ist die wichtigste Konfigurationsdatei von pytest, mit der ihr das
    Standardverhalten von pytest ändern könnt. Sie legt auch das
    Stammverzeichnis von pytest fest, oder ``rootdir``.
:file:`conftest.py`
    Diese Datei enthält :doc:`fixtures` und Hook-Funktionen. Sie kann in
    ``rootdir`` oder in einem beliebigen Unterverzeichnis existieren.
:file:`__init__.py`
    Wenn diese Datei in Test-Unterverzeichnissen abgelegt wird, ermöglicht sie
    die Verwendung identischer Testdateinamen in mehreren Testverzeichnissen.

Wenn ihr bereits eine :file:`tox.ini`, :file:`pyproject.toml` oder
:file:`setup.cfg` in eurem Projekt habt, können sie an die Stelle der
:file:`pytest.ini`-Datei treten: :file:`tox.ini` wird von :doc:`../tox`
verwendet, :file:`pyproject.toml` und :file:`setup.cfg` werden für die
Paketierung von Python-Projekten verwendet und können zum Speichern von
Einstellungen für verschiedene Werkzeuge, einschließlich pytest, verwendet
werden.

Ihr solltet eine Konfigurationsdatei haben, entweder :file:`pytest.ini`, oder
einem ``pytest``-Abschnitt in :file:`tox.ini`, :file:`pyproject.toml` oder in
:file:`setup.cfg`.

Sie Konfigurationsdatei legt das oberste Verzeichnis fest, von dem aus
``pytest`` gestartet wird.

Schauen wir uns einige dieser Dateien im Zusammenhang mit einer
Projekt-Verzeichnisstruktur an:

.. code-block:: console
   :emphasize-lines: 3, 7, 8

    items
    ├── …
    ├── pytest.ini
    ├── src
    │   └── …
    └── tests
        ├── __init__.py
        ├── conftest.py
        └── test_….py

Im Falle des ``items``-Projekts, das wir bisher zum Testen verwendet haben, gibt
es auf der obersten Ebene eine :file:`pytest.ini`-Datei und ein Verzeichnis
:file:`tests`. Wir werden uns auf diese Struktur beziehen, wenn wir im weiteren
Verlauf dieses Abschnitts über die verschiedenen Dateien sprechen.

Speichern von Einstellungen und Optionen in :file:`pytest.ini`
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: ini

   [pytest]
   addopts =
       --strict-markers
       --strict-config
       -ra
   testpaths = tests
   markers =
       smoke: Small subset of all tests
       exception: Only run expected exceptions

``[pytest]`` kennzeichnet den Beginn des pytest-Abschnitts. Danach folgen die
einzelnen Einstellungen. Bei Konfigurationseinstellungen, die
mehr als einen Wert zulassen, können die Werte entweder in eine oder in mehrere
Zeilen geschrieben werden in der Form :samp:`{EINSTELLUNG} = {WERT1} {WERT2}`.
Bei ``markers`` hingegen ist nur ein Marker pro Zeile erlaubt.

Dieses Beispiel ist eine einfache :file:`pytest.ini`-Datei, die ich so, oder so
ähnlich in fast allen meinen Projekten verwende. Gehen wir kurz die einzelnen
Zeilen durch:

.. _addopts:

``addopts``
    erlaubt die Angabe der pytest-Optionen, die wir immer in diesem Projekt
    ausführen wollen.
``--strict-markers``
    weist pytest an, bei jedem nicht registrierten Marker, der im Testcode
    auftaucht, einen Fehler statt einer Warnung auszugeben. Hierdurch können wir
    Tippfehler bei Marker-Namen vermeiden.
``--strict-config``
    weist pytest an, wenn beim Parsen von Konfigurationsdateien Schwierigkeiten
    auftauchen, nicht nur eine Warnung sondern einen Fehler auszugeben. Damit
    vermeiden wir, dass Tippfehler in der Konfigurationsdatei unbemerkt bleiben.
``-ra``
    weist pytest an, am Ende eines Testlaufs nicht nur zusätzliche Informationen
    zu *Failures* und *Errors* anzuzeigen sondern auch eine Testzusammenfassung.

    ``-r``
        zeigt zusätzliche Informationen zur Testzusammenfassung an.
    ``a``
        zeigt alle außer den bestanden Tests an. Dies fügt den *Failures* und
        *Errors* die Informationen ``skipped``, ``xfailed`` oder ``xpassed``
        hinzu.

``testpaths = tests``
    teilt pytest mit, wo es nach Tests suchen soll, wenn ihr auf der
    Kommandozeile keinen Datei- oder Verzeichnisnamen angegeben habt. In unserem
    Fall sucht pytest im :file:`tests`-Verzeichnis.

    Auf den ersten Blick mag es überflüssig erscheinen, ``testpaths`` auf
    :file:`tests` zu setzen, da pytest sowieso dort sucht, und wir keine
    :file:`test_`-Dateien in unseren :file:`src`- oder
    :file:`docs`-Verzeichnissen haben. Allerdings kann die Angabe eines
    :file:`testpaths`-Verzeichnisses ein wenig Startzeit sparen, besonders wenn
    unsere :file:`src`- oder :file:`docs`- oder andere Verzeichnisse recht groß
    sind.

``markers =``
    wird verwendet, um Marker zu deklarieren, wie in
    :ref:`select-tests-with-markers` beschrieben.

.. seealso::
   In den Konfigurationsdateien könnt ihr viele weitere
   Konfigurationseinstellungen und Befehlszeilen-Optionen angeben, die ihr euch
   mit dem Befehl ``pytest --help`` anzeigen lassen könnt.

Andere Konfigurationsdateien verwenden
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Wenn ihr Tests für ein Projekt schreibt, das bereits eine
:file:`pyproject.toml`, :file:`tox.ini` oder :file:`setup.cfg`-Datei hat, könnt
ihr :file:`pytest.ini` verwenden, um eure pytest-Konfigurationseinstellungen zu
speichern, oder ihr könnt eure Konfigurationseinstellungen in einer dieser
alternativen Konfigurationsdateien speichern. Die Syntax der beiden
Nicht-ini-Dateien unterscheidet sich ein wenig, daher werden wir uns beide
Dateien genauer ansehen.

:file:`pyproject.toml`
::::::::::::::::::::::

Die :file:`pyproject.toml`-Datei war ursprünglich für die Paketierung von
Python-Projekten gedacht; sie kann jedoch auch für die Definition von
Projekteinstellungen verwendet werden.

Da :doc:`Python4DataScience:data-processing/serialisation-formats/toml/index`
ein anderer Standard für Konfigurationsdateien ist als :file:`.ini`-Dateien, ist
das Format auch ein wenig anders:

.. code-block:: toml

   [tool.pytest.ini_options]
   addopts = [
       "--strict-markers",
       "--strict-config",
       "-ra"
       ]
   testpaths = "tests"
   markers = [
       "exception: Only run expected exceptions",
       "finish: Only run finish tests",
       "smoke: Small subset of all tests",
       "num_items: Number of items to be pre-filled for the items_db fixture"
       ]

Anstelle von ``[pytest]`` beginnt der Abschnitt mit
``[tool.pytest.ini_options]``, die Werte müssen in Anführungszeichen gesetzt
werden und Listen von Werten müssen Listen von Zeichenketten in eckigen Klammern
sein.

:file:`setup.cfg`
:::::::::::::::::

Das Dateiformat der :file:`setup.cfg` entspricht einer :file:`.ini`-Datei:

.. code-block:: ini

   [tool:pytest]
   addopts =
       --strict-markers
       --strict-config
       -ra
   testpaths = tests
   markers =
       smoke: Small subset of all tests
       exception: Only run expected exceptions

Der einzige Unterschied zwischen dieser und der :file:`pytest.ini` ist die
Angabe des Abschnitts ``[tool:pytest]``.

.. warning::
   Der Parser der :file:`.cfg`-Datei unterscheidet sich jedoch vom Parser der
   :file:`.ini`-Datei, und dieser Unterschied kann Probleme verursachen, die
   schwer aufzuspüren sind, :abbr:`s.a. (siehe auch)` `pytest-Dokumentation
   <https://docs.pytest.org/en/latest/reference/customize.html#setup-cfg>`_.

``rootdir`` festlegen
---------------------

Noch bevor pytest nach auszuführenden Testdateien sucht, liest es die
Konfigurationsdatei :file:`pytest.ini`, :file:`tox.ini`, :file:`pyproject.toml`
oder :file:`setup.cfg`, die einen pytest-Abschnitt enthält:

* wenn ihr ein Testverzeichnis angegeben habt, beginnt pytest dort zu suchen
* wenn ihr mehrere Dateien oder Verzeichnisse angegeben habt, beginnt pytest mit
  dem übergeordneten Verzeichnis
* wenn ihr keine Datei oder kein Verzeichnis angebt, beginnt pytest im aktuellen
  Verzeichnis.

Wenn pytest eine Konfigurationsdatei im Startverzeichnis findet, ist das die
Wurzel und wenn nicht, geht pytest den Verzeichnisbaum hoch, bis es eine
Konfigurationsdatei findet, die einen pytest-Abschnitt enthält. Sobald pytest
eine Konfigurationsdatei gefunden hat, markiert es das Verzeichnis, in dem es
sie gefunden hat, als ``rootdir``. Dieses Wurzelverzeichnis ist auch die
relative Wurzel der IDs. pytest sagt euch auch, wo es eine Konfigurationsdatei
gefunden hat. Durch diese Regeln können wir Tests auf verschiedenen Ebenen
durchführen und sicher sein, dass pytest die richtige Konfigurationsdatei
findet:

.. code-block:: pytest
   :emphasize-lines: 5, 6

   $ cd items
   $ pytest
   ============================= test session starts ==============================
   …
   rootdir: /Users/veit/cusy/prj/items
   configfile: pyproject.toml
   testpaths: tests
   plugins: Faker-19.11.0
   collected 39 items
   …

:file:`conftest.py` für die gemeinsame Nutzung von lokalen Fixtures und Hook-Funktionen
---------------------------------------------------------------------------------------

Die :file:`conftest.py`-Datei wird verwendet, um Fixtures und Hook-Funktionen zu
speichern, :abbr:`s.a. (siehe auch)` :doc:`fixtures` und :doc:`plugins`. Ihr
könnt so viele :file:`conftest.py`-Dateien in einem Projekt haben, wie ihr
wollt. Alles, was in einer :file:`conftest.py`-Datei definiert ist, gilt für
Tests in diesem Verzeichnis und allen Unterverzeichnissen.
Wenn ihr eine :file:`conftest.py`-Datei auf der obersten Testebene habt, können
die dort definierten Fixtures für alle Tests verwendet werden. Wenn es dann
spezielle Fixtures gibt, die nur für ein Unterverzeichnis gelten, können diese
in einer anderen :file:`conftest.py`-Datei in diesem Unterverzeichnis definiert
werden. Zum Beispiel könnten die CLI-Tests andere Fixtures benötigen als die
API-Tests, und einige könnt ihr auch gemeinsam nutzen.

.. tip::
   Es ist jedoch eine gute Idee, nur eine einzige :file:`conftest.py`-Datei zu
   halten, damit ihr die Fixture-Definitionen leicht finden können. Auch wenn
   wir mit :samp:`pytest --fixtures -v` immer herausfinden können, wo eine
   Fixture definiert ist, so ist es dennoch einfacher, wenn sie immer in der
   einen :file:`conftest.py`-Datei definiert ist.

:file:`__init__.py` um Kollision von Testdateinamen zu vermeiden
----------------------------------------------------------------

Die :file:`__init__.py`-Datei erlaubt es, doppelte Testdateinamen zu haben. Wenn
ihr :file:`__init__.py`-Dateien in jedem Test-Unterverzeichnis habt, könnt ihr
denselben Testdateinamen in mehreren Verzeichnissen verwenden, :abbr:`z.B. (zum
Beispiel)`:

.. code-block:: console
   :emphasize-lines: 8, 11

    items
    ├── …
    ├── pytest.ini
    ├── src
    │   └── …
    └── tests
        ├── api
        │   ├── __init__.py
        │   └── test_add.py
        ├── cli
        │   ├── __init__.py
        │   ├── conftest.py
        │   └── test_add.py
        └── conftest.py

Nun können wir die ``add``-Funktionalität sowohl über die :abbr:`API
(Anwendungsprogrammierschnittstelle)` als auch über die :abbr:`CLI
(Befehlszeilenschnittstelle)` testen, wobei eine :file:`test_add.py`
in beiden Verzeichnissen liegt:

.. code-block:: pytest

    $ pytest
    ============================= test session starts ==============================
    …
    rootdir: /Users/veit/cusy/prj/items
    configfile: pyproject.toml
    testpaths: tests
    plugins: Faker-19.11.0
    collected 6 items

    tests/api/test_add.py ....                                               [ 66%]
    tests/cli/test_add.py ..                                                 [100%]

    ============================== 6 passed in 0.03s ===============================


----

Die meisten meiner Projekte starten mit folgender Konfiguration:

.. code-block:: ini

   addopts =
      --strict-markers
      --strict-config
      -ra

.. seealso::
   * `Configuration
     <https://docs.pytest.org/en/latest/reference/customize.html>`_
   * `Configuration Options
     <https://docs.pytest.org/en/latest/reference/reference.html#configuration-options>`_
