Testen
======

.. _build-errors:

Build-Fehler
------------

Ihr habt die Möglichkeit, vor der Veröffentlichung eurer Änderungen zu
überprüfen, ob eure Inhalte ordnungsgemäß erstellt werden. Hierfür hat
`Sphinx <https://www.sphinx-doc.org/>`_ einen pingelig (:abbr:`engl.
(englisch)` nitpicky)-Modus, der mit der Option ``-n`` aufgerufen werden kann,
also :abbr:`z.B. (zum Beispiel)` mit:

.. code-block:: console

    $ python -m sphinx -nb html docs/ docs/_build/

.. _link-checks:

Links überprüfen
----------------

Ihr könnt auch automatisiert sicherstellen, dass die von euch angegebenen
Linkziele erreichbar sind. Unser Dokumentationswerkzeug Sphinx verwendet hierfür
einen ``linkcheck``-Builder, den ihr :abbr:`ggf. (gegebenenfalls)` aufrufen
könnt mit:

.. code-block:: console

    $ python -m sphinx -b linkcheck docs/ docs/_build/

Die Ausgabe kann dann :abbr:`z.B. (zum Beispiel)` so aussehen:

.. code-block:: console

    $ python -m sphinx -b linkcheck docs/ docs/_build/
    Running Sphinx v3.5.2
    loading translations [de]... done
    …
    building [mo]: targets for 0 po files that are out of date
    building [linkcheck]: targets for 27 source files that are out of date
    …
    (content/accessibility: line   89) ok        https://bbc.github.io/subtitle-guidelines/
    (content/writing-style: line  164) ok        http://disabilityinkidlit.com/2016/07/08/introduction-to-disability-terminology/

    …
    (   index: line    5) redirect  https://cusy-design-system.readthedocs.io/ - with Found to https://cusy-design-system.readthedocs.io/de/latest/
    …
    (accessibility/color: line  114) broken    https://chrome.google.com/webstore/detail/nocoffee/jjeeggmbnhckmgdhmgdckeigabjfbddl - 404 Client Error: Not Found for url: https://chrome.google.com/webstore/detail/nocoffee/jjeeggmbnhckmgdhmgdckeigabjfbddl
