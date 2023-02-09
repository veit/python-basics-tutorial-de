Intersphinx
===========

`sphinx.ext.intersphinx
<http://www.sphinx-doc.org/en/master/usage/extensions/intersphinx.html>`_
ermöglicht die Verknüpfung mit anderen Projektdokumentationen.

Konfiguration
-------------

In ``docs/conf.py`` muss Intersphinx als Erweiterung angegeben werden:

.. code-block:: python

    extensions = [
        ...
        'sphinx.ext.intersphinx',
        ]

Externe Sphinx-Dokumentation kann dann angegeben werden, :abbr:`z.B. (zum
Beispiel)` mit:

.. code-block:: python

    intersphinx_mapping = {
        'python': ('https://docs.python.org/3', None),
        'bokeh':  ('https://bokeh.pydata.org/en/latest/', None)
    }

Es können jedoch auch alternative Dateien für eine Bestandsaufnahme angegeben werden, :abbr:`z.B. (zum Beispiel)`:

.. code-block:: python

    intersphinx_mapping = {
        'python': ('https://docs.python.org/3', (None, 'python-inv.txt'),
        ...
    }

Bestimmen von Linkzielen
------------------------

Um die in einem Inventar verfügbaren Links zu ermitteln, könnt ihr :abbr:`z.B.
(zum Beispiel)` Folgendes eingeben:

.. code-block:: console

    $ python -m sphinx.ext.intersphinx https://docs.python.org/3/objects.inv
    c:function
        PyAnySet_Check                           c-api/set.html#c.PyAnySet_Check
        PyAnySet_CheckExact                      c-api/set.html#c.PyAnySet_CheckExact
        PyArg_Parse                              c-api/arg.html#c.PyArg_Parse
    …

Einen Link erstellen
--------------------

Um einen Link auf https://docs.python.org/3/c-api/arg.html#c.PyArg_Parse zu
erstellen, kann eine der folgenden Varianten angegeben werden:

:c:func:`PyArg_Parse`
    .. code-block:: rest

        :c:func:`PyArg_Parse`

:c:func:`!PyArg_Parse`
    .. code-block:: rest

        :c:func:`!PyArg_Parse`

:c:func:`Parsing arguments <PyArg_Parse>`
    .. code-block:: rest

        :c:func:`Parsing arguments <PyArg_Parse>`

Benutzerdefinierte Links
------------------------

Ihr könnt auch eure eigenen ``intersphinx``-Zuweisungen erstellen, wenn
:abbr:`z.B. (zum Beispiel)` ``objects.inv`` Fehler aufweis wie bei `Beautiful
Soup <https://bugs.launchpad.net/beautifulsoup/+bug/1453370>`_.

Der Fehler kann mit korrigiert werden:

#. Installation of ``sphobjinv``:

   .. code-block:: console

    $ python -m pip install sphobjinv

#. Dann könnt ihr die Originaldatei herunterladen mit:

   .. code-block:: console

    $ cd docs/build/
    $ mkdir _intersphinx
    $ !$
    $ curl -O https://www.crummy.com/software/BeautifulSoup/bs4/doc/objects.inv
    $ mv objects.inv bs4_objects.inv

#. Ändert die Sphinx-Konfiguration ``docs/conf.py``:

   .. code-block:: console

    intersphinx_mapping = {
        …
        'bs4':    ('https://www.crummy.com/software/BeautifulSoup/bs4/doc/', "_intersphinx/bs4_objects.inv")
    }

#. Konvertiert die Datei in eine Textdatei:

   .. code-block:: console

    $ sphobjinv convert plain bs4_objects.inv bs4_objects.txt

#. Editiert die Textdatei, :abbr:`z.B. (zum Beispiel)`:

   .. code-block:: console

    bs4.BeautifulSoup           py:class  1 index.html#beautifulsoup -
    bs4.BeautifulSoup.get_text  py:method 1 index.html#get-text      -
    bs4.element.Tag             py:class  1 index.html#tag           -

   Diese Einträge können dann in einer Sphinx-Dokumentation mit referenziert
   werden:

   .. code-block:: rest

    - :class:`bs4.BeautifulSoup`
    - :meth:`bs4.BeautifulSoup.get_text`
    - :class:`bs4.element.Tag`

   .. seealso::
      * `Sphinx objects.inv v2 Syntax
        <https://sphobjinv.readthedocs.io/en/latest/syntax.html>`_

#. Erstellt eine neue ``objects.inv``-Datei:

   .. code-block:: console

        $ sphobjinv convert zlib bs4_objects.txt bs4_objects.txt

#. Erstellt eine neue Sphinx-Dokumentation:

   .. code-block:: console

        $ python -m sphinx -ab html docs/ docs/_build/

Rollen hinzufügen
-----------------

Wenn ihr eine Fehlermeldung erhaltet, dass eine bestimmte Textrolle unbekannt
ist, :abbr:`z.B. (zum Beispiel)`:

.. code-block:: console

    WARNING: Unknown interpreted text role "confval".

so könnt ihr sie in der ``conf.py`` hinzufügen:

.. code-block:: python

    def setup(app):
        # from sphinx.ext.autodoc import cut_lines
        # app.connect('autodoc-process-docstring', cut_lines(4, what=['module']))
        app.add_object_type(
            "confval",
            "confval",
            objname="configuration value",
            indextemplate="pair: %s; configuration value",
        )
