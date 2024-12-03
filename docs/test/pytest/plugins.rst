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

    .. image:: https://raster.shields.io/github/stars/pytest-dev/pytest-xdist
       :alt: Stars
       :target: https://github.com/pytest-dev/pytest-xdist/stargazers

    .. image:: https://raster.shields.io/github/contributors/pytest-dev/pytest-xdist
       :alt: Contributors
       :target: https://github.com/pytest-dev/pytest-xdist/graphs/contributors

    .. image:: https://raster.shields.io/github/commit-activity/y/pytest-dev/pytest-xdist
       :alt: Commit activity
       :target: https://github.com/pytest-dev/pytest-xdist/graphs/commit-activity

    .. image:: https://raster.shields.io/github/license/pytest-dev/pytest-xdist
       :alt: Lizenz
       :target: https://github.com/pytest-dev/pytest-xdist?tab=MIT-1-ov-file#readme

`pytest-freethreaded <https://pypi.org/project/pytest-freethreaded/>`_
    für die Überprüfung, ob Tests und Bibliotheken mit dem experimentellen
    Freethreaded-Modus von Python 3.13 thread-sicher sind.

    .. image:: https://raster.shields.io/github/stars/tonybaloney/pytest-freethreaded
       :alt: Stars
       :target: https://github.com/tonybaloney/pytest-freethreaded/stargazers

    .. image:: https://raster.shields.io/github/contributors/tonybaloney/pytest-freethreaded
       :alt: Contributors
       :target: https://github.com/tonybaloney/pytest-freethreaded/contributors

    .. image:: https://raster.shields.io/github/commit-activity/y/tonybaloney/pytest-freethreaded
       :alt: Commit activity
       :target: https://github.com/tonybaloney/pytest-freethreaded/graphs/commit-activity

    .. image:: https://raster.shields.io/github/license/tonybaloney/pytest-freethreaded
       :alt: Lizenz
       :target: https://github.com/tonybaloney/pytest-freethreaded?tab=MIT-1-ov-file#readme

`pytest-rerunfailures <https://pypi.org/project/pytest-rerunfailures/>`_
    führt fehlgeschlagene Tests erneut aus und ist :abbr:`v.a. (vor allem)`
    hilfreich bei fehlerhaften Tests.

    .. image:: https://raster.shields.io/github/stars/pytest-dev/pytest-rerunfailures
       :alt: Stars
       :target: https://github.com/pytest-dev/pytest-dev/pytest-rerunfailures

    .. image:: https://raster.shields.io/github/contributors/pytest-dev/pytest-rerunfailures
       :alt: Contributors
       :target: https://github.com/pytest-dev/pytest-rerunfailures/graphs/contributors

    .. image:: https://raster.shields.io/github/commit-activity/y/pytest-dev/pytest-rerunfailures
       :alt: Commit activity
       :target: https://github.com/pytest-dev/pytest-rerunfailures/graphs/commit-activity

    .. image:: https://raster.shields.io/github/license/pytest-dev/pytest-rerunfailures
       :alt: Lizenz
       :target: https://github.com/pytest-dev/pytest-rerunfailures?tab=License-1-ov-file#readme

`pytest-repeat <https://pypi.org/project/pytest-repeat/>`_
    macht es einfach, einen oder mehrere Tests zu wiederholen.

    .. image:: https://raster.shields.io/github/stars/pytest-dev/pytest-repeat
       :alt: Stars
       :target: https://github.com/pytest-dev/pytest-repeat/stargazers

    .. image:: https://raster.shields.io/github/contributors/pytest-dev/pytest-repeat
       :alt: Contributors
       :target: https://github.com/pytest-dev/pytest-repeat/graphs/contributors

    .. image:: https://raster.shields.io/github/commit-activity/y/pytest-dev/pytest-repeat
       :alt: Commit activity
       :target: https://github.com/pytest-dev/pytest-repeat/graphs/commit-activity

    .. image:: https://raster.shields.io/github/license/pytest-dev/pytest-repeat
       :alt: Lizenz
       :target: https://github.com/pytest-dev/pytest-repeat?tab=License-1-ov-file#readme

