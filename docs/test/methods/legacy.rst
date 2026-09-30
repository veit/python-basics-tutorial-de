Legacy-Software
===============

Es kann verschiedene Gründe geben, bestehende Software ändern zu wollen:

* ein Feature hinzufügen
* einen Fehler beheben
* die Software-Architektur verbessern (→ :ref:`refactoring`)
* den Ressourcenverbrauch verbessern

Sofern eine bestehende Code-Basis über keine nennenswerte Testabdeckung verfügt,
wird häufig bestehender Code geändert und anschließend überprüft, ob das
gewünschte Feature ausgeführt wird oder der Fehler behoben ist. Vielleicht wird
auch noch geschaut, ob durch die Änderung nicht andere Features kaputt gegangen
sind; dennoch bleibt unsicher, ob alle Auswirkungen der Code-Änderung auch
berücksichtigt wurden.

Bei :doc:`testgetriebener Entwicklung <tdd>` für Legacy-Software werden zunächst
für den zu ändernden und allen darauf aufbauenden Code Tests geschrieben. Mit
solchen :term:`Regressionstests <Regressionstest>` können wir Änderungen
erkennen und festzustellen, ob die Software noch genauso funktioniert wie in der
Vergangenheit. Damit kann gewährleistet werden, dass dass nur das geändert wird,
was auch beabsichtigt ist. Meist werden Regressionstests jedoch an der
Anwendungsschnittstelle durchgeführt, sodass sie einige Probleme mit sich
bringen:

Fehlerlokalisierung
    Je weiter sich Tests von dem entfernen, was ihr eigentlich testen sollt,
    desto schwieriger wird es, die eigentliche Fehlerursache zu finden.
Ausführungszeit
    Größere Tests benötigen auch mehr Zeit für die Ausführung. Dies führt dazu,
    dass Testläufe frustrierend lang laufen können. Tests, deren Ausführung zu
    lange dauert, werden meist sehr viel seltener ausgeführt.

Unit-Tests erleichtern die Fehlerlokalisierung und verkürzen die
Ausführungszeit. Im Gegensatz zu Regressionstests
Sie schließen bbLücken, die größere Tests nicht abdecken können:

* können Code-Abschnitte unabhängig voneinander getestet werden
* können Tests so gruppiert werden, dass unter bestimmten Bedingungen nur
  einige ausgeführt werden und andere unter anderen Bedingungen.
* können wir mit ihrer Hilfe Fehler schnell lokalisieren.

Warum schreiben wir also nicht einfach Unit-Tests? Häufig verhindern dies
Abhängigkeitsprobleme. Wenn Objekte direkt von etwas abhängen, das in einem Test
schwer zu verwenden ist, lassen sich diese Abhängigkeiten auch nur schwer ändern
und schwer handhaben. Häufig Ein besteht ein Großteil der Arbeit an Legacy-Code
darin, solche Abhängigkeiten aufzubrechen, damit Änderungen einfacher
vorgenommen werden können. Daher können wir häufig keine Unit-Tests einrichten,
ohne dass wir vorher den Code ändern. Daher sind Unit-Tests in solchen Fällen
wenig praktikabel.

Wenn wir Abhängigkeiten auflösen, können wir Tests schreiben, die
tiefgreifendere Änderungen sicherer machen. Solche anfänglichen Refactorings
sollten jedoch sehr konservativ durchgeführt werden, sodass die Gefahr gering
bleibt, Fehler einzuführen. Wenn wir das tun, kann der Code in diesem Bereich
am Ende etwas weniger gut aussehen.

    *„Sie sind wie Schnittstellen bei einer Operation: Nach der Arbeit bleibt
    vielleicht eine Narbe im Code zurück, aber alles darunter kann besser
    werden.“*

– Michael C. Feathers: `Working Effectively with Legacy Code
<https://www.pearson.de/working-effectively-with-legacy-code-9780131177055>`_


