``dataclasses``
===============

:doc:`dataclasses <python3:library/dataclasses>` wurden in Python 3.7 eingeführt und sind
ein spezieller Shortcut, mit der wir Klassen erstellen können, die Daten speichern. Python
bietet einen speziellen :doc:`Dekorator <../functions/decorators>`, wenn wir eine solche
Klasse erstellen wollen.

.. tip::
   `cusy Seminar: Fortgeschrittenes Python
   <https://cusy.io/de/unsere-schulungsangebote/fortgeschrittenes-python>`_

.. tip::
   Für Tabellendaten verwende ich im Allgemeinen :doc:`pandas Series oder
   DataFrames <Python4DataScience:workspace/pandas/data-structures>` und wenn
   ich Matrizen mit Zahlen speichern muss, verwende ich :doc:`Numpy
   <Python4DataScience:workspace/numpy/index>`.

Nehmen wir an, wir wollen eine Klasse speichern, die ein Item repräsentiert mit
``summary``, ``owner``, ``state`` und ``id``. Wir können eine solche Klasse
definieren mit:

.. code-block:: pycon

   >>> from dataclasses import dataclass
   >>> @dataclass
   ... class Item:
   ...     summary: str = None
   ...     owner: str = None
   ...     state: str = "todo"
   ...     id: int = None
   ...

Der ``@dataclass``-Dekorator erstellt die ``__init__``- und
``__repr__``-Methoden. Wenn ich mir die Instanz der Klasse ausgeben lasse,
erhalte ich den Klassennamen und die Attribute:

.. code-block:: pycon

   >>> i1
   Item(summary='My first item', owner='veit', state='todo', id=1)

Im Allgemeinen werden Datenklassen als syntaktischer Zucker für die Erstellung
von Klassen, die Daten speichern, verwendet. Ihr könnt euren Klassen
zusätzliche Funktionalität verleihen, indem ihr Methoden definiert. Wir werden
der Klasse eine Methode hinzufügen, die ein Item-Objekt aus einem :doc:`Dict
<types/dicts>` erstellt:

.. code-block:: pycon

   >>> @dataclass
   ... class Item:
   ...     ...
   ...     @classmethod
   ...     def from_dict(cls, d):
   ...         return Item(**d)
   ...
   >>> item_dict = {
   ...     "summary": "My first item",
   ...     "owner": "veit",
   ...     "state": "todo",
   ...     "id": 1,
   ... }
   >>> Item.from_dict(item_dict)
   Item(summary='My first item', owner='veit', state='todo', id=1)
