Abstrakte Fabrik
================

.. warning::
   Die abstrakte Fabrik ist eine ungeschickte Lösung für das Fehlen von
   erstklassigen Funktionen und Klassen in weniger leistungsfähigen
   Programmiersprachen. Sie passt schlecht zu Python, wo wir stattdessen
   einfach eine Klasse oder eine Factory-Funktion übergeben können, wenn eine
   Bibliothek Objekte in unserem Namen erstellen muss.

   – Brandon Rhodes: `The Abstract Factory Pattern
   <https://python-patterns.guide/gang-of-four/abstract-factory/>`_

Das :mod:`python3:json`-Modul der Python-Standardbibliothek ist ein gutes
Beispiel für eine Bibliothek, die Objekte im Namen ihres caller instanziieren
muss. Betrachten wir einen JSON-String wie diesen:

.. literalinclude:: books.json
   :language: json

Normalerweise erzeugt die Funktion :func:`python3:json.load` des
:mod:`python3:json`-Moduls Unicode-Objekte für die Zeichenketten und ein Dict
für das JSON-Objekt der obersten Ebene.

Für das Veröffentlichungsdatum ist dieser Standardwert jedoch nicht
zufriedenstellend, sodass wir ihn in ein Datumsformat umwandeln wollen:

.. code-block:: pycon
   :emphasize-lines: 2-4, 8, 11

    >>> import json
    >>> from datetime import datetime
    >>> def convert_date(string):
    ...     return datetime.strptime(string, "%Y-%m-%d").date()
    ...
    >>> with open("books.json") as f:
    ...     books = json.load(f)
    ...     books["Publication date"] = convert_date(books["Publication date"])
    ...     print(books)
    ...
    {'Title': 'Python basics', 'Language': 'en', 'Authors': 'Veit Schiele', 'License': 'BSD-3-Clause', 'Publication date': datetime.date(2021, 10, 28)}

Diese einfache Fabrik wurde erfolgreich ausgeführt: das zurückgegebene Datum ist
vom Typ ``datetime.date``.

.. note::
   Ich habe das Verb :func:`convert_date` als Namen für diese Funktion gewählt,
   und nicht ein Substantiv wie :func:`date_factory`, da er ausdrückt, was die
   Funktion tut, anstatt mir zu sagen, was für eine Art von Funtion sie ist.

Einige Legacy-Sprachen unterstützen nur die Übergabe von Klasseninstanzen, nicht
auch aufrufbare Funktionen. Mit dieser Einschränkung müsste jede einfache Fabrik
von einer Funktion zu einer Methode werden:

.. code-block:: python

   class DateFactory(object):
       @staticmethod
       def build_date(dict):
           dict["Publication date"] = datetime.strptime(
               dict["Publication date"], "%Y-%m-%d"
           ).date()

In traditioneller objektorientierter Programmierung ist das Wort *Factory* der
Name für einee Art von Klasse, die eine Methode anbietet, mit der ein Objekt
erstellt wird. Wenn wir eine Python-Klasse nicht direkt übergeben könnten,
sondern lediglich Objektinstanzen, könnte die Klasse :class:`DateFactory` nicht
als Argument an die Methode :func:`load` übergeben werden. Stattdessen müsste
:class:`DateFactory` unnötigerweise instanziiert und anschließend das
resultierende Objekt übergeben werden:

.. code-block:: python
   :emphasize-lines: 6

   class Loader(object):
       @staticmethod
       def load(books_file, factory):
           with open(books_file) as f:
               books = json.load(f)
               factory.build_date(books)
               return books

.. code-block:: pycon

   >>> df = DateFactory()
   >>> b = Loader.load("books.json", df)
   >>> print(b)
   {'Title': 'Python basics', 'Language': 'en', 'Authors': 'Veit Schiele', 'License': 'BSD-3-Clause', 'Publication date': datetime.date(2021, 10, 28)}

.. note::
   #. Da Python-Klassen statische und Klassenmethoden bieten, die ohne Instanz
      aufgerufen werden können, müssen wir die :class:`DateFactory`-Klasse nicht
      erst instanziieren – wir können sie einfach als Objekt übergeben.
   #. Sprachen, die euch zwingen, den Typ jedes Methodenparameters im Voraus zu
      deklarieren, schränken eure zukünftigen Möglichkeiten übermäßig ein.

Schließlich soll m Entwurfsmuster *Abstrakte Fabrik* die Spezifikation von der
Implementierung getrennt werden, indem eine abstrakte Klasse erstellt wird. Eure
abstrakte Klasse würde lediglich versprechen, dass das
:class:`DateFactory`-Argument für :func:`load` eine Klasse sein wird, die der
erforderlichen Schnittstelle entspricht:

.. code-block:: python

   from abc import ABCMeta, abstractmethod


   class AbstractFactory(metaclass=ABCMeta):

       @abstractmethod
       def build_date(self, dict):
           pass

Sobald die abstrakte Klasse vorhanden ist und :class:`DateFactory` von ihr erbt,
sind die Vorgänge, die zur Laufzeit ablaufen, jedoch genau dieselben wie zuvor.
Die Methoden der :class:`DateFactory` werden mit verschiedenen Argumenten
aufgerufen, die sie anweisen, verschiedene Arten von Objekten zu erstellen, ohne
dass der Aufrufer die Details kennen muss.
