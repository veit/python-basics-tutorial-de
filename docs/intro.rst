Einführung
==========

Über Python
-----------

Vielleicht stellt ihr euch die Frage, warum ihr Python lernen solltet. Es gibt
viele Programmiersprachen von C und C++ über Java bis hin zu Lua und Go.

.. figure:: tiobe-index.svg
   :alt: TIOBE Index für Juni 2024

   `TIOBE Index für Juni 2024 <https://www.tiobe.com/tiobe-index/>`_

Python
hat eine sehr große Verbreitung gefunden und einer der Gründe dürfte sein, dass
sie auf vielen verschiedenen Plattformen läuft, von IoT-Geräten über die
gängigen Betriebssysteme bis hin zu Supercomputern. Es kann gut zur Entwicklung
kleiner Anwendungen und schneller Prototypen verwendet werden. Dabei gibt es
unzählige Software-Bibliotheken, die euch die Arbeit erleichtern.

Python ist eine moderne Programmiersprache, die von Guido van Rossum in den 90er
Jahren entwickelt wurde.

.. seealso::
    * Lambert Meertens: `The Origins of Python
      <https://inference-review.com/article/the-origins-of-python>`_

Einige Stärken von Python sind:

leichte Nutzbarkeit
    Einige der Gründe hierfür sind, dass Typen mit Objekten verbunden sind,
    nicht mit Variablen; einer Variablen können Werte eines beliebigen Typs
    zugewiesen werden und eine Liste kann Objekte verschiedener Typen enthalten.
    Zudem sind die Syntaxregeln sehr einfach und ihr könnt schnell lernen,
    nützlichen Code zu schreiben.

    .. figure:: python.png
       :alt: Python – and programming is fun again!

       `XKCD: Python <https://xkcd.com/353>`_

Ausdrucksstärke
    Häufig könnt ihr in wenigen Zeilen Code sehr viel mehr erreichen als in
    anderen Sprachen. Dies führt dazu, dass ihr eure Projekte schneller
    abschließen könnt und auch Fehlersuche und Wartung sehr vereinfacht werden.
Lesbarkeit
    Die leichte Lesbarkeit von Python-Code vereinfacht die Fehlersuche und
    Wartung. Dies erreicht Python :abbr:`u.a. (unter anderem)` durch
    erforderliche Einrückungen.
Vollständigkeit
    Mit der Installation von Python ist bereits alles wesentliche  vorhanden,
    was für das Programmieren mit Python benötigt wird, E-Mails, Websites,
    Datenbanken, ohne dass zusätzliche Bibliotheken installiert werden müssen.
Plattformunabhängigkeit
    Python läuft auf vielen Plattformen: Windows, Mac, Linux etc. Es gibt sogar
    Varianten, die auf Java (`Jython <https://www.jython.org/>`_) und .NET
    (`IronPython <https://ironpython.net/>`_) laufen.
Open Source
    Ihr könnt Python herunterladen und für die Entwicklung kommerzieller oder
    privater Anwendungen frei verwenden. Dabei wird Python von vielen
    etablierten Unternehmen genutzt und gefördert, :abbr:`u.a. (unter anderem)`
    von Google, Meta und Bloomberg. Und wenn ihr etwas zurückgeben wollt, könnt
    ihr dies ebenfalls gerne machen : `Python Software Foundation Sponsorship
    <https://www.python.org/psf/sponsorship/>`_.

Python hat zwar einige Vorteile, aber keine Sprache ist in allen Bereichen
die beste Lösung. So schneidet :abbr:`z.B. (zum Beispiel)` Python in den
folgenden Bereichen weniger gut ab:

Geschwindigkeit
    Python ist keine vollständig kompilierte Sprache und der Code wird zunächst
    in Bytecode kompiliert bevor er vom Python-Interpreter ausgeführt wird. Zwar
    gibt es einige Aufgaben, wie :abbr:`z.B. (zum Beispiel)` das Parsen von
    Zeichenketten mit regulären Ausdrücken, für die Python effiziente
    Implementierungen bereitstellt, und die genauso schnell wie ein C-Programm
    sind, dennoch werden Python-Programme in den meisten Fällen langsamer sein
    als C-Programme. Dies spielt jedoch selten eine entscheidende Rolle, da es
    bereits viele Python-Module gibt, die intern C verwenden.

    .. seealso::
        * :doc:`Python4DataScience:performance/index`

Bibliotheksvielfalt
    Python verfügt zwar bereits über sehr viele Bibliotheken, in einigen Fällen
    werdet ihr jedoch passende Bibliotheken nur in anderen Sprachen finden. Für
    die meisten Probleme, die programmatisch gelöst werden sollen, ist die
    Bibliotheksunterstützung von Python jedoch hervorragend.
Variablentypen
    Anders als in vielen anderen Sprachen sind Variablen keine Container,
    sondern eher Etiketten, die auf verschiedene Objekte verweisen: Ganzzahlen,
    Zeichenketten, Klasseninstanzen und vieles mehr. Manche empfinden es als
    Nachteil, dass Python hier nicht einfach eine Typ-Validierung durchführt,
    aber die Anzahl der Typ-Fehler ist meist überschaubar und die Flexibilität
    der dynamischen Typisierung wiegt die Probleme meist auf.
Unterstützung für mobile Geräte
    Es gibt mittlerweile einige Optionen, Python auf mobilen Geräten laufen zu
    lassen, :abbr:`z.B. (zum Beispiel)` mit :ref:`briefcase` oder
    :ref:`beeware`. Python selbst wird ab Version 3.13 `Tier 3
    <https://peps.python.org/pep-0011/#tier-3>`_ :abbr:`u.a. (unter anderem)`
    für Windows, iOS und Pi OS bieten. Zudem soll es zukünftig einfacher werden,
    :ref:`wheels` auch für mobile Endgeräte zu erstellen, indem Tools wie
    :doc:`packs/cibuildwheel` und :term:`setuptools` erweitert werden.

    .. seealso::
       * `The Python Language Summit 2024: Python on Mobile
         <https://pyfound.blogspot.com/2024/06/python-language-summit-2024-python-on-mobile.html>`_
       * :pep:`730`
       * :pep:`738`

Unterstützung für nebenläufige Berechnungen
    Prozessoren mit mehreren Kernen sind inzwischen weit verbreitet und führen
    in vielen Bereichen zu erheblichen Leistungssteigerungen. Die
    Standardimplementierung von Python ist jedoch nicht für die Nutzung mehrerer
    Kerne ausgelegt.

    .. seealso::
        * :doc:`Python4DataScience:performance/multiprocessing-threading-async`
