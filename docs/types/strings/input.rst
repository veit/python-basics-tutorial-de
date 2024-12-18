``input()``
===========

Ihr könnt die Funktion :func:`python3:input` verwenden, um Dateneingaben zu
erhalten. Verwendet den Prompt-String, den ihr anzeigen möchtet, als Parameter
für ``input``:

.. code-block:: pycon

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

.. code-block:: pycon

    >>> import datetime
    >>> current = datetime.datetime.now()
    >>> year = current.year
    >>> year_birth = input("Geburtsjahr? ")
    Geburtsjahr? 1964
    >>> age = year - int(year_birth)
    >>> print("Alter:", age, "Jahre")
    Alter: 58 Jahre

Checks
------

* Wie könnt ihr mit der :func:`input`-Funktion String- und Integer-Werte
  erhalten?

* Wie wirkt es sich aus, wenn ihr :func:`int` nicht verwendet um den Aufruf von
  :func:`input` für Integer-Eingaben zu verwenden?

* Könnt ihr den Code so abändern, dass er eine Fließkommazahl akzeptiert?

* Was passiert, wenn ihr einen *falschen* Werttyp eingebt?

* Schreibt den Code, um für drei User jeweils nach Namen und Alter zu fragen.
  Nachdem die Werte eingegeben wurden, fragt nach einem der Namen und gebt das
  zugehörige Alter aus.
