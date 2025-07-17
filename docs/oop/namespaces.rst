Namensräume
===========

Wenn ihr euch in der Methode einer Klasse befindet, habt ihr direkten Zugriff

#. auf den **lokalen Namensraum** mit den Parametern und Variablen, die in
   dieser Methode deklariert sind,
#. den **globalen Namensraum** mit Funktionen und Variablen, die auf Modulebene
   deklariert sind und
#. den **eingebauten Namensraum** mit den eingebauten Funktionen und eingebauten
   Exceptions.

Diese drei Namensräume werden in dieser Reihenfolge durchsucht.

Um die verschiedenen Namensräume in unserem Beispiel näher zu erläutern, haben
wir unser bestehendes Modul so erweitert, dass deutlich wird, worauf innerhalb
einer Methode zugegriffen werden kann: :download:`form_ns.py`.

Einen Überblick über die Methoden, die in einem Namensraum verfügbar sind,
erhaltet ihr mit

.. literalinclude:: form_ns.py
    :language: python
    :linenos:
    :lines: 65-70
    :lineno-start: 65

.. code-block:: pycon

    >>> import form_ns
    >>> c1 = form_ns.Circle()
    >>> c1.namespaces()
    Global namespace: ['__name__', '__doc__', '__package__', '__loader__', '__spec__', '__file__', '__cached__', '__builtins__', 'Form', 'Square', 'Circle']
    Superclass namespace: ['__class__', '__delattr__', '__dict__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__le__', '__lt__', '__module__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '__weakref__', 'move']
    Class namespace: ['__class__', '__delattr__', '__dict__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__le__', '__lt__', '__module__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '__weakref__', 'circles', 'circumference', 'circumferences', 'diameter', 'instance_variables', 'move', 'namespaces', 'pi']
    Instance namespace: ['_Circle__diameter', '__class__', '__delattr__', '__dict__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__le__', '__lt__', '__module__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '__weakref__', 'circles', 'circumference', 'circumferences', 'diameter', 'instance_variables', 'move', 'namespaces', 'pi', 'x', 'y']
    Local namespace: ['self']

Über die ``self``-Variable habt ihr auch Zugriff auf

#. den **Namensraum der Instanz** mit

   * Instanz-Variablen
   * privaten Instanz-Variablen und
   * Instanz-Variablen der Superklasse,


#. den **Namensraum der Klasse** mit

   * Methoden,
   * Klassenvariablen,
   * privaten Methoden und
   * privaten Klassenvariablen und

#. den **Namensraum der Superklasse** mit

   * Methoden der Superklasse und
   * Klassenvariablen der Superklasse.

Diese drei Namensräume werden ebenfalls in dieser Reihenfolge durchsucht.

Den Namensraum der Instanz könnt ihr nun :abbr:`z.B. (zum Beispiel)` analysieren
mit der Methode ``instance_variables``:

.. literalinclude:: form_ns.py
    :language: python
    :linenos:
    :lines: 72-
    :lineno-start: 72

.. code-block:: pycon

    >>> import form_ns
    >>> c1 = form_ns.Circle()
    >>> c1.instance_variables()
    Instance variables self.__diameter, self.x, self.y: 1 0 0

.. note::

    Während ihr auf die Methode ``move`` der Superklasse ``form`` mit ``self``
    zugreifen könnt, sind jedoch private Instanz-Variablen, private Methoden und
    private Klassenvariablen der Superklasse so nicht zugänglich.

Wenn ihr nur Instanzen einer bestimmten Klasse ändern wollt, könnt ihr dies
:abbr:`z.B. (zum Beispiel)` mit dem :mod:`Garbage Collector <gc>`:

.. code-block:: pycon

    >>> import forms
    >>> c1 = forms.Circle()
    >>> c2 = forms.Circle(2, 3, 4)
    >>> s1 = forms.Square(5, 6, 7)
    >>> import gc
    >>> for obj in gc.get_objects():
    ...     if isinstance(obj, forms.Circle):
    ...         obj.move(3, 0)
    ...
    >>> c1.x, c1.y
    (3, 0)
    >>> c2.x, c2.y
    (6, 4)
    >>> s1.x, s1.y
    (6, 7)
