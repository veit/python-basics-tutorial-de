Objektorientierte Designs
=========================

In der Softwareentwicklung beschreibt ein Entwurfsmuster (:abbr:`engl.
(englisch)` *design patterns*) einen relativ kleinen, genau definierten Aspekt
eines Computerprogramms in Bezug auf die Art und Weise, wie Code zu schreiben
ist. Die Verwendung eines Musters dient dazu, ein bestehendes Konzept zu nutzen,
anstatt es neu zu erfinden. Dadurch kann die Zeit für die Softwareentwicklung
verkürzt und die Qualität des resultierenden Programms erhöht werden.

    Konformität mit Mustern ist kein Maßstab für Güte. [#]_

Obwohl Entwurfsmuster sprachunabhängig sind, bedeutet das nicht, dass jedes
Muster für jede Sprache passt. In seinem Vortrag *Design Patterns in Dynamic
Languages* aus dem Jahr 1996 stellt Peter Norvig fest, dass 16 der 23 Patterns
aus dem Buch-Klassiker `Entwurfsmuster
<https://de.wikipedia.org/wiki/Entwurfsmuster_(Buch)>`_ in einer dynamischen
Sprache entweder unsichtbar oder einfacher werden [#]_. Auch die Autoren des
Buchers erkennen in ihrer Einleitung an, dass die Implementierungssprache
bestimmt, welche Muster relevant sind:

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

:doc:`SOLID <solid>` ist ein Akronym für fünf Designprinzipien, die
objektorientierte Designs verständlicher, flexibler und wartbarer machen sollen.

.. seealso::
   * Harry Percival, Bob Gregory: `Architecture Patterns with Python
     <https://www.oreilly.com/library/view/architecture-patterns-with/9781492052197/>`_
   * Leonardo Giordani: `Clean Architectures in Python
     <https://www.thedigitalcatbooks.com/pycabook-introduction/>`_
   * Gregor Hohpe, Bobby Woolf: `Enterprise Integration Patterns
     <https://www.pearson.de/enterprise-integration-patterns-designing-building-and-deploying-messaging-solutions-9780133065107>`_

----

.. [#] Ralph Johnson, Co-Autor des `Entwurfsmuster
       <https://de.wikipedia.org/wiki/Entwurfsmuster_(Buch)>`_-Standardwerks.
.. [#] `Design Patterns in Dynamic Languages
       <http://norvig.com/design-patterns/>`_

.. toctree::
   :titlesonly:
   :hidden:

   solid
   factory
   decorator
   strategy
   command
