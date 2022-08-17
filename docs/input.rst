Input
=====

Ihr könnt die Funktion :func:`python3:input` verwenden, um Dateneingaben zu
erhalten. Verwendet den Prompt-String, den ihr anzeigen möchtet, als Parameter
für ``input``:

.. code-block:: python

    >>> first_name = input("Vorname? ")
    Vorname? Veit
    >>> surname = input("Nachname? ")
    Nachname? Schiele
    >>> print(first_name, surname)
    Veit Schiele

Dies ist ein recht einfacher Weg, um Dateneingaben zu erhalten. Der einzige
Haken ist, dass die Eingabe als Zeichenkette eingeht. Wenn ihr also eine Zahl
verwenden wollt, müsst ihr sie mit der Funktion :class:`python3:int` oder
:class:`python3:float` umwandeln, :abbr:`z.B. (zum Beispiel)` für die Berechnung
des Alters aus dem Geburtsjahr:

.. code-block:: python

    >>> import datetime
    >>> currentDateTime = datetime.datetime.now()
    >>> year = currentDateTime.year
    >>> year_birth = input("Geburtsjahr? ")
    Geburtsjahr? 1964
    >>> age = year - int(year_birth)
    >>> print('Alter:', age, 'Jahre')
    Alter: 58 Jahre