`pytest-order <https://pypi.org/project/pytest-order/>`_
    ermöglicht die Festlegung der Reihenfolge durch :doc:`markers`.

    .. image:: https://raster.shields.io/github/stars/pytest-dev/pytest-order
       :alt: Stars
       :target: https://github.com/pytest-dev/pytest-order/stargazers

    .. image:: https://raster.shields.io/github/contributors/pytest-dev/pytest-order
       :alt: Contributors
       :target: https://github.com/pytest-dev/pytest-order/graphs/contributors

    .. image:: https://raster.shields.io/github/commit-activity/y/pytest-dev/pytest-order
       :alt: Commit activity
       :target: https://github.com/pytest-dev/pytest-order/graphs/commit-activity

    .. image:: https://raster.shields.io/github/license/pytest-dev/pytest-xdist
       :alt: Lizenz
       :target: https://github.com/pytest-dev/pytest-xdist?tab=MIT-1-ov-file#readme

`pytest-randomly <https://pypi.org/project/pytest-randomly/>`_
    lässt die Tests in zufälliger Reihenfolge ablaufen, zuerst nach Datei, dann
    nach Klasse, dann schließlich nach Testdatei.

    .. image:: https://raster.shields.io/github/stars/pytest-dev/pytest-randomly
       :alt: Stars
       :target: https://github.com/pytest-dev/pytest-randomly/stargazers

    .. image:: https://raster.shields.io/github/contributors/pytest-dev/pytest-randomly
       :alt: Contributors
       :target: https://github.com/pytest-dev/pytest-randomly/graphs/contributors

    .. image:: https://raster.shields.io/github/commit-activity/y/pytest-dev/pytest-randomly
       :alt: Commit activity
       :target: https://github.com/pytest-dev/pytest-randomly/graphs/commit-activity

    .. image:: https://raster.shields.io/github/license/pytest-dev/pytest-randomly
       :alt: Lizenz
       :target: https://github.com/pytest-dev/pytest-randomly?tab=MIT-1-ov-file#readme


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

    .. image:: https://raster.shields.io/github/stars/pytest-dev/pytest-instafail
       :alt: Stars
       :target: https://github.com/pytest-dev/pytest-instafail/stargazers

    .. image:: https://raster.shields.io/github/contributors/pytest-dev/pytest-instafail
       :alt: Contributors
       :target: https://github.com/pytest-dev/pytest-instafail/graphs/contributors

    .. image:: https://raster.shields.io/github/commit-activity/y/pytest-dev/pytest-instafail
       :alt: Commit activity
       :target: https://github.com/pytest-dev/pytest-instafail/graphs/commit-activity

    .. image:: https://raster.shields.io/github/license/pytest-dev/pytest-instafail
       :alt: Lizenz
       :target: https://github.com/pytest-dev/pytest-rerunfailures?tab=License-1-ov-file#readme

`pytest-edit <https://pypi.org/project/pytest-edit/>`_
    öffnet einen Editor nach einem fehlgeschlagenen Test.

    .. image:: https://raster.shields.io/github/stars/mrmino/pytest-edit
       :alt: Stars
       :target: https://github.com/mrmino/pytest-edit/stargazers

    .. image:: https://raster.shields.io/github/contributors/mrmino/pytest-edit
       :alt: Contributors
       :target: https://github.com/pytest-dev/mrmino/pytest-edit

    .. image:: https://raster.shields.io/github/commit-activity/y/mrmino/pytest-edit
       :alt: Commit activity
       :target: https://github.com/mrmino/pytest-edit/graphs/commit-activity

    .. image:: https://raster.shields.io/github/license/mrmino/pytest-edit
       :alt: Lizenz
       :target: https://github.com/mrmino/pytest-edit?tab=MIT-1-ov-file#readme

`pytest-sugar <https://pypi.org/project/pytest-sugar/>`_
    zeigt grüne Häkchen anstelle von Punkten für bestandene Tests und hat einen
    schönen Fortschrittsbalken. Es zeigt, wie pytest-instafail auch, Fehlschläge
    sofort an.

    .. image:: https://raster.shields.io/github/stars/Teemu/pytest-sugar
       :alt: Stars
       :target: https://github.com/Teemu/pytest-sugar/stargazers

    .. image:: https://raster.shields.io/github/contributors/Teemu/pytest-sugar
       :alt: Contributors
       :target: https://github.com/Teemu/pytest-sugar/graphs/contributors

    .. image:: https://raster.shields.io/github/commit-activity/y/Teemu/pytest-sugar
       :alt: Commit activity
       :target: https://github.com/Teemu/pytest-sugar/graphs/commit-activity

    .. image:: https://raster.shields.io/github/license/Teemu/pytest-sugar
       :alt: Lizenz
       :target: https://github.com/Teemu/pytest-sugar?tab=License-1-ov-file#readme

