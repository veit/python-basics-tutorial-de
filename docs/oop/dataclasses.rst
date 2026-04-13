``dataclasses``
===============

:doc:`dataclasses <python3:library/dataclasses>` wurden in Python 3.7 eingeführt und sind
ein spezieller Shortcut, mit der wir Klassen erstellen können, die Daten speichern. Python
bietet einen speziellen :doc:`Dekorator <../functions/decorators>`, wenn wir eine solche
Klasse erstellen wollen.

.. tip::
   Für Tabellendaten verwende ich im Allgemeinen :doc:`pandas Series oder
   DataFrames <Python4DataScience:workspace/pandas/data-structures>` und wenn
   ich Matrizen mit Zahlen speichern muss, verwende ich :doc:`NumPy
   <Python4DataScience:workspace/numpy/index>`.

Nehmen wir an, wir wollen eine Klasse speichern, die einen Task repräsentiert
mit ``summary``, ``owner``, ``state`` und ``id``. Wir können eine solche Klasse
definieren mit:

.. code-block:: pycon

   >>> from dataclasses import dataclass
   >>> @dataclass
   ... class Task:
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
   Task(summary='My first task', owner='veit', state='todo', id=1)

Im Allgemeinen werden Datenklassen als syntaktischer Zucker für die Erstellung
von Klassen, die Daten speichern, verwendet. Ihr könnt euren Klassen
zusätzliche Funktionalität verleihen, indem ihr Methoden definiert. Wir werden
der Klasse eine Methode hinzufügen, die ein Task-Objekt aus einem :doc:`Dict
<../types/dicts>` erstellt:

.. code-block:: pycon

   >>> @dataclass
   ... class Task:
   ...     ...
   ...     @classmethod
   ...     def from_dict(cls, d):
   ...         return Task(**d)
   ...
   >>> task_dict = {
   ...     "summary": "My first task",
   ...     "owner": "veit",
   ...     "state": "todo",
   ...     "id": 1,
   ... }
   >>> Task.from_dict(task_dict)
   Task(summary='My first task', owner='veit', state='todo', id=1)

.. tip::
   `cusy Seminar: Fortgeschrittenes Python
   <https://cusy.io/de/our-training-courses/advanced-python.html>`_
