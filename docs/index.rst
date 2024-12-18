=============
Python Basics
=============

Willkommen bei den Python Basics! Ich habe dieses Buch geschrieben, um einen
einfachen und praxisnahen Einstieg in Python zu ermöglichen. Das Buch ist nicht
als umfassendes Nachschlagewerk für Python gedacht, sondern das Ziel ist
vielmehr, euch grundlegend mit Python vertraut zu machen und euch schnell das
Schreiben eigener Programme zu ermöglichen.

Das Buch ist veröffentlicht unter der `BSD-3-Clause license
<https://github.com/veit/python-basics-tutorial-de?tab=BSD-3-Clause-1-ov-file#readme>`_,
so dass ihr das Buch :abbr:`u.a. (unter anderem)` für eure Zwecke anpassen und
veröffentlichen dürft, sofern ihr die Lizenz und den Copyright-Hinweis
beibehaltet.

Ich möchte der `cusy GmbH <https://cusy.io/de/front-page>`_ danken, die mir
großzügig ermöglicht, meine Zeit mit dem Schreiben dieses Buches zu verbringen.
Darüberhinaus möchte ich Kristian Rother nicht nur für die Unterstützung und
den Rat danken, den er mir im Laufe der Jahre zu diesem Buch gegeben hat,
sondern auch für das Lektorat, das dieses Buch besser gemacht hat. Danken möchte
ich auch Steffen Dahlem, für die Idee und Realisierung einer nachhaltigeren
Buchproduktion. Ein herzlicher Dank geht schließlich auch an die
Rezensent*innen, deren Einblicke und Rückmeldungen eine große Hilfe waren.

.. note::
   Wenn ihr Vorschläge für Verbesserungen und Ergänzungen habt, freue ich mich
   über eure :doc:`Verbesserungsvorschläge <contribute>`.

Das Python Basics Tutorial ist Teil einer Reihe von Tutorials zur Datenanalyse
und -visualisierung:

* :doc:`jupyter-tutorial:index`
* `Python für Data Science <https://www.python4data.science/de/latest/>`_
* `PyViz-Tutorial <https://pyviz-tutorial.readthedocs.io/de/latest/>`_
* `cusy Design-System: Datenvisualisierung
  <https://www.cusy.design/de/latest/viz/index.html>`_

Alle Tutorials dienen als Seminarunterlagen für unsere aufeinander abgestimmten
Trainings:

+---------------+--------------------------------------------------------------+
| Dauer         | Titel                                                        |
+===============+==============================================================+
| 3 Tage        | `Einführung in Python`_                                      |
+---------------+--------------------------------------------------------------+
| 2 Tage        | `Fortgeschrittenes Python`_                                  |
+---------------+--------------------------------------------------------------+
| 2 Tage        | `Entwurfsmuster in Python`_                                  |
+---------------+--------------------------------------------------------------+
| 2 Tage        | `Effizient Testen mit Python`_                               |
+---------------+--------------------------------------------------------------+
| 1 Tag         | `Software-Dokumentation mit Sphinx`_                         |
+---------------+--------------------------------------------------------------+
| 2 Tage        | `Technisches Schreiben`_                                     |
+---------------+--------------------------------------------------------------+
| 3 Tage        | `Jupyter-Notebooks für effiziente Data-Science-Workflows`_   |
+---------------+--------------------------------------------------------------+
| 2 Tage        | `Numerische Berechnungen mit NumPy`_                         |
+---------------+--------------------------------------------------------------+
| 2 Tage        | `Daten analysieren mit pandas`_                              |
+---------------+--------------------------------------------------------------+
| 3 Tage        | `Daten lesen, schreiben und bereitstellen mit Python`_       |
+---------------+--------------------------------------------------------------+
| 2 Tage        | `Daten bereinigen und validieren mit Python`_                |
+---------------+--------------------------------------------------------------+
| 5 Tage        | `Daten visualisieren mit Python`_                            |
+---------------+--------------------------------------------------------------+
| 1 Tag         | `Datenvisualisierungen gestalten`_                           |
+---------------+--------------------------------------------------------------+
| 2 Tage        | `Dashboards erstellen`_                                      |
+---------------+--------------------------------------------------------------+
| 3 Tage        | `Code und Daten versioniert und reproduzierbar speichern`_   |
+---------------+--------------------------------------------------------------+
| Abonnement    | `Neues aus Python für Data-Science`_                         |
| à 2 h im      |                                                              |
| Quartal       |                                                              |
+---------------+--------------------------------------------------------------+

.. _`Einführung in Python`:
   https://cusy.io/de/unsere-schulungsangebote/einfuehrung-in-python
.. _`Fortgeschrittenes Python`:
   https://cusy.io/de/unsere-schulungsangebote/fortgeschrittenes-python
.. _`Entwurfsmuster in Python`:
   https://cusy.io/de/unsere-schulungsangebote/entwurfsmuster-in-python
.. _`Effizient Testen mit Python`:
   https://cusy.io/de/unsere-schulungsangebote/effizient-testen-mit-python
.. _`Software-Dokumentation mit Sphinx`:
   https://cusy.io/de/unsere-schulungsangebote/software-dokumentation-mit-sphinx
.. _`Technisches Schreiben`:
   https://cusy.io/de/unsere-schulungsangebote/technisches-schreiben
.. _`Jupyter-Notebooks für effiziente Data-Science-Workflows`:
   https://cusy.io/de/unsere-schulungsangebote/jupyter-notebooks-fuer-effiziente-data-science-workflows
.. _`Numerische Berechnungen mit NumPy`:
   https://cusy.io/de/unsere-schulungsangebote/numerische-berechnungen-mit-numpy
.. _`Daten analysieren mit pandas`:
   https://cusy.io/de/unsere-schulungsangebote/daten-analysieren-mit-pandas
.. _`Daten lesen, schreiben und bereitstellen mit Python`:
   https://cusy.io/de/unsere-schulungsangebote/daten-lesen-schreiben-und-bereitstellen-mit-python
.. _`Daten bereinigen und validieren mit Python`:
   https://cusy.io/de/unsere-schulungsangebote/daten-bereinigen-und-validieren-mit-python
.. _`Daten visualisieren mit Python`:
   https://cusy.io/de/unsere-schulungsangebote/daten-visualisieren-mit-python
.. _`Datenvisualisierungen gestalten`:
   https://cusy.io/de/unsere-schulungsangebote/datenvisualisierungen-gestalten
.. _`Dashboards erstellen`:
   https://cusy.io/de/unsere-schulungsangebote/dashboards-erstellen
.. _`Code und Daten versioniert und reproduzierbar speichern`:
   https://cusy.io/de/unsere-schulungsangebote/code-und-daten-versioniert-und-reproduzierbar-speichern
.. _`Neues aus Python für Data-Science`:
   https://cusy.io/de/unsere-schulungsangebote/neues-aus-python-fuer-data-science

Folgt uns auf…

.. include:: ../README.rst
   :start-after: follow-us:
   :end-before: end-follow-us:

.. toctree::
   :titlesonly:
   :hidden:

   intro
   changelog
   install
   editors
   explore
   style
   variables-expressions
   types/index
   control-flows/index
   functions/index
   modules/index
   libs/index
   packs/index
   oop/index
   save-data/index
   logging/index
   test/index
   document/index
   appendix/index

.. Indizes und Tabellen
   ====================

   * :ref:`genindex`
   * :ref:`modindex`
   * :ref:`search`