`pytest-html <https://pypi.org/project/pytest-html/>`_
    ermöglicht die Erstellung von HTML-Berichten. Berichte können mit
    zusätzlichen Daten und Bildern, wie :abbr:`z.B. (zum Beispiel)` Screenshots
    von Fehlerfällen, erweitert werden.

    .. image:: https://raster.shields.io/github/stars/pytest-dev/pytest-html
       :alt: Stars
       :target: https://github.com/pytest-dev/pytest-html/stargazers

    .. image:: https://raster.shields.io/github/contributors/pytest-dev/pytest-html
       :alt: Contributors
       :target: https://github.com/pytest-dev/pytest-html/graphs/contributors

    .. image:: https://raster.shields.io/github/commit-activity/y/pytest-dev/pytest-html
       :alt: Commit activity
       :target: https://github.com/pytest-dev/pytest-html/graphs/commit-activity

    .. image:: https://raster.shields.io/github/license/pytest-dev/pytest-html
       :alt: Lizenz
       :target: https://github.com/pytest-dev/pytest-html?tab=License-1-ov-file#readme

`pytest-icdiff <https://pypi.org/project/pytest-icdiff/>`_
    verbessert Diffs in den Fehlermeldungen der Pytest-Assertion mit `ICDiff
    <https://www.jefftk.com/icdiff>`_.

    .. image:: https://raster.shields.io/github/stars/hjwp/pytest-icdiff
       :alt: Stars
       :target: https://github.com/hjwp/pytest-icdiff/stargazers

    .. image:: https://raster.shields.io/github/contributors/hjwp/pytest-icdiff
       :alt: Contributors
       :target: https://github.com/hjwp/pytest-icdiff/graphs/contributors

    .. image:: https://raster.shields.io/github/commit-activity/y/hjwp/pytest-icdiff
       :alt: Commit activity
       :target: https://github.com/hjwp/pytest-icdiff/graphs/commit-activity

    .. image:: https://raster.shields.io/github/license/hjwp/pytest-icdiff
       :alt: Lizenz
       :target: https://github.com/hjwp/pytest-icdiff?tab=MIT-1-ov-file#readme


… für die Webentwicklung
~~~~~~~~~~~~~~~~~~~~~~~~

pytest wird ausgiebig für das Testen von Webprojekten verwendet und es gibt eine
lange Liste von Plugins, die das Testen weiter vereinfachen:

`pytest-httpx <https://pypi.org/project/pytest-httpx/>`_
    erleichtert das Testen von `HTTPX <https://www.python-httpx.org>`_ und
    `FastAPI <https://fastapi.tiangolo.com>`_-Anwendungen.

    .. image:: https://raster.shields.io/github/stars/Colin-b/pytest_httpx
       :alt: Stars
       :target: https://github.com/Colin-b/pytest_httpx/stargazers

    .. image:: https://raster.shields.io/github/contributors/Colin-b/pytest_httpx
       :alt: Contributors
       :target: https://github.com/Colin-b/pytest_httpx/graphs/contributors

    .. image:: https://raster.shields.io/github/commit-activity/y/Colin-b/pytest_httpx
       :alt: Commit activity
       :target: https://github.com/Colin-b/pytest_httpx/graphs/commit-activity

    .. image:: https://raster.shields.io/github/license/Colin-b/pytest_httpx
       :alt: Lizenz
       :target: https://github.com/Colin-b/pytest_httpx?tab=MIT-1-ov-file#readme

