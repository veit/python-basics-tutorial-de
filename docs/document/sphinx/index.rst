Sphinx
======

Für umfangreiche Dokumentationen könnt ihr :abbr:`z.B.(zum Beispiel)` `Sphinx
<https://www.sphinx-doc.org/>`_ verwenden, ein Dokumentationswerkzeug, das
reStructuredText in HTML oder PDF, EPub und man pages umwandelt. Auch die Python
Basics werden mit Sphinx erstellt. Um einen ersten Eindruck von Sphinx zu
bekommen, könnt ihr euch den Quellcode dieser Seite unter dem Link `Page source
<../_sources/document/index.rst.txt>`_ ansehen.

Ursprünglich wurde Sphinx für die Dokumentation von Python entwickelt und wird
heute in fast allen Python-Projekten verwendet, darunter `NumPy and SciPy
<https://docs.scipy.org/doc/>`_, `Matplotlib
<https://matplotlib.org/stable/users/index.html>`_, `Pandas
<https://pandas.pydata.org/docs/>`_ und `SQLAlchemy
<https://docs.sqlalchemy.org/>`_.

Die Sphinx `autodoc
<https://www.sphinx-doc.org/en/master/usage/extensions/autodoc.html>`_-Funktion,
die zur Erstellung von Dokumentation aus
Python-:doc:`docstrings` verwendet werden kann, könnte ebenfalls zur Verbreitung
von Sphinx unter Python-Entwicklern beitragen. Insgesamt ermöglicht es Sphinx
Entwicklungsteams, eine vollständige Dokumentation an Ort und Stelle zu
erstellen. Oft wird die Dokumentation auch im gleichen  :doc:`Git
<Python4DataScience:productive/git/index>`-Repository gespeichert, so dass die
Erstellung der neuesten Software-Dokumentation einfach bleibt.

Sphinx wird auch in Projekten außerhalb der Python-Gemeinschaft eingesetzt,
:abbr:`z.B. (zum Beispiel)` für die Dokumentation des Linux-Kernels: `Kernel
documentation update <https://lwn.net/Articles/705224/>`_.

`Read the Docs <https://about.readthedocs.com/>`_ wurde entwickelt, um die
Dokumentation weiter zu vereinfachen. Read the Docs erleichtert das Erstellen
und Veröffentlichen von Dokumentationen nach jedem Commit.

Für die Projektdokumentation kann die Visualisierung von :doc:`Git Feature
Branches
<Python4DataScience:productive/git/workflows/feature-branches>` und :doc:`Tags
<Python4DataScience:productive/git/tag>` mit
:doc:`Python4DataScience:productive/git/advanced/git-big-picture` hilfreich
sein.

.. note::
   Wenn der Inhalt von ``long_description`` in ``setup()`` in reStructured Text
   geschrieben ist, wird er als gut formatiertes HTML im :term:`Python Package
   Index` (:term:`PyPI`) angezeigt.

.. toctree::
   :titlesonly:
   :hidden:

   start
   rest
   convert
   code-blocks
   placeholder
   ui-elements
   directives
   docstrings
   intersphinx
   uml/index
   extensions
   test