Das Ziel bei Legacy-Code ist es, funktionale Änderungen vorzunehmen, die einen
Mehrwert bieten und gleichzeitig einen größeren Teil des Systems in die Tests
einbeziehen. Am Ende jedes Programmier-Zyklus sollten wir nicht nur auf Code
verweisen können, der eine neue Funktion bereitstellt, sondern auch auf die
dazugehörigen Tests. Zukünftig wird das Arbeiten an bereits getestetem Code
wesentlich einfacher werden. Folgende Schritte könnt ihr anwenden, Wenn ihr
Änderungen an Legacy-Code vornehmen müsst:

#. **Zu ändernde Code-Stelle identifizieren.** An welchen Stellen Änderungen
   vorgenommen werden müssen, hängt stark von der Architektur ab.
#. :ref:`find-test-opportunities`: In manchen Fällen ist es einfach, geeignete
   Stellen für das Schreiben von Tests zu finden, doch bei Legacy-Code kann dies
   oft auch schwierig sein.
#. :ref:`resolve-dependencies`: Abhängigkeiten sind oft das offensichtlichste
   Hindernis beim Testen, da sie in Testumgebungen Schwierigkeiten bereiten beim
   Instanziieren von Objekten oder beim Ausführen von Methoden. Bei Legacy-Code
   müssen häufig Abhängigkeiten aufgehoben werden, um Tests einrichten zu
   können. Im Idealfall hätten wir Tests, die uns zeigen, ob die Maßnahmen, die
   wir zur Aufhebung von Abhängigkeiten ergreifen, selbst Probleme verursacht
   haben – doch dies ist oft nicht der Fall.
#. :ref:`write-tests`: Die Tests für Legacy-Code können sich etwas von denen für
   neuen Code unterscheiden.
#. :ref:`refactoring`: Nachdem wir Änderungen am Legacy-Code vorgenommen haben,
   sind wir oft besser mit dessen Problemen vertraut, und die Tests, die wir zum
   Hinzufügen von Funktionen geschrieben haben, bieten uns oft eine gewisse
   Absicherung, um ein Refactoring durchführen zu können. Oft bedeutet dies,
   dass der Code ein bisschen wartbarer als zuvor geworden ist. Aber
   unterschätzt diese Arbeit nicht: einfache Maßnahmen – wie :abbr:`z. B. (zum
   Beispiel)` das Aufteilen einer großen Klasse, kann einen erheblichen
   Unterschied in Anwendungen ausmachen, auch wenn sie etwas mechanisch anmuten.

.. _find-test-opportunities:

Testmöglichkeiten finden
------------------------

Sprout
~~~~~~

Sprout könnt ihr verwenden, wenn ihr einem System eine Funktion oder Klasse
hinzufügen müsst und diese vollständig neu formuliert werden kann. Schreibt den
Code an der Stelle, an der die neue Funktionalität benötigt wird. Es mag sein,
dass ihr die Aufrufstellen nicht ohne Weiteres testen könnt, aber zumindest
können ihr Tests für euren neuen Code schreiben.

Im Grunde genommen geben wir damit jedoch die Verbesserung und das Testen der
ursprünglichen Funktion oder Klasse auf. Wir fügen lediglich neue Features
hinzu. Dennoch ist dies manchmal der praktikabelbste Weg, auch wenn der Code
damit in einer Art Schwebezustand bleibt: es ist nicht wirklich klar, warum
gerade diese Funktion oder Klasse an anderer Stelle stattfindet. Umgekehrt wird
jedoch der neue Code klar vom alten getrennt: eure Änderungen können separat
betrachtet werden und haben eine saubere Schnittstelle zum alten Code.

Wrap
~~~~

Für Wrap wird eine Funktion mit dem Namen der ursprünglichen Funktion erstellt
und diese verweist dann auf unseren ursprünglichen Code. Mit dieser
Vorgehensweise können wir Aufrufen der ursprünglichen Funktion ein neues
Verhalten oder einen neuen Aufruf hinzufügen. Damit scheinen alle
Zuständigkeiten gut voneinander getrennt zu sein. Hier sind die Schritte für
diese Wrap-Methode:

