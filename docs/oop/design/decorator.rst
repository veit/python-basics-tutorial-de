Decorator
=========

Der Decorator ist ein Strukturmuster (:abbr:`engl. (englisch)` *structural
patterns*). Das Muster ist eine flexible Alternative zur Unterklassen-Bildung,
um eine Klasse um zusätzliche Funktionalitäten zu erweitern.

.. warning::
   Das Decorator-Pattern hat nichts mit Python-:doc:`../../functions/decorators`
   zu tun.

Beispiel
--------

Im Python-Wiki findet ihr ein Beispiel für das `DecoratorPattern
<https://wiki.python.org/moin/DecoratorPattern>`_. Es zeigt uns, wie Dekoratoren
in die Pipeline eingebaut werden, um dynamisch viele Verhaltensweisen in ein
Objekt einzufügen.

Vor- und Nachteile
------------------

Vorteile

* Mehrere Dekorierer können hintereinandergeschaltet werden
* Die Dekorierer können zur Laufzeit und sogar nach der Instanziierung
  ausgetauscht werden.
* Die zu dekorierende Klasse ist nicht unbedingt festgelegt, wohl aber deren
  Schnittstelle.
* Zudem können lange und unübersichtliche Vererbungshierarchien vermieden
  werden.

Nachteile

* Da eine dekorierte Komponente nicht identisch mit der Komponente selbst ist,
  muss man beim Testen auf Objekt-Identität vorsichtig sein.
* Bei der Verwendung von dekorierten Komponenten müssen die Nachrichten vom
  Dekorierer an das dekorierte Objekt weitergeleitet werden.