`Playwright for Python <https://pypi.org/project/playwright/>`_
    wurde speziell für End-to-End-Tests entwickelt. Playwright unterstützt alle
    modernen Rendering-Engines wie Chromium, WebKit und Firefox mit einer
    einzigen :abbr:`API (Application Programming Interface)`.

    .. image:: https://raster.shields.io/github/stars/Microsoft/playwright-python
       :alt: Stars
       :target: https://github.com/Microsoft/playwright-python/stargazers

    .. image:: https://raster.shields.io/github/contributors/Microsoft/playwright-python
       :alt: Contributors
       :target: https://github.com/Microsoft/playwright-python/graphs/contributors

    .. image:: https://raster.shields.io/github/commit-activity/y/Microsoft/playwright-python
       :alt: Commit activity
       :target: https://github.com/Microsoft/playwright-python/graphs/commit-activity

    .. image:: https://raster.shields.io/github/license/Microsoft/playwright-python
       :alt: Lizenz
       :target: https://github.com/Microsoft/playwright-python?tab=MIT-1-ov-file#readme

`pyleniumio <https://pypi.org/project/pyleniumio/>`_
    ist ein dünner Python-Wrapper um Selenium mit einfacher und klarer Syntax.

    .. image:: https://raster.shields.io/github/stars/ElSnoMan/pyleniumio
       :alt: Stars
       :target: https://github.com/ElSnoMan/pyleniumio/stargazers

    .. image:: https://raster.shields.io/github/contributors/ElSnoMan/pyleniumio
       :alt: Contributors
       :target: https://github.com/pytest-dev/ElSnoMan/pyleniumio

    .. image:: https://raster.shields.io/github/commit-activity/y/ElSnoMan/pyleniumio
       :alt: Commit activity
       :target: https://github.com/ElSnoMan/pyleniumio/graphs/commit-activity

    .. image:: https://raster.shields.io/github/license/ElSnoMan/pyleniumio
       :alt: Lizenz
       :target: https://github.com/ElSnoMan/pyleniumio?tab=MIT-1-ov-file#readme

`pytest-selenium <https://pypi.org/project/pytest-selenium/>`_
    stellt Fixtures zur Verfügung, die eine einfache Konfiguration von
    browserbasierten Tests mit `Selenium <https://www.selenium.dev>`_
    ermöglichen.

    .. image:: https://raster.shields.io/github/stars/pytest-dev/pytest-selenium
       :alt: Stars
       :target: https://github.com/pytest-dev/pytest-selenium/stargazers

    .. image:: https://raster.shields.io/github/contributors/pytest-dev/pytest-selenium
       :alt: Contributors
       :target: https://github.com/pytest-dev/pytest-selenium

    .. image:: https://raster.shields.io/github/commit-activity/y/pytest-dev/pytest-selenium
       :alt: Commit activity
       :target: https://github.com/pytest-dev/pytest-selenium/graphs/commit-activity

    .. image:: https://raster.shields.io/github/license/pytest-dev/pytest-selenium
       :alt: Lizenz
       :target: https://github.com/pytest-dev/pytest-selenium?tab=License-1-ov-file#readme


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

    .. image:: https://raster.shields.io/github/stars/joke2k/faker
       :alt: Stars
       :target: https://github.com/joke2k/faker/stargazers

    .. image:: https://raster.shields.io/github/contributors/joke2k/faker
       :alt: Contributors
       :target: https://github.com/joke2k/faker/graphs/contributors

    .. image:: https://raster.shields.io/github/commit-activity/y/joke2k/faker
       :alt: Commit activity
       :target: https://github.com/joke2k/faker/graphs/commit-activity

    .. image:: https://raster.shields.io/github/license/joke2k/faker
       :alt: Lizenz
       :target: https://github.com/joke2k/faker?tab=MIT-1-ov-file#readme

`pytest-factoryboy <https://pypi.org/project/pytest-factoryboy/>`_
    enthält Fixtures für `factory-boy <https://pypi.org/project/factory-boy/>`_,
    einen Datenbankmodell-Datengenerator.

    .. image:: https://raster.shields.io/github/stars/pytest-dev/pytest-factoryboy
       :alt: Stars
       :target: https://github.com/pytest-dev/pytest-factoryboy/stargazers

    .. image:: https://raster.shields.io/github/contributors/pytest-dev/pytest-factoryboy
       :alt: Contributors
       :target: https://github.com/pytest-dev/pytest-factoryboy/graphs/contributors

    .. image:: https://raster.shields.io/github/commit-activity/y/pytest-dev/pytest-factoryboy
       :alt: Commit activity
       :target: https://github.com/pytest-dev/pytest-factoryboy/graphs/commit-activity

    .. image:: https://raster.shields.io/github/license/pytest-dev/pytest-factoryboy
       :alt: Lizenz
       :target: https://github.com/pytest-dev/pytest-factoryboy?tab=MIT-1-ov-file#readme


