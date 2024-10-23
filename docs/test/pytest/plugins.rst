Plugins
=======

So leistungsfähig pytest ist, es kann noch mehr, wenn wir Plugins hinzufügen.
Die Codebasis von pytest ist so konzipiert, dass Anpassungen und Erweiterungen
möglich sind, und es gibt Hooks, die Änderungen und Verbesserungen durch Plugins
ermöglichen.

Es wird euch vielleicht überraschen, dass ihr bereits einige Plugins geschrieben
habt, wenn ihr die vorherigen Abschnitte durchgearbeitet habt. Jedes Mal, wenn
ihr Fixtures oder Hook-Funktionen in die Datei :file:`conftest.py` eines
Projekts einfügt, erstellt ihr ein lokales Plugin. Es ist nur ein wenig
zusätzliche Arbeit, diese :file:`conftest.py`-Dateien in installierbare Plugins
umzuwandeln, die ihr zwischen Projekten, mit anderen Personen oder mit der
ganzen Welt teilen könnt.

Zunächst beginnen wir jedoch mit der Frage, wo ihr Plugins von Drittanbietern
finden könnt. Es gibt eine ganze Reihe von Plugins, so dass die
Wahrscheinlichkeit groß ist, dass Änderung, die ihr an pytest vornehmen wollt,
bereits geschrieben wurden.

Plugins finden
--------------

Ihr könnt pytest-Plugins von Drittanbietern an verschiedenen Stellen finden,
:abbr:`u.a. (unter anderem)` enthält die :doc:`pytest-Dokumentation
<pytest:reference/plugin_list>` eine alphabetisch geordnete Liste von Plugins,
die von :term:`pypi.org` stammen. Ihr könnt auch pypi.org selbst durchsuchen,
:abbr:`z.B. (zum Beispiel)` nach `pytest <https://pypi.org/search/?q=pytest>`_
oder nach dem `pytest-Framework
<https://pypi.org/search/?q=&c=Framework+%3A%3A+Pytest>`_. Schließlich finden
sich in `pytest-dev <https://github.com/pytest-dev>`_  auf GitHub auch viele
beliebte pytest-Plugins.

Plugins installieren
--------------------

pytest-Plugins lassen sich, wie anderen Python-Pakete einfach mit :term:`pip`
installieren: :samp:`python -m pip install {pytest-cov}`.

Plugins für …
-------------

… veränderte Testabläufe
~~~~~~~~~~~~~~~~~~~~~~~~

pytest führt unsere Tests üblicherweise in einer vorhersehbaren Reihenfolge aus.
Bei einem Verzeichnis mit Testdateien führt pytest jede Datei in alphabetischer
Reihenfolge aus. Innerhalb jeder Datei wird jeder Test in der Reihenfolge
ausgeführt, in der er in der Datei erscheint. Manchmal kann es jedoch sinnvoll
sein, diese Reihenfolge zu ändern. Die folgenden Plugins ändern den üblichen
Ablauf eines Test:

`pytest-xdist <https://pypi.org/project/pytest-xdist/>`_
    führt Tests parallel aus, entweder mit mehreren CPUs auf einer Maschine oder
    mehreren entfernten Maschinen.
`pytest-freethreaded <https://pypi.org/project/pytest-freethreaded/>`_
    für die Überprüfung, ob Tests und Bibliotheken mit dem experimentellen
    Freethreaded-Modus von Python 3.13 thread-sicher sind.
`pytest-rerunfailures <https://pypi.org/project/pytest-rerunfailures/>`_
    führt fehlgeschlagene Tests erneut aus und ist :abbr:`v.a. (vor allem)`
    hilfreich bei fehlerhaften Tests.
`pytest-repeat <https://pypi.org/project/pytest-repeat/>`_
    macht es einfach, einen oder mehrere Tests zu wiederholen.
`pytest-order <https://pypi.org/project/pytest-order/>`_
    ermöglicht die Festlegung der Reihenfolge durch :doc:`markers`.
`pytest-randomly <https://pypi.org/project/pytest-randomly/>`_
    lässt die Tests in zufälliger Reihenfolge ablaufen, zuerst nach Datei, dann
    nach Klasse, dann schließlich nach Testdatei.

… veränderten Output
~~~~~~~~~~~~~~~~~~~~

Die normale pytest-Ausgabe zeigt hauptsächlich Punkte für bestandene Tests und
Zeichen für andere Ausgaben. Wenn ihr ``-v`` übergebt, seht ihr eine Liste von
Testnamen mit dem Ergebnis. Es gibt jedoch Plugins, die die Ausgabe noch weiter
verändern:

`pytest-instafail <https://pypi.org/project/pytest-instafail/>`_
    fügt eine ``--instafail``-Option hinzu, das Tracebacks und Ausgaben von
    fehlgeschlagenen Tests direkt nach dem Fehlschlag meldet. Normalerweise
    meldet pytest Tracebacks und Ausgaben von fehlgeschlagenen Tests erst,
    nachdem alle Tests abgeschlossen wurden.