#. Identifiziert die Methode, die ihr ändern müsst
#. Wenn sich die Änderung als einzelne Abfolge von Anweisungen an einer Stelle
   formulieren lässt, benennt die Funktion um und erstellt anschließend eine
   neue Funktion mit demselben Namen und derselben Signatur wie die alte
   Methode. Achtet darauf, die Signaturen beizubehalten.
#. Fügt in der neuen Funktion einen Aufruf der alten Funktion ein.
#. Entwickelt eine neue Funktion, testet diese zuerst.

Als ein Nachteil könnte betrachtet werden, dass die neue Funktion nicht mit der
Logik der alten Funktion verflochten werden darf. Es muss sich um etwas handeln,
das entweder vor oder nach der alten Funktion ausgeführt wird. Eigentlich ist es
das gar nicht. Ein ernsthafter Nachteil ist jedoch, dass ein neuer Name für den
alten Code gefunden werden muss.

Eine weitere Form von Wrap-Methode, die wir verwenden können, wenn lediglich
eine neue Funktion hinzugefügt werden soll, kennt dieses Problem nicht. Die
Schritte sehen dann wie folgt aus:

#. Identifiziert eine Funktion, die ihr ändern müsst
#. Wenn sich die Änderung als einzelne Abfolge von Anweisungen an einer Stelle
   formulieren lässt, entwickelt dafür eine neue Funktion mithilfe der
   testgetriebenen Entwicklung
#. Erstellt eine weitere Funktion, die sowohl die neue als auch die alte
   Funktion aufruft.

Wrap hat gegenüber Sprout den Vorteil, dass der bestehende Umfang nicht
vergrößert wird. Auch ist die neue Funktionalität von der bestehenden
Funktionalität unabhängig und Code für einen Zweck wird nicht mit Code für einen
anderen Zweck verflochten.

Wenn wir Wrap auf Klassen anwenden, wird dies als *Decorator-Pattern*
bezeichnet. Wir erstellen Objekte einer Klasse, die eine andere Klasse umhüllt
und geben diese weiter. Die umhüllende Klasse sollte dieselbe Schnittstelle wie
die umhüllte Klasse haben, damit die Clients nicht erkennen, dass sie mit einem
Wrapper arbeiten. Mit dem Decorator-Pattern lassen sich komplexe
Verhaltensweisen durch die Zusammensetzung von Objekten zur Laufzeit aufbauen.

.. admonition:: Decorator-Pattern
   :collapsible: closed

   Decorator ist ein Strukturmuster, das eine flexible Alternative zur
   Unterklassenbildung ist, um eine Klasse um zusätzliche Funktionalitäten zu
   erweitern. Es sollte nicht verwechselt werden mit
   Python-:doc:`../../functions/decorators`.

   Im Python-Wiki findet ihr ein Beispiel für das `Decorator-Pattern
   <https://wiki.python.org/moin/DecoratorPattern>`_. Es zeigt uns, wie
   Dekoratoren in die Pipeline eingebaut werden, um dynamisch viele
   Verhaltensweisen in ein Objekt einzufügen.

Das Dekorator-Pattern ist zwar praktisch, sollte aber sparsam eingesetzt werden:

    *„Sich durch Code zu arbeiten, der Dekoratoren enthält, die wiederum andere
    Dekoratoren umschließen, ist fast so, als würde man die Schichten einer
    Zwiebel abziehen. Das ist zwar notwendig, bringt aber die Augen zum
    Tränen.“*

– Michael C. Feathers: `Working Effectively with Legacy Code
<https://www.pearson.de/working-effectively-with-legacy-code-9780131177055>`_

Wenn das neue Verhalten nur an wenigen Stellen zum Tragen kommen muss, kann es
daher nützlich sein, einen Wrapper zu erstellen, der nicht dem Decorator-Pattern
entspricht. Im Laufe der Zeit solltet ihr die Aufgaben des Wrappers im Auge
behalten und prüfen, ob er zu einem weiteren übergeordneten Konzept in eurem
System werden kann. Hier sind die Schritte für die Wrap-Klasse:

