Mock
====

In diesem Kapitel werden wir die :abbr:`CLI (Befehlszeilenschnittstelle)`
testen. Hierfür werden wir die :doc:`mock
<python3:library/unittest.mock>`-Bibliothek verwenden, das seit Python 3.3 als
Teil der Python-Standardbibliothek unter dem Namen ``unittest.mock``
ausgeliefert wird.

Objekte, die nicht real sind, können entweder :term:`Dummies <Dummy>`,
:term:`Fakes <Fake>`, :term:`Stubs <Stub>`, :term:`Mocks <Mock>` oder
:term:`Spies <Spy>` sein. Sie sind alle :abbr:`sog. (sogenannte)` Test-Doubles.
Mit dem pytest-eigenen :ref:`monkeypatch-fixture`-Fixture und
:doc:`unittest.mock <python3:library/unittest.mock>` solltet ihr jedoch über
alle Funktionen verfügen, die ihr benötigt.

Die drei Kernfunktionalitäten von :doc:`unittest.mock
<python3:library/unittest.mock>` sind:

:class:`Mock <python3:unittest.mock.Mock>`
    Die Mock-Klasse kann zur Simulation eines beliebigen Objekts verwendet
    werden.
:class:`MagickMock <python3:unittest.mock.MagicMock>`
    Unterklasse von Mock, die alle magischen Methoden enthält, :abbr:`z. B. (zum
    Beispiel)` ``__str__``, ``__len__`` :abbr:`usw (und so weiter)`.
:func:`patch <python3:unittest.mock.patch>`-Methode
    In einem bestimmten Modul wird ein Objekt gesucht und ersetzt durch ein
    anderes Objekt.

Wir werden uns im Folgenden das Mocking von Rückgabewerten, die Überprüfen von
Mock-Funktionen-Aufrufen von und das Mocking von Exceptions anschauen. Es gibt
jedoch noch eine ganze Reihe weiterer Mocking-Techniken, die wir nicht behandeln
werden. Lest also unbedingt :doc:`python3:library/unittest.mock`, wenn ihr
Mocking ausgiebig nutzen möchtet.

.. toctree::
   :titlesonly:
   :hidden:

   examples
   spies
   autospec
   assert_called
   limitations
   plugins
