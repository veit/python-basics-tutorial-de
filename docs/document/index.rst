Dokumentieren
=============

Damit euer Software-Paket sinnvoll genutzt werden kann, sind Dokumentationen
ierforderlich, die Beschreiben, wie eure Software installiert, betrieben,
genutzt und verbessert werden kann:

* Diejenigen, die euer Paket nutzen wollen, benötigen Informationen,

  * welche Probleme eure Software löst und was die Hauptfunktionen und
    Limitationen der Software sind (``README``)
  * wie das Software beispielhaft verwendet werden kann
  * welche Veränderungen in aktuelleren Software-Versionen gekommen sind
    (``CHANGELOG``)

* Diejenigen, die die Software betreiben wollen, benötigen eine
  Installationsanleitung für eure Software und die erforderlichen Abhängigkeiten

* Diejenigen, die die Software verbessern wollen, benötigen Informationen

  * wie sie mit Fehlerbehebungen zur Verbesserung des Produkts beitragen können
    (``CONTRIBUTING``)
  * wie sie mit anderen kommunizieren (``CODE_OF_CONDUCT``) können

Alle gemeinsam benötigen Informationen, wie das Produkt lizenziert ist
(``LICENSE``-Datei oder ``LICENSES``-Ordner) und wie sie bei Bedarf Hilfe
erhalten können.

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
   shot-scraper

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
   https://img.shields.io/pypi/pyversions/cookiecutter-namespace-template/0.2.9
   :target: https://pypi.org/project/cookiecutter-namespace-template/0.2.9/
.. |Contributors| image::
   https://img.shields.io/github/contributors/veit/cookiecutter-namespace-template.svg
   :target: https://github.com/veit/cookiecutter-namespace-template/graphs/contributors
.. |License| image::
   https://img.shields.io/github/license/veit/cookiecutter-namespace-template.svg
   :target: https://github.com/veit/cookiecutter-namespace-template/blob/main/LICENSE
.. |Docs| image::
   https://readthedocs.org/projects/cookiecutter-namespace-template/badge/?version=latest
   :target: https://cookiecutter-namespace-template.readthedocs.io/en/latest/

Ihr könnt auch eigene Badges erstellen, :abbr:`z.B. (zum Beispiel)`:

.. image:: https://img.shields.io/badge/dynamic/json?label=Mastodon&query=totalItems&url=https%3A%2F%2Fmastodon.social%2F@JupyterTutorial%2Ffollowers.json&logo=mastodon
   :alt: Mastodon
   :target: https://mastodon.social/@JupyterTutorial

.. seealso::
    * `shields.io <https://shields.io>`_

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
<Python4DataScience:productive/git/index>`-Repository gespeichert, so dass die
Erstellung der neuesten Software-Dokumentation einfach bleibt.

Sphinx wird auch in Projekten außerhalb der Python-Gemeinschaft eingesetzt,
:abbr:`z.B. (zum Beispiel)` für die Dokumentation des Linux-Kernels: `Kernel
documentation update <https://lwn.net/Articles/705224/>`_.

`Read the Docs <https://readthedocs.org/>`_ wurde entwickelt, um die
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

Andere Dokumentationswerkzeuge
------------------------------

`Pycco <https://pycco-docs.github.io/pycco/>`_
    ist eine Python-Portierung von `Docco
    <https://github.com/jashkenas/docco>`_.
