Erweiterungen
=============

Eingebaute Erweiterungen
------------------------

`sphinx.ext.autodoc <https://www.sphinx-doc.org/en/master/usage/extensions/autodoc.html>`_
    bindet Dokumentationen aus Docstrings ein.
`sphinx.ext.autosummary <https://www.sphinx-doc.org/en/master/usage/extensions/autosummary.html>`_
    erzeugt Zusammenfassungen von Funktionen, Methoden und Attributen aus
    Docstrings.
`sphinx.ext.autosectionlabel <https://www.sphinx-doc.org/en/master/usage/extensions/autosectionlabel.html>`_
    referenziert Abschnitte mit Hilfe des Titels.
`sphinx.ext.graphviz <https://www.sphinx-doc.org/en/master/usage/extensions/graphviz.html>`_
    rendert `Graphviz <https://www.graphviz.org/>`_ Graphen.
`sphinx.ext.ifconfig <https://www.sphinx-doc.org/en/master/usage/extensions/ifconfig.html>`_
    schließt Inhalte nur unter bestimmten Bedingungen ein.
`sphinx.ext.intersphinx <https://www.sphinx-doc.org/en/master/usage/extensions/intersphinx.html>`_
    ermöglicht das Einbinden anderer Projektdokumentationen.
`sphinx.ext.mathjax <https://www.sphinx-doc.org/en/master/usage/extensions/math.html#module-sphinx.ext.mathjax>`_
    Rendertmathematische Formeln über JavaScript.
`sphinx.ext.napoleon <https://www.sphinx-doc.org/en/master/usage/extensions/napoleon.html>`_
    unterstützt NumPy und Google Style Docstrings.
`sphinx.ext.todo <https://www.sphinx-doc.org/en/master/usage/extensions/todo.html>`_
    unterstützt ToDo-Elemente.
`sphinx.ext.viewcode <https://www.sphinx-doc.org/en/master/usage/extensions/viewcode.html>`_
    fügt Links auf den Quellcode der Sphinx-Dokumentation hinzu.

.. seealso::
   Einen vollständigen Überblick erhaltet ihr unter `Sphinx Extensions
   <https://www.sphinx-doc.org/en/master/usage/extensions/index.html>`_.

Erweiterungen von Drittanbietern
--------------------------------

`nbsphinx <https://nbsphinx.readthedocs.io/>`_
    Jupyter Notebooks in Sphinx
`jupyter-sphinx <https://github.com/jupyter-widgets/jupyter-sphinx>`_
    ermöglicht das Rendern von interaktiven Jupyter-Widgets in Sphinx, siehe
    auch `Embedding Widgets in the Sphinx HTML Documentation
    <https://ipywidgets.readthedocs.io/en/latest/embedding.html#embedding-widgets-in-the-sphinx-html-documentation>`_.
`numpydoc <https://github.com/numpy/numpydoc>`_
    `NumPy <NumPy>`_ Sphinx-Erweiterung.
`Releases <https://github.com/bitprophet/releases>`_
    schreibt eine Changelog-Datei.
`sphinxcontrib-napoleon <https://sphinxcontrib-napoleon.readthedocs.io/en/latest/>`_
    Präprozessor zum Parsen von NumPy- und Google-Style Docstrings.
`Sphinx-autodoc-typehints <https://github.com/agronholm/sphinx-autodoc-typehints>`_
    Unterstützung von Typ-Hints für die Sphinx-Autodoc-Erweiterung.
`sphinx-git <sphinx-git>`_
    `git <https://git-scm.com/>`_-Changelog für Sphinx.
`sphinx-intl <https://pypi.python.org/pypi/sphinx-intl>`_
    Sphinx-Erweiterung für Übersetzungen.
`sphinx-autobuild <https://github.com/GaretJax/sphinx-autobuild>`_
    überwacht ein Sphinx-Repository und erstellt neue Dokumentation, sobald
    Änderungen vorgenommen werden.
`Sphinxcontrib-mermaid <https://github.com/mgaitan/sphinxcontrib-mermaid>`_
    ermöglicht euch, Mermaid-Grafiken in Ihre Dokumente einzubetten.

Eigene Erweiterungen
--------------------

Lokale Erweiterungen in einem Projekt sollten relativ zur Dokumentation
angegeben werden. Der entsprechende Pfad wird in der Sphinx-Konfigurationsdatei
``docs/conf.py`` angegeben. Wenn sich eure Erweiterung im Verzeichnis ``exts``
in der Datei ``foo.py`` befindet, dann sollte die ``conf.py``-Datei wie folgt
aussehen:

.. code-block:: python

    import sys
    import os
    sys.path.insert(0, os.path.abspath('exts'))

    extensions = [
        'foo',
        ...
        ]
