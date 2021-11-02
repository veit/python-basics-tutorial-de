Dokumentieren
=============

Eine ``README``-Datei im Stammverzeichnis sollte allgemeine Informationen sowohl
für nutzende als auch für betreuende Personen eines Projekts enthalten.

* Sie sollte in einem sehr leicht lesbaren Markup, wie
  :doc:`rest` oder Markdown, geschrieben sein.
* Sie sollte den Zweck des Projekts oder der Bibliothek in einer nicht
  voraussetzungsvollen Weise erklären.
* Sie sollte Links zu den Hauptquellen der Software enthalten.
* Eine ``LICENSE``-Datei sollte immer vorhanden sein, um deutlich zu machen,
  welche Nutzungsrechte gelten.

.. seealso::
   * `Eric Holscher: Why You Shouldn’t Use “Markdown” for Documentation
     <https://ericholscher.com/blog/2016/mar/15/dont-use-markdown-for-technical-docs/>`_
   * `Tom Johnson: 10 reasons for moving away from DITA
     <https://idratherbewriting.com/2015/01/28/10-reasons-for-moving-away-from-dita/>`_
   * `Tom Johnson: Jekyll versus DITA
     <https://idratherbewriting.com/2015/03/23/new-series-jekyll-versus-dita/>`_
   * `Google developer documentation style guide
     <https://developers.google.com/style/>`_
   * `Google Technical Writing Courses for Engineers
     <https://developers.google.com/tech-writing/overview>`_
   * `Cusy Design System: Schreiben
     <https://cusy-design-system.readthedocs.io/de/latest/writing/index.html>`_

.. toctree::
   :titlesonly:
   :hidden:

   start
   rest
   code-blocks
   placeholder
   ui-elements
   directives
   docstrings
   intersphinx
   uml/index
   extensions
   test

Badges
------

Einige dieser Informationen und mehr können als Badges abgerufen werden. Sie
sind hilfreich, um einen schnellen Überblick über ein Produkt zu erhalten. Für
das `cookiecutter-namespace-template
<https://github.com/veit/cookiecutter-namespace-template>`_ sind dies
:abbr:`z.B. (zum Beispiel)`:

|Downloads| |Updates| |Versions| |Contributors| |License| |Docs|

.. |Downloads| image::
   https://static.pepy.tech/badge/cookiecutter-namespace-template
   :target: https://pepy.tech/project/cookiecutter-namespace-template
.. |Updates| image::
   https://pyup.io/repos/github/veit/cookiecutter-namespace-template/shield.svg
   :target: https://pyup.io/repos/github/veit/cookiecutter-namespace-template/
.. |Versions| image::
   https://img.shields.io/pypi/pyversions/cookiecutter-namespace-template.svg
   :target: https://pypi.org/project/cookiecutter-namespace-template/
.. |Contributors| image::
   https://img.shields.io/github/contributors/veit/cookiecutter-namespace-template.svg
   :target: https://github.com/veit/cookiecutter-namespace-template/graphs/contributors
.. |License| image::
   https://img.shields.io/github/license/veit/cookiecutter-namespace-template.svg
   :target: https://github.com/veit/cookiecutter-namespace-template/blob/main/LICENSE
.. |Docs| image::
   https://readthedocs.org/projects/cookiecutter-namespace-template/badge/?version=latest
   :target: https://cookiecutter-namespace-template.readthedocs.io/en/latest/

Sphinx
------

Für umfangreiche Dokumentationen könnt ihr :abbr:`z.B.(zum Beispiel)` `Sphinx
<https://www.sphinx-doc.org/>`_ verwenden, ein Dokumentationswerkzeug, das 
reStructuredText in HTML oder PDF, EPub und man pages umwandelt. Auch die Python
Basics werden mit Sphinx erstellt. Um einen ersten Eindruck von Sphinx zu
bekommen, könnt ihr euch den Quellcode dieser Seite unter dem Link `Page source
<../_sources/document/index.rst.txt>`_ ansehen.

Ursprünglich wurde Sphinx für die Dokumentation von Python entwickelt und wird
heute in fast allen Python-Projekten verwendet, darunter `NumPy and SciPy
<https://docs.scipy.org/doc/>`_, `Matplotlib
<https://matplotlib.org/users/index.html>`_, `Pandas
<https://pandas.pydata.org/docs/>`_ und `SQLAlchemy
<https://docs.sqlalchemy.org/>`_.

Die Sphinx `autodoc
<http://www.sphinx-doc.org/en/master/usage/extensions/autodoc.html>`_-Funktion,
die zur Erstellung von Dokumentation aus
Python-:doc:`docstrings` verwendet werden kann, könnte ebenfalls zur Verbreitung
von Sphinx unter Python-Entwicklern beitragen. Insgesamt ermöglicht es Sphinx
Entwicklungsteams, eine vollständige Dokumentation an Ort und Stelle zu
erstellen. Oft wird die Dokumentation auch im gleichen  :doc:`Git
<jupyter-tutorial:productive/git/index>`-Repository gespeichert, so dass die
Erstellung der neuesten Software-Dokumentation einfach bleibt.

Sphinx wird auch in Projekten außerhalb der Python-Gemeinschaft eingesetzt,
:abbr:`z.B. (zum Beispiel)` für die Dokumentation des Linux-Kernels: `Kernel
documentation update <https://lwn.net/Articles/705224/>`_.

`Read the Docs <https://readthedocs.org/>`_ wurde entwickelt, um die
Dokumentation weiter zu vereinfachen. Read the Docs erleichtert das Erstellen
und Veröffentlichen von Dokumentationen nach jedem Commit.

.. note::
   Wenn der Inhalt von ``long_description`` in ``setuptools.setup()`` in
   reStructured Text geschrieben ist, wird er als gut formatiertes HTML im
   :term:`Python Package Index (PyPI)` angezeigt.

.. seealso::
   * `reStructuredText Primer
     <https://www.sphinx-doc.org/en/master/usage/restructuredtext/basics.html>`_
   * `reStructuredText Quick Reference
     <https://docutils.sourceforge.io/docs/user/rst/quickref.html>`_

Andere Dokumentationswerkzeuge
------------------------------

`Pycco <https://pycco-docs.github.io/pycco/>`_
    ist eine Python-Portierung von `Docco
    <https://github.com/jashkenas/docco>`_.