… für Verschiedenes
~~~~~~~~~~~~~~~~~~~

`pytest-cov <https://pypi.org/project/pytest-cov/>`_
    führt die :doc:`../pytest/coverage` beim Testen aus.

    .. image:: https://raster.shields.io/github/stars/pytest-dev/pytest-cov
       :alt: Stars
       :target: https://github.com/pytest-dev/pytest-cov/stargazers

    .. image:: https://raster.shields.io/github/contributors/pytest-dev/pytest-cov
       :alt: Contributors
       :target: https://github.com/pytest-dev/pytest-cov/graphs/contributors

    .. image:: https://raster.shields.io/github/commit-activity/y/pytest-dev/pytest-cov
       :alt: Commit activity
       :target: https://github.com/pytest-dev/pytest-cov/graphs/commit-activity

    .. image:: https://raster.shields.io/github/license/pytest-dev/pytest-cov
       :alt: Lizenz
       :target: https://github.com/pytest-dev/pytest-cov?tab=MIT-1-ov-file#readme

`pytest-benchmark <https://pypi.org/project/pytest-benchmark/>`_
    führt Benchmark-Timing für Code innerhalb von Tests durch.

    .. image:: https://raster.shields.io/github/stars/ionelmc/pytest-benchmark
       :alt: Stars
       :target: https://github.com/ionelmc/pytest-benchmark/stargazers

    .. image:: https://raster.shields.io/github/contributors/ionelmc/pytest-benchmark
       :alt: Contributors
       :target: https://github.com/ionelmc/pytest-benchmark/graphs/contributors

    .. image:: https://raster.shields.io/github/commit-activity/y/ionelmc/pytest-benchmark
       :alt: Commit activity
       :target: https://github.com/ionelmc/pytest-benchmark/graphs/commit-activity

    .. image:: https://raster.shields.io/github/license/ionelmc/pytest-benchmark
       :alt: Lizenz
       :target: https://github.com/pytest-dev/ionelmc/pytest-benchmark-1-ov-file#readme

`pytest-timeout <https://pypi.org/project/pytest-timeout/>`_
    lässt Tests nicht zu lange laufen.

    .. image:: https://raster.shields.io/github/stars/pytest-dev/pytest-timeout
       :alt: Stars
       :target: https://github.com/pytest-dev/pytest-timeout/stargazers

    .. image:: https://raster.shields.io/github/contributors/pytest-dev/pytest-timeout
       :alt: Contributors
       :target: https://github.com/pytest-dev/pytest-timeout/graphs/contributors

    .. image:: https://raster.shields.io/github/commit-activity/y/pytest-dev/pytest-timeout
       :alt: Commit activity
       :target: https://github.com/pytest-dev/pytest-timeout/graphs/commit-activity

    .. image:: https://raster.shields.io/github/license/pytest-dev/pytest-timeout
       :alt: Lizenz
       :target: https://github.com/pytest-dev/pytest-timeout?tab=MIT-1-ov-file#readme

`pytest-asyncio <https://pypi.org/project/pytest-asyncio/>`_
    testet asynchrone Funktionen.

    .. image:: https://raster.shields.io/github/stars/pytest-dev/pytest-asyncio
       :alt: Stars
       :target: https://github.com/pytest-dev/pytest-asyncio/stargazers

    .. image:: https://raster.shields.io/github/contributors/pytest-dev/pytest-asyncio
       :alt: Contributors
       :target: https://github.com/pytest-dev/pytest-asyncio/graphs/contributors

    .. image:: https://raster.shields.io/github/commit-activity/y/pytest-dev/pytest-asyncio
       :alt: Commit activity
       :target: https://github.com/pytest-dev/pytest-asyncio/graphs/commit-activity

    .. image:: https://raster.shields.io/github/license/pytest-dev/pytest-asyncio
       :alt: Lizenz
       :target: https://github.com/pytest-dev/pytest-asyncio?tab=MIT-1-ov-file#readme

