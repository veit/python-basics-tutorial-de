SOLID-Prinzipien
================

`SOLID
<https://de.wikipedia.org/wiki/Prinzipien_objektorientierten_Designs#SOLID-Prinzipien>`_
ist ein Akronym für die ersten fünf Prinzipien des objektorientierten Designs
(OOD) von Robert C. Martin (auch bekannt als `Uncle Bob
<https://de.wikipedia.org/wiki/Robert_Cecil_Martin>`_).

Diese Prinzipien legen Praktiken für die Entwicklung von Software mit
Überlegungen zur Wartung und Erweiterbarkeit fest, wenn das Projekt wächst. Die
Übernahme dieser Prinzipien kann auch dazu beitragen, Code Smells zu vermeiden,
Code zu refaktorisieren und agile oder adaptive Software zu entwickeln.

SOLID steht für:

S – :ref:`single-responsibility`
    Die Methoden einer Klasse sollten auf einen einzigen Zweck ausgerichtet
    sein.
O – :ref:`open-closed`
    Objekte sollten offen für Erweiterungen, aber geschlossen für Änderungen
    sein.
L – :ref:`liskov-substitution`
    Unterklassen sollten durch ihre Oberklassen substituierbar sein.
I – :ref:`interface-segregation`
    Objekte sollten nicht von Methoden abzuhängen, die sie nicht verwenden.
D – :ref:`dependency-inversion`
    Abstraktionen sollten nicht von Details abhängen.

.. _single-responsibility:

Single-Responsibility-Prinzip
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Das `Single-Responsibility-Prinzip
<https://de.wikipedia.org/wiki/Single-Responsibility-Prinzip>`_ besagt, dass
jede Klasse nur eine Aufgabe erfüllen soll:

    Es sollte nie mehr als einen Grund geben, eine Klasse zu ändern.

– `Robert C. Martin: SRP: The Single Responsibility Principle
<https://web.archive.org/web/20140407020253/http://www.objectmentor.com/resources/articles/srp.pdf>`_

Nehmen wir :abbr:`z.B. (zum Beispiel)` eine Anwendung, die eine Sammlung von
Formen – Kreise und Quadrate – nimmt und die Summe der Umfänge aller Formen  der
Sammlung berechnet.

Erstellt zunächst die :class:`Form`-Klassen mit den notwendigen Parametern. Für
Quadrate ist dies die Kantenlänge und für Kreise der Durchmesser:

.. literalinclude:: forms.py
   :language: python
   :lines: 4, 6-8, 13-18, 22-24, 27-29

Nun könnt ihr eine Klasse :class:`SquaresAndCircles` erstellen mit der Logik zur
Berechnung aller Umfänge von Quadraten und Kreisen:

.. literalinclude:: forms.py
   :language: python
   :lines: 1-3, 35-46

Die Klasse :class:`SquaresAndCircles` übernimmt die Logik, die zur Berechnung
aller Umfänge von Quadraten und Kreisen erforderlich ist. Damit ist das Prinzip
der Einzelverantwortung erfüllt.

.. _open-closed:

Open-Closed-Prinzip
-------------------

Das :abbr:`OCP (Open-Closed-Prinzip)` besagt:

    Objekte oder Entitäten sollten offen für Erweiterungen, aber geschlossen für
    Änderungen sein.

Das bedeutet, dass eine Klasse erweiterbar sein sollte, ohne die Klasse selbst
zu verändern.

Schauen wir uns die Klasse :class:`SquaresAndCircles` an und konzentrieren uns
auf die :func:`circumferences`-Methode. Stellt euch ein Szenario vor, in dem die
Summe zusätzlicher Formen wie Dreiecke, Fünfecke, Sechsecke :abbr:`usw. (und so
weiter)` berechnet werden sollen. Ihr müsstet diese Klasse ständig bearbeiten
und weitere ``if``-Blöcke hinzufügen. Das würde gegen das Open-Closed-Prinzip
verstoßen. Eine Möglichkeit, diese Methode zu verbessern, besteht darin, die
Logik zur Berechnung des Umfangs jeder Form aus der Klasse
:class:`SquaresAndCircles` zu entfernen und sie an die Klassen der speziellen
Formen anzuhängen. Hier sind die Umfangsberechnungen in den Klassen
:class:`Square` und :class:`Circle` definiert:

.. literalinclude:: forms.py
   :language: python
   :lines: 15-32

Die Summenmethode :func:`circumferences` in der Klasse
:class:`CircumferenceFormInstances` kann dann wie folgt umgeschrieben werden:

.. literalinclude:: forms.py
   :language: python
   :lines: 49-

Damit ist das Open-Closed-Prinzip erfüllt.