#. Identifiziert eine Methode, an der ihr die Änderung vornehmen müsst.
#. Wenn sich die Änderung als einzelne Abfolge von Anweisungen an einer Stelle
   formulieren lässt, erstellt eine Klasse, die die zu umhüllende Klasse als
   Konstruktor-Argument akzeptiert. Solltet ihr Schwierigkeiten haben, eine
   Klasse zu erstellen, die die ursprüngliche Klasse umhüllt, müsst ihr
   möglicherweise aus der umhüllten Klasse Funktionen oder Schnittstellen
   extrahieren, damit ihr euren Wrapper instanziieren könnt.
#. Erstellt mithilfe der testgetriebenen Entwicklung eine Methode in dieser
   Klasse, die die neue Aufgabe übernimmt. Schreibt eine weitere Methode, die
   die neue Methode und die alte Methode der umschlossenen Klasse aufruft.
#. Instanziiert die Wrapper-Klasse in eurem Code an der Stelle, an der ihr das
   neue Verhalten aktivieren möchtet.

.. _resolve-dependencies:

Abhängigkeiten auflösen
-----------------------

Systeme, die in kleine, aussagekräftig benannte und verständliche Teile
unterteilt sind, ermöglichen ein schnelleres Arbeiten. Viele Abhängigkeiten sind
problematisch, können aber glücklicherweise aufgehoben werden. Bei
objektorientiertem Code besteht der erste Schritt oft darin, die benötigten
Klassen in einem :term:`Test Fixture (Prüfvorrichtung)` zu instanziieren.

Wenn ihr eine Klasseninstanz habt, die in einem Test geändert werden soll, geht
das :abbr:`i. A. (im Allgemeinen)` sehr schnell. Wenn dafür jedoch auf externe
Ressourcen wie eine Datenbank, Hardware oder Kommunikationsinfrastruktur
zugegriffen werden muss, wird es schnell aufwändig. Dabei lohnt es sich, alles
zu überprüfen, was von dem abhängt, was ihr instanziieren wollt. Anschließend
solltet ihr Schnittstellen definieren und die Abhängigkeiten in neue Module
verschieben.

.. admonition:: Dependency-Inversion-Prinzip
   :collapsible: closed

   Das `Dependency-Inversion-Prinzip
   <https://de.wikipedia.org/wiki/Dependency-Inversion-Prinzip>`_ besagt, dass
   die Abhängigkeit eures Codes sehr viel geringer ist, wenn er von einer
   Schnittstelle abhängt. Schnittstellen ändern sich in der Regel weitaus
   seltener als der dahinterstehende Code. Ihr könnt dann Klassen bearbeiten
   oder die Schnittstelle implementieren ohne den dahinterliegenden Code ändern
   zu müssen. Aus diesem Grund ist es besser, sich auf Schnittstellen oder
   abstrakte Klassen zu stützen, als auf konkrete Klassen. Wenn ihr euch auf
   weniger veränderliche Dinge stützt, minimiert ihr die Wahrscheinlichkeit,
   dass bestimmte Änderungen eine umfangreiche Neubearbeitung auslösen.

Wir können Abhängigkeiten auflösen und Klassen auf verschiedene Module
verteilen, um unsere Tests schnell ausführen und Feedback erhalten zu können,
wodurch die Fehlerquote reduziert wird. Dies hat jedoch seinen Preis: Mehr
Schnittstellen und Module bringen konzeptionellen Overhead mit sich. Am Ende
erhält man Codeabschnitte, mit denen sich leichter arbeiten lässt. Es mag etwas
mühsamer sein, eine kleine Gruppe von Klassen separat zu testen, aber man sollte
nicht vergessen, dass sie nicht mitgetestet werden müssen, wenn eine andere
Gruppe von Klassen getestet wird.

Programming by Difference
~~~~~~~~~~~~~~~~~~~~~~~~~

Bei objektorientierter Programmierung haben wir die Möglichkeit, Vererbung zu
nutzen, um Funktionen einzuführen, ohne eine Klasse direkt ändern zu müssen.
Nachdem wir die Funktion hinzugefügt haben, können wir genau herausfinden, wie
wir sie tatsächlich integrieren möchten. Dieses *Programming by Difference* ist
nützlich um Änderungen schnell vorzunehmen. Einige Fallstricke, wie die
Verletzung der :doc:`solid` sollten jedoch vermieden werden.