`pytest-mock <https://pypi.org/project/pytest-mock/>`_
    ist ein dünner Wrapper um die :doc:`unittest.mock <../mock>`-Patching-API.

    .. image:: https://raster.shields.io/github/stars/pytest-dev/pytest-mock
       :alt: Stars
       :target: https://github.com/pytest-dev/pytest-mock/stargazers

    .. image:: https://raster.shields.io/github/contributors/pytest-dev/pytest-mock
       :alt: Contributors
       :target: https://github.com/pytest-dev/pytest-mock/graphs/contributors

    .. image:: https://raster.shields.io/github/commit-activity/y/pytest-dev/pytest-mock
       :alt: Commit activity
       :target: https://github.com/pytest-dev/pytest-mock/graphs/commit-activity

    .. image:: https://raster.shields.io/github/license/pytest-dev/pytest-mock
       :alt: Lizenz
       :target: https://github.com/pytest-dev/pytest-mock?tab=MIT-1-ov-file#readme

`pytest-patterns <https://pypi.org/project/pytest-patterns/>`_
    stellt eine für Tests optimierte Pattern-Matching-Engine bereit.

    .. image:: https://raster.shields.io/github/stars/flyingcircusio/pytest-patterns
       :alt: Stars
       :target: https://github.com/flyingcircusio/pytest-patterns/stargazers

    .. image:: https://raster.shields.io/github/contributors/flyingcircusio/pytest-patterns
       :alt: Contributors
       :target: https://github.com/flyingcircusio/pytest-patterns/graphs/contributors

    .. image:: https://raster.shields.io/github/commit-activity/y/flyingcircusio/pytest-patterns
       :alt: Commit activity
       :target: https://github.com/flyingcircusio/pytest-patterns/graphs/commit-activity

    .. image:: https://raster.shields.io/github/license/flyingcircusio/pytest-patterns
       :alt: Lizenz
       :target: https://github.com/flyingcircusio/pytest-patterns?tab=MIT-1-ov-file#readme

:doc:`pytest-grpc <Python4DataScience:data-processing/apis/grpc/test>`
    ist ein Pytest-Plugin für
    :doc:`Python4DataScience:data-processing/apis/grpc/index`.

    .. image:: https://raster.shields.io/github/stars/kataev/pytest-grpc
       :alt: Stars
       :target: https://github.com/kataev/pytest-grpc/stargazers

    .. image:: https://raster.shields.io/github/contributors/kataev/pytest-grpc
       :alt: Contributors
       :target: https://github.com/kataev/pytest-grpc/graphs/contributors

    .. image:: https://raster.shields.io/github/commit-activity/y/kataev/pytest-grpc
       :alt: Commit activity
       :target: https://github.com/kataev/pytest-grpc/graphs/commit-activity

    .. image:: https://raster.shields.io/github/license/kataev/pytest-grpc
       :alt: Lizenz
       :target: https://github.com/kataev/pytest-grpc?tab=MIT-1-ov-file#readme

`pytest-bdd <https://pypi.org/project/pytest-bdd/>`_
    schreibt :abbr:`BDD (Behavior Driven Development, deutsch:
    verhaltensgetriebene Softwareentwicklung)`-Tests mit pytest.

    .. image:: https://raster.shields.io/github/stars/pytest-dev/pytest-bdd
       :alt: Stars
       :target: https://github.com/pytest-dev/pytest-bdd/stargazers

    .. image:: https://raster.shields.io/github/contributors/pytest-dev/pytest-bdd
       :alt: Contributors
       :target: https://github.com/pytest-dev/pytest-bdd/graphs/contributors

    .. image:: https://raster.shields.io/github/commit-activity/y/pytest-dev/pytest-bdd
       :alt: Commit activity
       :target: https://github.com/pytest-dev/pytest-bdd/graphs/commit-activity

    .. image:: https://raster.shields.io/github/license/pytest-dev/pytest-bdd
       :alt: Lizenz
       :target: https://github.com/pytest-dev/pytest-bdd?tab=MIT-1-ov-file#readme

Eigene Plugins
--------------

.. seealso::
   * `Writing plugins
     <https://docs.pytest.org/en/latest/how-to/writing_plugins.html>`_
