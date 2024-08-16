Klassen
=======

Eine :doc:`Klasse in Python <python3:tutorial/classes>` ist eigentlich ein
Datentyp. Alle in Python eingebauten Datentypen sind Klassen, und Python stellt
euch leistungsfähige Werkzeuge bereit, um jeden Aspekt des Verhaltens einer
Klasse zu manipulieren. Ihr könnt eine Klasse mit der ``class``-Anweisung
definieren:

.. code-block:: pycon

    >>> class MyClass:
    ...     STATEMENTS
    ...

``MyClass``
    Klassenbezeichner werden üblicherweise in Großbuchstaben geschrieben,
    :abbr:`d.h. (das heißt)` der erste Buchstabe jedes Wortes wird
    großgeschrieben, um die Bezeichner hervorzuheben.
``STATEMENTS``
    ist eine Liste von Python-Anweisungen – in der Regel Variablenzuweisungen
    und Funktionsdefinitionen. Es sind jedoch keine Zuweisungen oder
    Funktionsdefinitionen erforderlich, es kann auch nur eine einzige
    ``pass``-Anweisung sein.

Nachdem ihr die Klasse definiert habt, könnt ihr ein neues Objekt des
Klassentyps (eine Instanz der Klasse) erstellen, indem ihr den Klassennamen als
Funktion aufruft:

.. code-block:: pycon

    >>> class Square:
    ...     length = 1
    ...
    >>> my_square = Square()

Klasseninstanzen können als Strukturen oder Datensätze verwendet werden. Im
Gegensatz zu C-Strukturen oder Java-Klassen müssen die Datenfelder einer Instanz
jedoch nicht im Voraus deklariert werden. Das folgende kurze Beispiel definiert
eine Klasse namens ``Square``, erstellt eine ``Square``-Instanz, weist der
Kantenlänge einen Wert zu und verwendet diesen Wert dann zur Berechnung des
Umfangs:

.. code-block:: pycon

    >>> my_square.length = 3
    >>> print(f"Der Umfang des Quadrats ist {4 * my_square.length}.")
    Der Umfang des Quadrats ist 12.

Zeile 1
    Wie in Java und vielen anderen Sprachen werden die Felder einer Instanz mit
    Hilfe der Punktnotation angesprochen.

Ihr könnt Felder einer Instanz automatisch initialisieren, indem ihr eine
``__init__``-Initialisierungsmethode in die Klasse aufnehmt. Diese Funktion wird
jedes Mal ausgeführt, wenn eine Instanz der Klasse mit dieser neuen Instanz als
erstes Argument ``self`` erstellt wird. Anders als in Java und C++ können
Python-Klassen auch nur eine ``__init__``-Methode haben. Im folgenden Beispiel
werden standardmäßig Quadrate mit einer Kantenlänge von ``1`` erzeugt:

.. code-block:: pycon
    :linenos:

    >>> class Square:
    ...     def __init__(self):
    ...         self.length = 1
    ...
    >>> my_square = Square()
    >>> print(f"Der Umfang des Quadrats ist {4 * my_square.length}.")
    Der Umfang des Quadrats ist 4.

Zeile 2
    Der Konvention nach ist ``self`` immer der Name des ersten Arguments von
    ``__init__``. ``self`` wird auf die neu erstellte ``Square``-Instanz
    gesetzt, wenn ``__init__`` ausgeführt wird.
Zeile 5
    Als nächstes erstellt ihr ein ``Square``-Instanzobjekt.
Zeile 6
    Diese Zeile nutzt die Tatsache, dass das ``length``-Feld bereits
    initialisiert ist.

Ihr könnt das ``length``-Feld auch überschreiben, so dass die letzte Zeile ein
anderes Ergebnis ausgibt als die vorherige ``print``-Anweisung:

    .. code-block:: pycon

        >>> my_square.length = 3
        >>> print(f"Der Umfang des Quadrats ist {4 * my_square.length}.")
        Der Umfang des Quadrats ist 12.

Checks
------

# Schreibt eine :class:`Triangle`-Klasse, die auch die Fläche berechnen kann.