.. _write-tests:

Tests schreiben
---------------

Es ist nicht immer leicht, eine Klasse in einem Test-Fixture zu instanziieren.
Hier sind die vier häufigsten Probleme, auf die wir stoßen:

* Objekte der Klasse lassen sich nicht ohne Weiteres erstellen.
* Das Test-Fixture lässt sich mit der Klasse nicht ohne Weiteres bauen.
* Der Konstruktor, den wir verwenden müssen, hat unerwünschte Nebenwirkungen.
* Im Konstruktor werden umfangreiche Berechnungen durchgeführt, und wir müssen
  diese erfassen.

Es gibt ein ganzes Arsenal von Techniken, um diese Probleme anzugehen:

Störende Konfigurationen, Umgebungsvariablen, Pfade und Parameter
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Um zu überprüfen, ob diese Angaben tatsächlich verwendet werden, können wir
:abbr:`ggf. (gegebenenfalls)` einfach :doc:`../../types/none` übergeben. Das
Schlimmste, was passieren kann, ist, dass ein Teil des Codes versucht, diesen
Parameter zu verwenden und eine :doc:`Exception <../../control-flow/exceptions>`
auslöst. Alternativ kann die Methode, die dies verarbeitet, mit
:ref:`/test/libs/pytest/builtin-fixtures.rst#monkeypatch` überschrieben werden.
Wir müssen dabei aber sicherstellen, dass wir dabei nicht das Verhalten
verändern, das wir testen wollen.

Versteckte Ressourcen
~~~~~~~~~~~~~~~~~~~~~

Häufig wird versteckt eine Ressource, :abbr:`z. B. (zum Beispiel)` ein
SMTP-Server genutzt, auf die wir in unserem Test-Fixture nicht problemlos
zugreifen können. *Parameterize Constructor* externalisiert eine solche
Ressource und übergeben sie dann als Parameter.

.. _refactoring:

Refactoring
-----------

Wie ein Refactoring aussehen könnte, wollen wir an zwei verschiedenen Beispielen
aufzeigen:

* **Abhängigkeiten von Bibliotheken:** Eine Bibliothek, die ein bestimmtes
  Problem für uns löst, spart uns oft viel Zeit in einem Projekt ein. Sie
  sollten jedoch nicht wahllos im gesamten Code eingesetzt werden, da ansonsten
  das Wechseln der Bibliothek einer Neuprogrammierung gleichkommen kann.

* **Refactoring von API-Aufrufen:** Im Wesentlichen gibt es zwei Ansätze für ein
  Refactoring von API-Aufrufen:

  * Bei *Skin and Wrap* der API erstellen wir Schnittstellen, die die API so
    genau wie möglich widerspiegeln, und dann erstellen wir Wrapper um die
    lassen der Bibliothek. Am Ende haben wir keine Abhängigkeiten vom
    zugrundeliegenden API-Code und die Wrapper können im Produktionscode an die
    echte API delegieren, während wir beim Testen :term:`Fakes <Fake>`
    verwenden.

    *Skin and Wrap* eignet sich gut, wenn

    * die API relativ klein ist
    * ihr Abhängigkeiten von einer Drittanbieter-Bibliothek vollständig
      auslagern möchtet
    * ihr über keine Tests verfügt und diese auch nicht schreiben könnt, da ihr
      die API nicht testen könnt. Wir haben dann die Möglichkeit, unseren
      gesamten Code zu testen – mit Ausnahme einer dünnen Delegationsschicht vom
      Wrapper zu den eigentlichen API-Klassen.

  * Bei *Responsibility-Based Extraction* identifizieren wir
    Verantwortlichkeiten im Code und beginnen damit, entsprechende Methoden zu
    extrahieren.

    *Responsibility-Based Extraction* eignet sich besser, wenn

    * die API komplexer ist
    * ihr bereits über ein Tool mit Extraktionsmethoden verfügt