`pytest-edit <https://pypi.org/project/pytest-edit/>`_
    öffnet einen Editor nach einem fehlgeschlagenen Test.
`pytest-sugar <https://pypi.org/project/pytest-sugar/>`_
    zeigt grüne Häkchen anstelle von Punkten für bestandene Tests und hat einen
    schönen Fortschrittsbalken. Es zeigt, wie pytest-instafail auch, Fehlschläge
    sofort an.
`pytest-html <https://pypi.org/project/pytest-html/>`_
    ermöglicht die Erstellung von HTML-Berichten. Berichte können mit
    zusätzlichen Daten und Bildern, wie :abbr:`z.B. (zum Beispiel)` Screenshots
    von Fehlerfällen, erweitert werden.
`pytest-icdiff <https://pypi.org/project/pytest-icdiff/>`_
    verbessert Diffs in den Fehlermeldungen der Pytest-Assertion mit `ICDiff
    <https://www.jefftk.com/icdiff>`_.

… für die Webentwicklung
~~~~~~~~~~~~~~~~~~~~~~~~

pytest wird ausgiebig für das Testen von Webprojekten verwendet und es gibt eine
lange Liste von Plugins, die das Testen weiter vereinfachen:

`pytest-httpx <https://pypi.org/project/pytest-httpx/>`_
    erleichtert das Testen von `HTTPX <https://www.python-httpx.org>`_ und
    `FastAPI <https://fastapi.tiangolo.com>`_-Anwendungen.
`Playwright for Python <https://pypi.org/project/playwright/>`_
    wurde speziell für End-to-End-Tests entwickelt. Playwright unterstützt alle
    modernen Rendering-Engines wie Chromium, WebKit und Firefox mit einer
    einzigen :abbr:`API (Application Programming Interface)`.
`pyleniumio <https://pypi.org/project/pyleniumio/#test-example>`_
    ist ein dünner Python-Wrapper um Selenium mit einfacher und klarer Syntax.
`pytest-selenium <https://pypi.org/project/pytest-selenium/>`_
    stellt Fixtures zur Verfügung, die eine einfache Konfiguration von
    browserbasierten Tests mit `Selenium <https://www.selenium.dev>`_
    ermöglichen.

.. _fake_plugins:

… für Fake-Daten
~~~~~~~~~~~~~~~~

Wir haben `Faker <https://pypi.org/project/Faker/>`_ schon verwendet in
:ref:`marker_fixtures_combined`, um mehrere Item-Instanzen zu erzeugen. Es gibt
viele Fälle in verschiedenen Bereichen, in denen es hilfreich ist, Fake-Daten zu
erzeugen. Es überrascht daher nicht, dass es mehrere Plugins gibt, die diesen
Bedarf decken:

`Faker <https://pypi.org/project/Faker/>`_
    generiert Fake-Daten für euch und bietet ein Faker Fixture für die
    Verwendung mit pytest.
`pytest-factoryboy <https://pypi.org/project/pytest-factoryboy/>`_
    enthält Fixtures für `factory-boy <https://pypi.org/project/factory-boy/>`_,
    einen Datenbankmodelldatengenerator.

… für Verschiedenes
~~~~~~~~~~~~~~~~~~~

`pytest-cov <https://pypi.org/project/pytest-cov/>`_
    führt die :doc:`../coverage` beim Testen aus.
`pytest-benchmark <https://pypi.org/project/pytest-benchmark/>`_
    führt Benchmark-Timing für Code innerhalb von Tests durch.
`pytest-timeout <https://pypi.org/project/pytest-timeout/>`_
    lässt Tests nicht zu lange laufen.
`pytest-asyncio <https://pypi.org/project/pytest-asyncio/>`_
    testet asynchrone Funktionen.
`pytest-mock <https://pypi.org/project/pytest-mock/>`_
    ist ein dünner Wrapper um die :doc:`unittest.mock <../mock>`-Patching-API.
`pytest-patterns <https://pypi.org/project/pytest-patterns/>`_
    stellt eine für Tests optimierte Pattern-Matching-Engine bereit.
:doc:`pytest-grpc <Python4DataScience:data-processing/apis/grpc/test>`
    ist ein Pytest-Plugin für
    :doc:`Python4DataScience:data-processing/apis/grpc/index`.
`pytest-bdd <https://pypi.org/project/pytest-bdd/>`_
    schreibt :abbr:`BDD (Behavior Driven Development, deutsch:
    verhaltensgetriebene Softwareentwicklung)`-Tests mit pytest.

Eigene Plugins
--------------

.. seealso::
   * `Writing plugins
     <https://docs.pytest.org/en/latest/how-to/writing_plugins.html>`_
