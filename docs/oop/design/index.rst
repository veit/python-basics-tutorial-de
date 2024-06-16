Entwurfsmuster
==============

    Konformität mit Mustern ist kein Maßstab für Güte. [#]_

Obwohl Entwurfsmuster (:abbr:`engl. (englisch)` *design patterns*
sprachunabhängig sind, bedeutet das nicht, dass jedes Muster für jede Sprache
passt. In seinem Vortrag *Design Patterns in Dynamic Languages* aus dem Jahr
1996 stellt Peter Norvig fest, dass 16 der 23 Patterns aus dem ursprünglichen
Buch `Entwurfsmuster
<https://de.wikipedia.org/wiki/Entwurfsmuster_(Buch)>`_-Buch in einer
dynamischen Sprache entweder unsichtbar oder einfacher werden [#]_.
Die Autoren von *Entwurfsmuster* erkennen in ihrer Einleitung an, dass die
Implementierungssprache bestimmt, welche Muster relevant sind:

    Die Wahl der Programmiersprache ist wichtig, weil sie den Blickwinkel
    beeinflusst. Unsere Muster gehen von Smalltalk/C++-Sprachmerkmalen aus, und
    diese Wahl bestimmt, was leicht implementiert werden kann und was nicht.
    Wären wir von prozeduralen Sprachen ausgegangen, hätten wir vielleicht
    Entwurfsmuster mit den Bezeichnungen *Vererbung*, *Kapselung* und
    *Polymorphismus* aufgenommen. In ähnlicher Weise werden einige unserer
    Muster direkt von den weniger verbreiteten objektorientierten Sprachen
    unterstützt.

Norvig schlägt :abbr:`u.a. (unter anderem)` vor, das Strategiemuster mit
Instanzen einiger Klassen durch einfache Funktionen zu ersetzen und so eine
Menge Boilerplate-Code zu reduzieren. Im folgenden :doc:`Strategiemuster
<strategy>`-Abschnitt werden wir das Strategiemuster mithilfe von
Funktionsobjekten refaktorisieren.

----

.. [#] Ralph Johnson, Co-Autor des `Entwurfsmuster
       <https://de.wikipedia.org/wiki/Entwurfsmuster_(Buch)>`_-Standardwerks.
.. [#] `Design Patterns in Dynamic Languages
       <http://norvig.com/design-patterns/>`_

.. toctree::
   :titlesonly:
   :hidden:

   factory
   decorator
   strategy
   command
