Datentypen als Objekte
======================

Inzwischen habt ihr die grundlegenden Python-:doc:`../types/index`
kennengelernt und wisst, wie ihr mit Hilfe von :doc:`classes` eure eigenen
Datentypen erstellen könnt. Beachtet dabei, dass Python dynamisch typisiert ist,
:abbr:`d.h.(das heißt)`, die Typen werden zur Laufzeit bestimmt, nicht beim
Kompilieren. Dies ist einer der Gründe, warum Python so einfach zu benutzen ist.
Ihr könnt einfach folgendes ausprobieren:

.. code-block:: pycon

   >>> type(3)
   <class 'int'>
   >>> type("Hello")
   <class 'str'>
   >>> type(["Hello", "Pythonistas"])
   <class 'list'>

In diesen Beispielen seht ihr die eingebaute :class:`type`-Funktion in Python.
Sie kann auf jedes Python-Objekt angewendet werden und gibt den Typ des Objekts
zurück. In diesem Beispiel sagt euch die Funktion, dass ``3`` ein ``int``
(Integer) ist, dass ``'Hello'`` ein ``str`` (String) und dass ``['Hello', 'Pythonistas']`` eine ``list`` (Liste) ist.

Von größerem Interesse dürfte jedoch die Tatsache sein, dass Python als Antwort
auf die Aufrufe von :class:`type` Objekte zurückgibt; ``<class 'int'>``,
``<class 'str'>`` und ``<class 'list'>`` sind die Bildschirmdarstellungen der
zurückgegebenen Objekte. Ihr könnt diese Python-Objekte also miteinander
vergleichen:

.. code-block:: pycon

   >>> type("Hello") == type("Pythonistas!")
   True
   >>> type("Hello") == type("Pythonistas!") == type(["Hello", "Pythonistas"])
   False

Mit dieser Technik könnt ihr :abbr:`u.a. (unter anderem)` eine Typ-Überprüfung
in euren Funktions- und Methodendefinitionen durchführen. Die häufigste Frage
zu den Typen von Objekten ist jedoch, ob ein bestimmtes Objekt eine Instanz
einer Klasse ist. Ein Beispiel mit einer einfachen Vererbungshierarchie macht
dies klarer:

#. Zunächst definieren wir zwei Klassen mit einer Vererbungshierarchie:

   .. code-block:: pycon

      >>> class Form:
      ...     pass
      ...
      >>> class Square(Form):
      ...     pass
      ...
      >>> class Circle(Form):
      ...     pass
      ...

#. Nun könnt ihr eine Instanz ``c1`` der Klasse ``Circle`` erstellen:

   .. code-block:: pycon

      >>> c1 = Circle()

#. Wie erwartet, gibt die type-Funktion auf ``c1`` aus, dass ``c1`` eine Instanz
   der Klasse ``Circle`` ist, die in Ihrem aktuellen ``__main__`` Namespace
   definiert ist:

   .. code-block:: pycon

      >>> type(c1)
      <class '__main__.Circle'>

#. Ihr könnt genau dieselben Informationen auch durch Zugriff auf das
   ``__class__``-Attribut der Instanz erhalten:

   .. code-block:: pycon

      >>> c1.__class__
      <class '__main__.Circle'>

#. Ihr könnt auch explizit überprüfen, ob die beiden Klassenobjekte identisch
   sind:

   .. code-block:: pycon

      >>> c1.__class__ == Circle
      True

#. Zwei eingebaute Funktionen bieten jedoch benutzerfreundlichere Möglichkeit,
   die meisten der normalerweise benötigten Informationen zu erhalten:

   :func:`python3:isinstance`
        stellt fest, ob :abbr:`z.B. (zum Beispiel)` eine Klasse, die an eine
        Funktion oder Methode übergeben wird, vom erwarteten Typ ist.
   :func:`python3:issubclass`
        stellt fest, ob eine Klasse die Unterklasse einer anderen ist.

   .. code-block:: pycon

      >>> issubclass(Circle, Form)
      True
      >>> issubclass(Square, Form)
      True
      >>> isinstance(c1, Form)
      True
      >>> isinstance(c1, Square)
      False
      >>> isinstance(c1, Circle)
      True
      >>> issubclass(c1.__class__, Form)
      True
      >>> issubclass(c1.__class__, Square)
      False
      >>> issubclass(c1.__class__, Circle)
      True

.. _duck-typing:

Duck-Typing
-----------

