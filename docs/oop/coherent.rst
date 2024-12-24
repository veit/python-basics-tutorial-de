Zusammenhängendes Beispiel
==========================

Die bisher angesprochenen Punkte, sind die Grundlagen der Verwendung von Klassen
und Objekten in Python. Diese Grundlagen werde ich nun in einem
zusammenhängenden Beispiel dargestellt: :download:`form.py`.

#. Zunächst erstellen wir eine Basisklasse:

   .. literalinclude:: form.py
      :language: python
      :linenos:
      :lines: 1-12
      :lineno-start: 1

   Zeile 7
       Die ``__init__``-Methode benötigt eine Instanz (``self``) und zwei
       Parameter
   Zeilen 8 und 9
       Auf die beiden Instanz-Variablen ``x`` und ``y``, auf die über ``self``
       zugegriffen wird.
   Zeile 10
       Die ``move``-Methode benötigt eine Instanz (``self``) und zwei
       Parameter.
   Zeilen 11 und 12
       Instanz-Variablen, die in der ``move``-Methode gesetzt werden.

#. Als nächstes erstellt eine Unterklasse, die von der Basisklasse ``Form``
   erbt:

   .. literalinclude:: form.py
      :language: python
      :linenos:
      :lines: 16-21
      :lineno-start: 16

   Zeile 16
       Die Klasse ``Square`` erbt von der Klasse ``Form``.
   Zeile 19
       ``Square``’s ``__init__`` nimmt eine Instanz (``self``) und drei
       Parameter, alle mit Voreinstellungen.
   Zeile 20
       ``__init__`` von Square verwendet ``super()``, um ``__init__`` von
       ``Form`` aufzurufen.

#. Schließlich erstellen wir eine weitere Unterklasse, die zudem eine statische
   Methode enthält:

   .. literalinclude:: form.py
      :language: python
      :linenos:
      :lines: 27-43
      :lineno-start: 27

   Zeilen 29 und 30
       ``pi`` und ``circles`` sind Klassenvariablen für ``Circle``.
   Zeile 34
       In der ``__init__``-Methode fügt sich die Instanz in die Liste
       ``circles`` ein.
   Zeilen 37 und 38
       ``circumferences`` ist eine Klassenmethode und nimmt die Klasse selbst
       (``cls``) als Parameter.
   Zeile 41
       verwendet den Parameter ``cls`` für den Zugriff auf die Klassenvariable
       ``circles``.

Jetzt könnt ihr einige Instanzen der Klasse ``Circle`` erstellen und sie
analysieren. Da die ``__init__``-Methode von ``Circle`` Standardparameter hat,
könnt ihr einen Kreis erstellen, ohne irgendwelche Parameter anzugeben:

.. code-block:: pycon

   >>> import form
   >>> c1 = form.Circle()
   >>> c1.diameter, c1.x, c1.y
   (1, 0, 0)

Wenn ihr Parameter angebt, werden diese verwendet, um die Werte der Instanz
festzulegen:

.. code-block:: pycon

   >>> c2 = form.Circle(2, 3, 4)
   >>> c2.diameter, c2.x, c2.y
   (2, 3, 4)

Wenn ihr die ``move()``-Methode aufruft, findet Python keine ``move()``-Methode
in der Klasse ``Circle``, also wird in der Vererbungshierarchie nach oben
gegangen und die ``move()``-Methode von ``Form`` verwendet:

.. code-block:: pycon

   >>> c2.move(5, 6)
   >>> c2.diameter, c2.x, c2.y
   (2, 8, 10)

Ihr könnt auch die Klassenmethode ``circumferences()`` der Klasse ``Circle``
aufrufen, entweder über die Klasse selbst oder durch eine Instanz:

.. code-block:: pycon

   >>> form.Circle.circumferences()
   9.424769999999999
   >>> c2.circumferences()
   9.424769999999999
