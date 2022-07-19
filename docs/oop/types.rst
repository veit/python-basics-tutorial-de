Datentypen als Objekte
======================

Inzwischen habn ihr die grundlegenden Python-:doc:`../types/index`
kennengelernt und wisst, wie ihr mit Hilfe von :doc:`classes` eure eigenen
Datentypen erstellen könnt. Beachtet dabei, dass Python dynamisch typisiert ist,
:abbr:`d.h.(das heißt)`, die Typen werden zur Laufzeit bestimmt, nicht zur
Kompilierzeit. Dies ist einer der Gründe, warum Python so einfach zu benutzen
ist. Ihr könnt einfach folgendes ausprobieren:

.. code-block:: python

    >>> type(3)
    <class 'int'>
    >>> type('Hello')
    <class 'str'>
    >>> type(['Hello', 'Pythonistas'])
    <class 'list'>

In diesen Beispielen seht ihr die eingebaute :class:`type`-Funktion in Python.
Sie kann auf jedes Python-Objekt angewendet werden und gibt den Typ des Objekts
zurück. In diesem Beispiel sagt euch die Funktion, dass ``3`` ein ``int``
(Integer) ist, dass ``'Hello'`` ein ``str`` (String) und dass ``['Hello', 'Pythonistas']`` eine ``list`` (Liste) ist.

Von größerem Interesse dürfte jedoch die Tatsache sein, dass Python als Antwort
auf die Aufrufe von :class:`type` Objekte zurückgibt; ``<class 'int'>``,
``<class 'str'>`` und ``<class 'list'>`` sind die Bildschirmdarstellungen der
zurückgegebenen Objekte. Ihr könnt diese Python-Pbjekte also miteinander
vergleichen:

.. code-block:: python

    >>> type('Hello') == type('Pythonistas!')
    True
    >>> type('Hello') == type('Pythonistas!') == type(['Hello', 'Pythonistas'])
    False

Mit dieser Technik könnt ihr :abbr:`u.a. (unter anderem)` eine Typüberprüfung
in euren Funktions- und Methodendefinitionen durchführen. Die häufigste Frage
zu den Typen von Objekten ist jedoch, ob ein bestimmtes Objekt eine Instanz
einer Klasse ist. Ein Beispiel mit einer einfachen Vererbungshierarchie macht
dies klarer:

#. Zunächst definieren wir zwei Klassen mit einer Vererbungshierarchie:

    .. code-block:: python

        >>> class Form:
        ...     pass
        ...
        >>> class Square(Form):
        ...     pass
        ...
        >>> class Circle(Form):
        ...     pass

#. Nun könnt ihr eine Instanz ``c1`` der Klasse ``Circle`` erstellen:

    .. code-block:: python

        >>> c1 = Circle()

#. Wie erwartet, gibt die type-Funktion auf ``c1`` aus, dass ``c1`` eine Instanz
   der Klasse ``Circle`` ist, die in Ihrem aktuellen ``__main__`` Namespace
   definiert ist:

    .. code-block:: python

        >>> type(c1)
        <class '__main__.Circle'>

#. Ihr könnt genau dieselben Informationen auch durch Zugriff auf das
   ``__class__``-Attribut der Instanz erhalten:

    .. code-block:: python

        >>> c1.__class__
        <class '__main__.Circle'>

#. Ihr könnt auch explizit überprüfen, ob die beiden Klassenobjekte identisch
   sind:

    .. code-block:: python

        >>> c1.__class__ == Circle
        True

#. Zwei eingebaute Funktionen bieten jedoch benutzerfreundlichere Möglichkeit,
   die meisten der normalerweise benötigten Informationen zu erhalten:

   :func:`python3:isinstance`
        stellt fest, ob :abbr:`z.B. (zum Beispiel)` eine Klasse, die an eine
        Funktion oder Methode übergeben wird, vom erwarteten Typ ist.
   :func:`python3:issubclass`
        stellt fest, ob eine Klasse die Unterklasse einer anderen ist.

    .. code-block:: python

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