.. tip::
   Wenn euer Code noch nicht *offen* für neue Anforderungen ist, solltet ihr
   zunächst den vorhandenen Code so umordnen (refaktorieren), dass er für die
   neue Funktion offen ist. Erst dann solltet ihr neuen Code hinzufügen.

       Unter Refaktorierung versteht man den Prozess, ein Softwaresystem so zu
       verändern, dass das äußere Verhalten des Codes nicht verändert, aber
       seine innere Struktur verbessert wird.

   – `Martin Fowler: Refactoring
   <https://www.mitp.de/IT-WEB/Software-Entwicklung/Refactoring.html>`_

.. note::
   Sicheres Refactoring ist auf :doc:`Tests </test/index>` angewiesen. Wenn ihr
   den Code wirklich umgestaltet, ohne das Verhalten zu ändern, sollten die
   vorhandenen Tests bei jedem Schritt weiterhin erfolgreich sein. Die Tests
   sind ein Sicherheitsnetz, das das Vertrauen in die neue Anordnung des Codes
   rechtfertigt. Wenn sie versagen,

   * habt ihr den Code versehentlich beschädigt,
   * oder die vorhandenen Tests sind fehlerhaft.

.. _liskov-substitution:

Liskovsches Substitutionsprinzip
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Das `Liskovsche Substitutionsprinzip
<https://de.wikipedia.org/wiki/Liskovsches_Substitutionsprinzip>`_ besagt, dass
ein Programm, das Objekte der Basisklasse verwendet, auch mit Objekten der
Unterklasse korrekt funktionieren muss.

Erweitern wir die Klasse :class:`Form`, so dass die daraus abgeleiteten Klassen
in der x- und y-Richtung verschoben werden können:

.. literalinclude:: forms.py
   :language: python
   :lines: 4-12
   :emphasize-lines: 7-9

Anschließend könnt ihr sowohl Quadrate wie auch Kreise auf der x- und y-Achse
verschieben:

.. code-block:: pycon

   >>> import forms
   >>> s1 = forms.Square()
   >>> c1 = forms.Circle()
   >>> s1.x, s1.y, c1.x, c1.y
   (0, 0, 0, 0)
   >>> s1.move(4, 5)
   >>> c1.move(2, 3)
   >>> s1.x, s1.y, c1.x, c1.y
   (4, 5, 2, 3)

.. note::
   Das Liskovsche Substitutionsprinzip gilt auch für :ref:`duck-typing`: jedes
   Objekt, das behauptet, eine Ente zu sein, muss die API der Ente vollständig
   implementieren. Duck-Types sollten gegeneinander austauschbar sein. Die Logik
   über verschiedene Datentypen von Objekten hinweg anzuwenden, nennt sich
   `Polymorphie <https://de.wikipedia.org/wiki/Polymorphie_(Programmierung)>`_.

.. _interface-segregation:

Interface-Segregation-Prinzip
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Das `Interface-Segregation-Prinzip
<https://de.wikipedia.org/wiki/Interface-Segregation-Prinzip>`_ wendet das
:ref:`single-responsibility` auf Schnittstellen (:abbr:`engl. (englisch)`:
Interfaces) an um ein bestimmtes Verhalten zu isolieren. Wenn eine Änderung an
einem Teil eures Codes erforderlich ist, eröffnet die Extraktion eines Objekts, das eine Rolle spielt, die Möglichkeit, das neue Verhalten zu unterstützen, ohne
dass der bestehende Code geändert werden muss. Dies ist kodierten
Konkretisierungen vorzuziehen.

So haben wir im vorigen Beispiel überprüft, ob unser :obj:`Form`-Objekt auch
tatsächlich eine :func:`circumference`-Methode bereitstellt. Dies ist notwendig,
wenn später Formen wie :class:`Point` oder :class:`Line` hinzukommen sollten,
die keinen Umfang aufweisen.

.. note::
   In diesem Zusammenhang ist auch das `Gesetz von Demeter
   <https://de.wikipedia.org/wiki/Gesetz_von_Demeter>`_ interessant, das besagt,
   dass Objekte nur mit Objekten in ihrer unmittelbaren Umgebung kommunizieren
   sollen. Damit wird die Liste der anderen Objekte wirksam eingeschränkt, an
   die ein Objekt eine Nachricht senden kann und die Kopplung zwischen Objekten
   verringert: ein Objekt kann nur mit seinen Nachbarn sprechen, nicht aber mit
   den Nachbarn seiner Nachbarn; Objekte können nur Nachrichten an direkt
   Beteiligte senden.

.. _dependency-inversion:

Dependency-Inversion-Prinzip
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Das `Dependency-Inversion-Prinzip
<https://de.wikipedia.org/wiki/Dependency-Inversion-Prinzip>`_ kann definiert
werden als

    Abstraktionen sollten nicht von Details abhängen. Details sollten von
    Abstraktionen abhängen.

– `Robert C. Martin: The Dependency Inversion Principle
<https://www.cs.utexas.edu/~downing/papers/DIP-1996.pdf>`_

:func:`circumferences` sollte nicht bereits in der :class:`Form`-Klasse
definiert werden, da es auch Formen ohne Umfang gibt.
