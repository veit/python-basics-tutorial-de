behave
======

`behave <https://behave.readthedocs.io/en/latest/>`_ implementiert
Behavior-Driven Development (:abbr:`BDD (Behavior-Driven Development)`). BDD ist
eine agile Softwareentwicklungstechnik, die die Zusammenarbeit zwischen
Entwicklung, Qualitätssicherung und nicht-technischen oder Geschäftsführung an
einem Softwareprojekt fördert. Der Begriff wurde ursprünglich 2003 von `Daniel
Terhorst-North <https://dannorth.net/introducing-bdd>`_ als Antwort auf die
:term:`testgetriebene Entwicklung <Testgetriebene Entwicklung>` geprägt und
umfasst :term:`Akzeptanztests <Akzeptanztest>` oder kundentestgetriebene
Entwicklungspraktiken, wie sie in :term:`Extreme Programming` zu finden sind. In
`Selling BDD to the Business
<https://speakerdeck.com/tastapod/selling-bdd-to-the-business>`_ gab er folgende
Definition:

    *„BDD ist eine agile Methodik der zweiten Generation, die von außen nach
    innen arbeitet, auf Pull-Prinzipien basiert, mehrere Interessengruppen
    einbezieht, mehrere Ebenen umfasst, einen hohen Automatisierungsgrad
    aufweist und agil ist. Sie beschreibt einen Zyklus von Interaktionen mit
    klar definierten Ergebnissen, der zur Lieferung funktionsfähiger, getesteter
    und relevanter Software führt.“*

Gherkin
-------

Beschreibungssprache auf der Grundlage natürlicher Schriftsprache zur textuellen
Spezifikation von Software-Anforderungen. Einzig bestimmte Schlüsselwörter sind
vorbelegt.

.. code-block:: gherkin

   Scenario: Add an item to the database
     Given an empty database
      When an item with a summary is added
      Then the number of items should be 1
       and the queried item from the db should correspond to the added object.

Jedes Szenario ist ein Beispiel, das einen bestimmten Aspekt des Verhaltens der
Anwendung veranschaulichen soll.

Installation
------------

Ihr könnt behave in eurer :ref:`virtuellen Umgebungen <venv>` installieren mit:

.. tab:: Linux/macOS

   .. code-block:: console

      $ python -m pip install behave

.. tab:: Windows

   .. code-block:: ps1con

      C:> python -m pip install behave

.. toctree::
   :titlesonly:
   :hidden:

   example
