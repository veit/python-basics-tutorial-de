Erweiterungen
=============

.. seealso::
    `Sphinx Extensions
    <https://www.sphinx-doc.org/en/master/usage/extensions/index.html>`_

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
    rendert mathematische Formeln über JavaScript.
`sphinx.ext.napoleon <https://www.sphinx-doc.org/en/master/usage/extensions/napoleon.html>`_
    unterstützt NumPy und Google Style Docstrings.
`sphinx.ext.todo <https://www.sphinx-doc.org/en/master/usage/extensions/todo.html>`_
    unterstützt ToDo-Elemente.
`sphinx.ext.viewcode <https://www.sphinx-doc.org/en/master/usage/extensions/viewcode.html>`_
    fügt Links auf den Quellcode der Sphinx-Dokumentation hinzu.

.. seealso::
    `sphinx/sphinx/ext/
    <https://github.com/sphinx-doc/sphinx/tree/master/sphinx/ext>`_

Erweiterungen von Drittanbietern
--------------------------------

`nbsphinx <https://nbsphinx.readthedocs.io/>`_
    Jupyter Notebooks in Sphinx
`jupyter-sphinx <https://github.com/jupyter/jupyter-sphinx>`_
    ermöglicht das Rendern von interaktiven Jupyter-Widgets in Sphinx.

    .. seealso::
        `Embedding Widgets in the Sphinx HTML Documentation
        <https://ipywidgets.readthedocs.io/en/latest/embedding.html#embedding-widgets-in-the-sphinx-html-documentation>`_.

`Breathe <https://github.com/breathe-doc/breathe>`_
    ReStructuredText and Sphinx bridge to `Doxygen <https://www.doxygen.nl>`_
`numpydoc <https://github.com/numpy/numpydoc>`_
    `NumPy <https://numpy.org/>`_ Sphinx-Erweiterung.
`Releases <https://github.com/bitprophet/releases>`_
    schreibt eine :file:`CHANGELOG`-Datei.
`sphinxcontrib-napoleon <https://sphinxcontrib-napoleon.readthedocs.io/en/latest/>`_
    Präprozessor zum Parsen von NumPy- und Google-Style Docstrings.
`sphinx-autodoc-annotation <https://github.com/nicolashainaux/sphinx-autodoc-annotation>`_
    verwendet Python3-Annotations in Sphinx docstrings
`Sphinx-autodoc-typehints <https://github.com/agronholm/sphinx-autodoc-typehints>`_
    Unterstützung von Typ-Hints für die Sphinx-Autodoc-Erweiterung.
`sphinx-git <https://sphinx-git.readthedocs.io/>`_
    `git <https://git-scm.com/>`_-Changelog für Sphinx.
`Sphinx Gitstamp Generator Extension <https://github.com/jdillard/sphinx-gitstamp>`_
    fügt git Zeitstempel im Kontext ein
`sphinx-issues <https://pypi.org/project/sphinx-issues/>`_
    erstellt Links zu GitHub or GitLab Issues, Pull-Requests und
    Benutzerprofilen.
`sphinx-intl <https://pypi.org/project/sphinx-intl/>`_
    Sphinx-Erweiterung für Übersetzungen.
`sphinx-autobuild <https://github.com/sphinx-doc/sphinx-autobuild>`_
    überwacht ein Sphinx-Repository und erstellt neue Dokumentation, sobald
    Änderungen vorgenommen werden.
`Sphinx-Needs <https://sphinx-needs.readthedocs.io/>`_
    erlaubt die Definition, Verlinkung und Filterung von need-Objekten, also
    :abbr:`z.B. (zum Beispiel)` Anforderungen und Testfälle
`Sphinx-pyreverse <https://github.com/alendit/sphinx-pyreverse>`_
    erstellt ein UML-Diagramm von Python-Modulen
`sphinx-jsonschema <https://github.com/lnoor/sphinx-jsonschema>`_
    zeigt ein `JSON Schema <https://json-schema.org>`_ in der
    Sphinx-Dokumentation
`Sphinxcontrib-mermaid <https://github.com/mgaitan/sphinxcontrib-mermaid>`_
    ermöglicht euch, `Mermaid <http://mermaid.js.org/>`_-Grafiken in eure
    Dokumente einzubetten.
`Sphinx Sitemap Generator Extension <https://github.com/jdillard/sphinx-sitemap>`_
    generiert multiversion- und multilanguage-`sitemaps
    <https://www.sitemaps.org/protocol.html>`_ für die HTML-Version
`Sphinx Lint <https://github.com/sphinx-contrib/sphinx-lint>`_
    basiert auf `rstlint.py
    <https://github.com/python/cpython/blob/e0433c1e7/Doc/tools/rstlint.py>`_
    aus CPython.
`sphinx-toolbox <https://sphinx-toolbox.readthedocs.io/en/stable/index.html>`_
    Werkzeugkasten für Sphinx mit vielen nützlichen Werkzeugen.

.. seealso::
    `sphinx-contrib <https://github.com/sphinx-contrib/>`_
        A repository of Sphinx extensions maintained by their respective authors.
    `sphinx-extensions <https://sphinx-extensions.readthedocs.io/en/latest/>`_
        Curated site with Sphinx extensions with live examples and their
        configuration.

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

    sys.path.insert(0, os.path.abspath("exts"))

    extensions = ["foo", ...]

.. seealso::
    * `Developing extensions for Sphinx
      <https://www.sphinx-doc.org/en/master/extdev/>`_
    * `Application API
      <https://www.sphinx-doc.org/en/master/extdev/appapi.html>`_