Die Verwendung von :class:`python3:type`, :func:`python3:isinstance` und
:func:`python3:issubclass` macht es ziemlich einfach, die Vererbungshierarchie
eines Objekts oder einer Klasse korrekt zu bestimmen. Python hat jedoch auch
eine Funktion, die die Verwendung von Objekten noch einfacher macht:
Duck-Typing:

    „Wenn es wie eine Ente aussieht und wie eine Ente quakt, muss es eine
    Ente sein.“

Dies bezieht sich auf Pythons Art und Weise zu bestimmen, ob ein Objekt der
erforderliche Typ für eine Operation ist, wobei der Schwerpunkt auf der
Schnittstelle eines Objekts liegt. Kurz gesagt müsst ihr euch in Python nicht um
die Typ-Überprüfung von Funktions- oder Methodenargumenten und Ähnlichem
kümmern, sondern euch stattdessen auf lesbaren und dokumentierten Code in
Verbindung mit Tests verlassen, um sicherzustellen, dass ein Objekt bei Bedarf
*„wie eine Ente quakt.“*

Duck-Typing kann die Flexibilität von gut geschriebenem Code erhöhen und gibt
euch in Kombination mit fortgeschrittenen objektorientierten Funktionen die
Möglichkeit, Klassen und Objekte zu erstellen, die fast jede Situation abdecken.
Solche :ref:`speziellen Methoden <python3:specialnames>` sind Attribute einer
Klasse mit besonderer Bedeutung für Python. Sie sind zwar als Methoden
definiert, aber nicht dazu gedacht, sie direkt aufzurufen; stattdessen werden
sie von Python automatisch als Reaktion auf eine Anforderung an ein Objekt
dieser Klasse aufgerufen.

Eines der einfachsten Beispiele für eine spezielle Methode ist
:meth:`object.__str__`. Wenn es in einer Klasse definiert ist, wird das
``__str__``-Methoden-Attribut jedes Mal aufgerufen, wenn eine Instanz dieser
Klasse verwendet wird und Python eine benutzerlesbare Zeichenketten-Darstellung
dieser Instanz benötigt. Um dieses Attribut in Aktion zu sehen, verwenden wir
erneut unsere ``Form``-Klasse mit der Standardmethode ``__init__`` um Instanzen
der Klasse zu initialisieren, sondern auch eine ``__str__``-Methode um
Zeichenketten zurückzugeben, die Instanzen in einem lesbaren Format darstellen:

.. code-block:: pycon

   >>> class Form:
   ...     def __init__(self, x, y):
   ...         self.x = x
   ...         self.y = y
   ...     def __str__(self):
   ...         return "Position: x={0}, y={1}".format(self.x, self.y)
   ...
   >>> f = Form(2, 3)
   >>> print(f)
   Position: x=2, y=3

Auch wenn unser spezielles ``__str__``-Methoden-Attribut nicht von unserem Code
explizit aufgerufen wurde, konnte es dennoch von Python verwendet werden, da
Python weiß, dass das ``__str__``-Attribut, falls vorhanden, eine Methode zur
Umwandlung von Objekten in benutzerlesbare Zeichenketten definiert. Und genau
dies zeichnet die speziellen Methoden-Attribute aus. So ist es :abbr:`z.B. (zum
Beispiel)` oft eine gute Idee, das ``__str__``-Attribut für eine Klasse zu
definieren, damit ihr im Debugging-Code ``print(instance)`` aufrufen könnt und
eine informative Aussage über euer Objekt zu erhalten.

Umgekehrt kann es jedoch auch verwundern, dass ein Objekttyp anders auf
spezielle Methoden-Attribute reagiert. Daher verwende ich spezielle
Methoden-Attribute meist nur in einer der folgenden beiden Fälle:

* in einer häufig verwendeten Klasse, meist für Sequenzen, die sich ähnlich wie
  ein in Python eingebauter Typ verhält, und die durch spezielle
  Methoden-Attribute nützlicher wird.
* in einer Klasse, die sich fast identisch zu einer eingebauten Klasse verhält,
  :abbr:`z.B. (zum Beispiel)` Listen, die als balancierte Bäume implementiert
  sind, um das Einfügen zu beschleunigen, kann ich die speziellen
  Methoden-Attribute definieren.

Checks
------

* Was wäre der Unterschied zwischen der Verwendung von :func:`type` und
  :func:`isinstance` in :ref:`Check: Listen <check-list>`?
