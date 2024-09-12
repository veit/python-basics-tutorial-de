Strategiemuster
===============

Im Entwurfsmuster-Buch wird das `Strategiemuster
<https://de.wikipedia.org/wiki/Strategie_(Entwurfsmuster)>`_ definiert als eine
Familie von Algorithmen, die gekapselt und austauschbar sein sollen. Dabei
variieren die Algorithmen unabhängig von den Klienten.

.. uml::

    title UML-Klassendiagramm für das Strategie-Entwurfsmuster

    abstract class  "Client"
    Client --> Context
    Client --> Strategy

    together {
        interface Context {
            {method} context_interface()
        }
        abstract class Strategy {
            {method} algorithm()
        }
    }
    Context o-> Strategy

    together {
        class ConcreteStrategyA {
            {method} algorithm()
        }
        class ConcreteStrategyB {
            {method} algorithm()
        }
    }
    ConcreteStrategyA -u-|> Strategy
    ConcreteStrategyB -u-|> Strategy

Das Strategiemuster ist ein gutes Beispiel für ein Entwurfsmuster, das in Python
einfacher sein kann, wenn Funktionen als First-Class-Objekte benutzt werden.
Hierfür implementieren wir zunächst die klassische Struktur dieses Musters und
refaktorisieren anschließend diesen Code mithilfe von Funktionen.

Ein anschauliches Beispiel für die Anwendung des Strategiemusters ist die
Berechnung von Rabatten auf Bestellungen in Abhängigkeit von den Eigenschaften
der Kund*innen und der bestellten Artikel.

Nehmen wir ein Online-Geschäft mit den folgenden Rabattregeln:

- Kunden mit tausend oder mehr Treuepunkten erhalten einen globalen Rabatt von
  5 % pro Bestellung.
- Ein Rabatt von 10 % wird auf jede Position mit zehn oder mehr Einheiten in
  derselben Bestellung gewährt.
- Auf Bestellungen mit mindestens zehn verschiedenen Artikeln wird ein Rabatt
  von 7 % gewährt.

Dabei kann nur ein Rabatt auf eine Bestellung angewendet werden.

Kontext
    hält eine Variable von Strategie, die auf eine konkrete Strategie
    referenziert. In unserem E-Commerce-Beispiel ist der
    Kontext eine Bestellung (engl.: :samp:`Order`, die so konfiguriert ist, dass
    sie einen Aktionsrabatt nach einem von mehreren Algorithmen anwendet.
Strategie
    ist die gemeinsame Schnittstelle für die Komponenten, die die verschiedenen
    Algorithmen implementieren. In unserem Beispiel wird diese Rolle von einer
    abstrakten Klasse namens :samp:`Discount` übernommen.
Konkrete Strategie
    ist eine der konkreten Unterklassen der abstrakten Strategie.
    :samp:`LoyaltyDiscount`, :samp:`QuantityDiscount` und :samp:`BulkDiscount`
    sind die drei implementierten konkreten Strategien.

.. literalinclude:: strategy.py
   :language: python
   :linenos:

Funktionsorientierte Strategie
------------------------------

Jede konkrete Strategie im vorigen Beispiel ist eine Klasse mit einer einzigen
Methode, :func:`discount`. Darüber hinaus haben die Strategieinstanzen keinen
Zustand (keine Instanzattribute). Im folgenden Beispiel machen wir ein
Refactoring, wobei die konkreten Strategien durch einfache Funktionen ersetzt
werden und die abstrakte Klasse :class:`Promotion` entfernt wird.

.. literalinclude:: promos.py
   :language: python
   :linenos:
   :lines: 1-57

Zeile 33:
    Um einen Rabatt zu berechnen, ruft einfach die Funktion
    :func:`self.promotion` auf.
Zeile 40:
    Jede Strategie ist eine Funktion und keine Klasse.

Die Autoren des Entwurfsmuster-Buch schlagen die gemeinsame Nutzung mit dem
`Fliegengewicht
<https://de.wikipedia.org/wiki/Fliegengewicht_(Entwurfsmuster)>`_-Entwurfsmuster
vor:

    Strategieobjekte sind oft gute Fliegengewichte.

    Ein Fliegengewicht ist ein gemeinsam genutztes Objekt, das in mehreren
    Kontexten gleichzeitig verwendet werden kann.

Die gemeinsame Nutzung wird empfohlen, um die Kosten für die Erstellung eines
neuen konkreten Strategieobjekts zu verringern, wenn dieselbe Strategie immer
wieder in jedem neuen Kontext angewendet wird – in unserem Beispiel bei jeder
neuen Bestellinstanz. Um also einen Nachteil des Strategiemusters zu überwinden
– seine Laufzeitkosten – empfehlen die Autoren die Anwendung eines weiteren
Musters. In der Zwischenzeit türmen sich die Codemenge und die Wartungskosten.

.. tip::
   In einem schwierigeren Anwendungsfall mit komplexen konkreten Strategien,
   die einen internen Zustand enthalten, können alle Teile des Strategie- und
   Fliegengewichtmusters kombiniert werden. Aber oft haben konkrete Strategien
   keinen internen Zustand; sie verarbeiten nur Daten aus dem Kontext. In diesem
   Fall solltet Sie auf jeden Fall einfache Funktionen verwenden, anstatt
   Ein-Methoden-Klassen zu kodieren, die eine Ein-Methoden-Schnittstelle
   implementieren, die in einer anderen Klasse deklariert ist. Eine Funktion ist
   leichtgewichtiger als eine Instanz einer benutzerdefinierten Klasse, und es
   besteht keine Notwendigkeit für die Fliegengewicht-Strategie, da jede
   Strategie-Funktion nur einmal von Python erstellt wird, wenn das :doc:`Modul
   <../../modules/index>` kompiliert wird. Eine einfache Funktion ist auch *ein
   gemeinsam genutztes Objekt, das in mehreren Kontexten gleichzeitig verwendet
   werden kann*.

Dabei kann hilfreich sein, dass sich die eingebaute Funktion :py:func:`globals`
innerhalb einer Funktion oder Methode immer auf das Modul, in dem diese Funktion
oder Methode definiert ist, bezieht – und nicht auf das Modul, aus dem sie
aufgerufen wird.

So kann :py:func:`globals` dazu verwendet werden, um alle im Modul verfügbaren
:samp:`{special}_promo`-Funktionen automatisch zu finden:

.. literalinclude:: promos.py
   :language: python
   :lines: 60
   :lineno-start: 60

Dies iteriert über jeden Namen im :doc:`Dictionary <../../types/dicts>`, das von
:py:func:`globals` zurückgegeben wird und wählt nur diejenigen Namen aus, die
mit dem Suffix ``_promo`` enden.

Um die :samp:`{special}_promo`-Funktionen in einem anderen Modul zu finden, kann
die :doc:`inspect <python3:library/inspect>`-Bibliothek verwendet werden:

.. literalinclude:: best_promo.py
   :language: python
   :linenos:

Die Funktion :py:func:`inspect.getmembers` gibt die Attribute eines Objekts
zurück – in diesem Fall das :mod:`promos`. Anschließend verwenden wir
:py:func:`inspect.isfunction`, um nur die Funktionen des Moduls zu erhalten.
Dieses Beispiel funktioniert unabhängig von den Namen der Funktionen; wichtig
ist nur, dass das :mod:`promos`-Modul die relevanten Funktionen enthält.
