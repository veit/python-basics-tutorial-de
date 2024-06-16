Abstrakte Fabrik
================

Die abstrakte Fabrik (:abbr:`engl. (englisch)` *abstract factory* ist ein
Erzeugungsmuster (:abbr:`engl. (englisch)` *creational pattern*), das eine
Schnittstelle zur Erzeugung von Objekten definiert, wobei die konkreten Klassen
der zu instanziierenden Objekte nicht spezifiziert werden.

Das Abstract Factory Pattern besteht aus den folgenden Kernkomponenten:

Abstrakte Fabrik
    Diese abstrakte Klasse definiert die Schnittstelle zur Erstellung von
    Familien verwandter Objekte. Sie deklariert Methoden zur Erstellung jedes
    Objekttyps innerhalb einer Familie.
Konkrete Fabriken
    Konkrete Fabrikklassen implementieren die abstrakte Fabrikschnittstelle.
    Jede konkrete Fabrik ist für die Erstellung eines bestimmten Satzes
    verwandter Objekte zuständig.
Abstrakte Produkte
    Dies sind die abstrakten Klassen, die die Schnittstellen für einzelne
    Produkte innerhalb jeder Familie definieren. Jede abstrakte Produktklasse
    entspricht einer bestimmten Art von Objekt.
Konkrete Produkte
    Konkrete Produktklassen implementieren die abstrakten Produktschnittstellen.
    Diese Klassen stellen die eigentlichen Objekte dar, die von den konkreten
    Fabriken erzeugt werden.

Beispiel
--------

In Python können wir dies mit :py:class:`abc.ABC` und
:py:func:`abc.abstractmethod` realisieren:

.. code-block:: python

    from abc import ABC, abstractmethod


    # Abstract Products
    class Oval(ABC):
        @abstractmethod
        def circumference(self):
            pass


    class Polygon(ABC):
        @abstractmethod
        def circumference(self):
            pass


    # Concrete Products
    class Circular(Oval):
        def circumference(self):
            return "Circular Oval"


    class SquarePolygon(Polygon):
        def circumference(self):
            return "Square Polygon"


    # Abstract Factory
    class FormFactory(ABC):
        @abstractmethod
        def create_oval(self):
            pass

        @abstractmethod
        def create_polygon(self):
            pass


    # Concrete Factories
    class CircularFactory(FormFactory):
        def create_oval(self):
            return CircularOval()

        def create_polygon(self):
            return None  # Circular doesn’t make polygons


    class SquareFactory(FormFactory):
        def create_oval(self):
            return None  # Square doesn't make ovals

        def create_polygon(self):
            return SquarePolygon()

Vor- und Nachteile
------------------

Vorteile:

* Der Klient ist von konkreten Klassen isoliert.
* Der Austausch von Produktfamilien ist auf einfache Art und Weise möglich.

Nachteile

* Neue Produktarten lassen sich schwer hinzufügen, da in allen konkreten
  Fabriken Änderungen vorzunehmen sind.
